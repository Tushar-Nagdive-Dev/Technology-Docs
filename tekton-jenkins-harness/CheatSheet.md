# quick mental model

* **jenkins** = general automation server (groovy pipelines, huge plugin eco).
* **tekton** = k8s-native CI/CD (CRDs: Task/TaskRun, Pipeline/PipelineRun, Triggers).
* **harness** = SaaS CI/CD with built-in verification, RBAC, governance, templates.

---

# golden rules (apply everywhere)

* keep pipelines **declarative, idempotent, and fast** (≤15 min end-to-end).
* **fail fast** (tests, vuln scans, policy checks up front).
* **immutable artifacts** (tag by commit SHA; never mutate same tag).
* everything as code: pipeline, infra, policies, dashboards.
* one source of truth: **git** (pipelines, helm charts, env configs).
* **separate concerns**: CI (build/test) vs CD (deploy/verify).
* **shift left security** (SAST/SBOM/vulns in CI; verify signatures in CD).
* **observability by default** (logs, metrics, traces; alerts wired).

---

# repo structure (baseline)

```
.
├─ app/ (src, tests)
├─ Dockerfile
├─ helm/ (chart for k8s)
├─ .ci/
│  ├─ Jenkinsfile
│  ├─ tekton/ (tasks, pipelines, triggers)
│  └─ harness/ (pipelines, services, envs)
└─ scripts/ (lint, test, build, deploy, rollback)
```

---

# webhooks & triggers

* **jenkins**: GitHub webhook → `http://<jenkins>/github-webhook/`; job trigger: *GitHub hook trigger for GITScm polling*.
* **tekton**: Triggers (`EventListener` + `TriggerBinding` + `TriggerTemplate`) → webhook to EL service URL.
* **harness**: Pipeline **Trigger** on push / PR / artifact update.

---

# quality gates (minimal)

* unit tests + coverage ≥ **80%** (jacoco or coverage tool).
* linters (language-specific).
* SBOM (syft) generated.
* vulnerability scan (trivy) **fail on HIGH/CRITICAL**.
* policy checks (OPA/Rego) before deploy.

---

# artifacts & images

* tag images as: `repo/name:<git-sha>` (+ optional build number).
* push to registry (DockerHub/GHCR/ECR). enable **content trust/signing**.
* **promotion by digest** (not mutable tags) across envs.

---

# deployment strategies (k8s)

* **rolling** (default).
* **blue-green**: `svc` selector flips between `color=blue|green`.
* **canary**: weighted rollout (10%→50%→100%) with metric gates.

---

# secrets

* **jenkins**: Credentials store → `withCredentials`.
* **tekton**: K8s Secret + SA + `volumeMount/envFrom`.
* **harness**: Secrets Manager (text/file/SSH) → `${secrets.getValue()}`.
* prefer external managers (Vault/ASM/SM) injected at runtime.

---

# caching & speed

* cache deps (`~/.m2`, `node_modules`, build caches).
* **jenkins**: `stash/unstash`, persistent workspace, matrix/parallel where worth it.
* **tekton**: `workspaces` on PVC; reuse task images; parallel tasks (no `runAfter`).
* **harness**: Cache Intelligence for language ecosystems.
* profile slow stages; keep pipelines **under 200 lines**—factor into scripts/libs.

---

# reusability

* **jenkins**: Shared Libraries (`vars/` functions), JCasC for controller config.
* **tekton**: Catalog tasks (`git-clone`, `maven`, `kaniko`) + your own task yamls.
* **harness**: Stage/Step/Pipeline **templates** with inputs; org-level governance.

---

# security & compliance (minimum viable)

* **CI**: SAST, dependency scan, SBOM, sign images (cosign).
* **CD**: verify signature, policy gate (no deploy if CVEs ≥ HIGH).
* store SBOM + scan reports as pipeline artifacts for audits.

---

# observability & alerts

* **jenkins**: Prometheus plugin → Grafana; Slack/Teams on failure.
* **tekton**: Tekton Dashboard; controller metrics → Prometheus; `finally` steps for notifications.
* **harness**: built-in CV (Prometheus/Datadog/NewRelic); health score gates; auto-rollback.

---

# HA/DR (quick)

* **jenkins**: active-passive controllers; `JENKINS_HOME` on resilient storage; nightly backups; k8s agents.
* **tekton**: replicate controllers; GitOps for CRDs; PVC snapshots for logs/artifacts.
* **harness**: ≥2 delegates/cluster; export pipeline YAMLs to git.

---

# multi-env promotion (dev → staging → prod)

* **rule**: same artifact digest promoted; config via values/env vars only.
* approvals required for prod; record who/what/when (audit trail).
* smoke test + quick synthetic after every deploy; rollback path documented.

---

# multi-service patterns

* multibranch per repo; OR monorepo with **path filters** to build only changed modules.
* parallelize independent services; serialize shared DB migrations w/ locks.

---

# cost controls

* ephemeral agents/runners; set resource requests/limits.
* artifact/log retention (last N builds).
* prune old image tags.
* concurrency caps; nightly cleanup jobs.

---

# minimal snippets (copy-paste)

## Jenkinsfile (java/maven baseline)

```groovy
pipeline {
  agent any
  options { timeout(time: 15, unit: 'MINUTES'); ansiColor('xterm') }
  environment {
    IMAGE = 'org/app'
    TAG = "${env.GIT_COMMIT.take(12)}"
  }
  stages {
    stage('Checkout'){ steps { checkout scm } }
    stage('Build & Test'){ steps { sh 'mvn -B -DskipTests=false clean verify' } }
    stage('Coverage Gate'){ steps { junit '**/surefire-reports/*.xml' } }
    stage('SBOM & Scan'){
      steps {
        sh 'syft . -o json > sbom.json'
        sh "trivy image --exit-code 1 --severity HIGH ${IMAGE}:${TAG} || true"
      }
    }
    stage('Docker Build & Push'){
      steps {
        withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'U', passwordVariable: 'P')]) {
          sh """
            echo $P | docker login -u $U --password-stdin
            docker build -t ${IMAGE}:${TAG} .
            docker push ${IMAGE}:${TAG}
          """
        }
      }
    }
    stage('Deploy (Helm)'){
      steps {
        sh "helm upgrade --install app ./helm --set image.repository=${IMAGE} --set image.tag=${TAG}"
      }
    }
    stage('Smoke'){ steps { sh 'curl -fsS http://app.namespace.svc.cluster.local:8080/actuator/health' } }
  }
  post { failure { sh 'helm rollback app 1 || true' } }
}
```

## Tekton (ultra-short skeleton)

```yaml
# task.yaml
apiVersion: tekton.dev/v1
kind: Task
metadata: { name: build-test }
spec:
  workspaces: [{ name: source }, { name: maven } ]
  steps:
    - name: build
      image: maven:3.9-eclipse-temurin-17
      workingDir: /workspace/source
      script: |
        mvn -Dmaven.repo.local=/workspace/maven/.m2 clean verify
# pipeline.yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata: { name: ci }
spec:
  workspaces: [{ name: source }, { name: maven }]
  tasks:
    - name: build
      taskRef: { name: build-test }
# trigger bits: EventListener + Binding + Template → create PipelineRun on push
```

## Harness (what to remember)

* create **Connectors** (Git, Registry, K8s).
* CI stage: build/test → SBOM/scan → docker push.
* CD stage: Helm/K8s deploy; **strategy** (rolling/blue-green/canary) + **verification**.
* everything saved as YAML templates; lock critical steps with RBAC.

---

# policy-as-code (quick Rego idea)

```rego
package cicd

deny[msg] {
  input.sbom.missing == true
  msg := "SBOM required"
}

deny[msg] {
  some cve
  input.vulns[cve].severity == "HIGH"
  msg := sprintf("High CVE: %v", [cve])
}
```

---

# common pitfalls (avoid)

* mutable `latest` tags → use commit SHA.
* secrets in env/plain YAML → use credential stores.
* massive pipelines doing “everything” → split CI vs CD; reuse libs/tasks.
* no rollback plan → always script `helm rollback`/previous artifact deploy.
* ignoring flakiness → add retries w/ backoff, quarantine tests, fix root causes.

---

# go-live checklist (10 items)

1. repo has **Dockerfile**, **helm chart**, **pipeline code**
2. webhooks configured & healthy
3. tests + coverage gate enabled
4. SBOM + vuln scan running
5. images **signed**; CD **verifies** signature
6. prod requires approval; audit trail on
7. smoke tests & rollback scripted
8. metrics/logs/traces wired + alerts
9. caching + parallel tuned; build ≤15 min
10. backups for pipeline state & configs (JCasC / Tekton YAML / Harness YAML)
