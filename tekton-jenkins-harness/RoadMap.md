# Mastery Roadmap (from beginner → expert)

## Phase 0 — Grounding (Day 1–2)

* **Core ideas:** CI vs CD, pipelines, stages, artifacts, environments, approvals, rollbacks, Git branching, semantic versioning, trunk vs GitFlow, DORA metrics.
* **Platform basics to prepare:** Docker fundamentals, Kubernetes 101 (Pods, Services, Namespaces, CRDs), YAML & JSON, GitHub/GitLab webhooks.
* **Outcome:** Shared vocabulary + a working K8s playground (kind/minikube) and a sample app (REST + DB).

## Phase 1 — Jenkins Fundamentals (Week 1)

* Install & tour Jenkins; create first **Declarative** pipeline with a `Jenkinsfile` (build → test → package).
* **Pipeline Syntax** & steps; agents, stages, post conditions, credentials, parameters. ([Jenkins][1])
* **Multibranch Pipelines**; Webhooks; Artifacts.
* **Exercises:** build + unit test + JUnit report + archive artifact for your sample app.

## Phase 2 — Tekton Fundamentals (Week 2)

* Install **Tekton Pipelines** on your cluster; understand CRDs: `Task`, `TaskRun`, `Pipeline`, `PipelineRun`, `Workspace`, `Params`, `Results`. Use `tkn` CLI. ([Tekton][2], [GitHub][3])
* Add **Tekton Triggers**: `EventListener`, `TriggerBinding`, `TriggerTemplate`, interceptors; wire to Git webhooks. ([Tekton][4])
* **Exercises:** reproduce the Jenkins build/test/package with Tekton; trigger on PRs.

## Phase 3 — Harness Fundamentals (Week 3)

* Navigate **Harness Developer Hub**. Create org/project, connectors (Git, Docker, K8s), secrets, and your first **CI pipeline**. ([Harness][5])
* Build **CD pipeline modeling**: services, environments, stages, strategies (Rolling/Blue-Green/Canary), approvals, and GitOps view. ([Harness][6])
* **Exercises:** CI (build + cache + test + publish image), then CD (deploy to dev & staging with approval to prod).

## Phase 4 — Intermediate Patterns (Week 4)

* **Quality gates:** SAST/DAST, license scanning, SBOM; fail-the-build conditions.
* **Caching & parallelism:** Jenkins (caches & matrix), Tekton (workspaces, PVCs), Harness (cache intelligence). ([Jenkins][7])
* **Artifacts & registries:** DockerHub/GHCR/Artifactory; immutability & retention.
* **Observability:** test reports, coverage, build times; DORA in Harness. ([Harness][8])
* **Exercises:** add unit test, coverage gate, SBOM upload; speed up builds via caching.

## Phase 5 — Enterprise Practices (Week 5)

* **Jenkins at scale:** shared libraries, folder/seed jobs, **Configuration as Code (JCasC)**, controller/agent architecture, Kubernetes agents. ([Jenkins][9], [Jenkins Plugins][10])
* **Tekton at scale:** reusable `Tasks`, catalogs, workspaces, **Triggers** security (interceptors & secrets), governance. ([Tekton][4], [Red Hat][11])
* **Harness at scale:** templates, governance/RBAC, policy-as-code, deployment verification.
* **Supply chain security:** signed images, provenance, PR-to-prod traceability.

## Phase 6 — Advanced & Cloud-Native (Week 6)

* **GitOps** (progressive delivery, canaries), feature flags, drift detection.
* **Release strategies:** Blue-Green, Canary with automated verification.
* **Policy & compliance:** OPA/Conftest hooks; audit trails; secrets management.
* **Disaster recovery:** backups, stateless controllers, restoring pipelines quickly.

## Capstone Project (end of Week 6)

**One app, three pipelines:**

1. Jenkins with Shared Library + JCasC
2. Tekton Pipelines + Triggers (webhook-driven)
3. Harness CI/CD with verification & approval
   Compare build time, failure modes, rollback speed, and developer UX.

---

# What you’ll build throughout

* A microservice (REST + DB + tests) containerized with Docker.
* A Helm/K8s deployment for dev/staging/prod.
* CI logic for lint/unit/integration, caching, SBOM.
* CD logic for progressive delivery + approvals + rollback.

---

# Common mistakes (and how we’ll avoid them)

* **Mixing infra & app concerns** in one giant pipeline → we’ll modularize stages and reuse via Jenkins Shared Libraries and Tekton Tasks. ([Jenkins][9])
* **Snowflake Jenkins** → we’ll use **JCasC** and keep config in Git. ([Jenkins][12])
* **Webhook chaos** in Tekton → we’ll use Triggers + interceptors + secrets correctly. ([Tekton][4], [Red Hat][11])
* **Click-ops in Harness** → we’ll use YAML pipelines, templates, and RBAC. ([Harness][13])

---

# Resources you’ll use (authoritative)

* **Tekton docs:** Pipelines, Triggers, CLI (`tkn`) + API spec. ([Tekton][2], [GitHub][14])
* **Jenkins docs:** Pipeline (Declarative/Scripted), Syntax ref, Shared Libraries, JCasC. ([Jenkins][1])
* **Harness docs:** Developer Hub, CI/CD modeling, Pipelines in YAML. ([Harness][5])

---

# Your first lesson (start now)

## Lesson 1 — Set up your playground (≈90 minutes)

### 1) Local cluster + sample app

* Install **Docker** and **kind** (or minikube). Create a cluster:
  `kind create cluster --name cicd-lab`
* Clone a simple sample app (your favorite language) with tests; add a Dockerfile and a Helm chart (we’ll refine later).

### 2) Jenkins quickstart

* Run Jenkins in Docker:

  ```bash
  docker run -p 8080:8080 -p 50000:50000 --name jenkins \
    -v jenkins_home:/var/jenkins_home jenkins/jenkins:lts
  ```
* Create a **Declarative** `Jenkinsfile` in your repo:

  ```groovy
  pipeline {
    agent any
    stages {
      stage('Checkout'){ steps { checkout scm } }
      stage('Build'){ steps { sh 'make build || ./gradlew build || mvn -B -DskipTests package' } }
      stage('Test'){ steps { sh 'make test || ./gradlew test || mvn -B test' } }
      stage('Package'){ steps { archiveArtifacts artifacts: '**/build/** || **/target/**', fingerprint: true } }
    }
    post { always { junit '**/test-results/**/*.xml, **/surefire-reports/*.xml' } }
  }
  ```
* Push to Git; create a Pipeline job pointing to the repo.
* **Reference for syntax/steps:** Jenkins Pipeline book & syntax guide. ([Jenkins][1])

### 3) Tekton quickstart

* Install **Tekton Pipelines** (via operator or manifests) and **tkn** CLI. ([Tekton][2])
* Create a `Task` and `Pipeline` that mimic your Jenkins stages (build/test/package).
* Run with `tkn pipeline start ...`.
* Add **Tekton Triggers** later this week to fire on Git webhooks. ([Tekton][4])

### 4) Harness quickstart

* Create a free Harness project; add connectors (Git, Docker registry), and your first **CI pipeline** that builds + tests the app. ([Harness][5])

### Success criteria for Lesson 1

* Jenkins job runs green with archived artifacts.
* Tekton PipelineRun completes locally.
* Harness CI job builds/tests successfully.

---

# Exercises for the week

**E1. Jenkins basics**

* Add parameters (e.g., `BRANCH`, `RUN_TESTS=true/false`), a timeout, and a `post { failure { ... } }` Slack/Teams notification. Use the syntax guide. ([Jenkins][7])

**E2. Tekton basics**

* Pass inputs via `params`, share data with `workspaces`, and output a `result`. List & inspect with `tkn`. ([Tekton][2])

**E3. Harness basics**

* Convert your CI pipeline to YAML and commit it; add a cache step to speed up builds. ([Harness][13])

---

# How we’ll learn (your study loop)

1. **Concept** → 2) **Tiny demo** → 3) **Guided exercise** → 4) **Review & improve** → 5) **Automate & scale**.

If you’re ready, we’ll jump to **Lesson 2** next: *“Quality gates (tests, coverage), caching, and artifact publishing in all three tools.”*

[1]: https://www.jenkins.io/doc/book/pipeline/?utm_source=chatgpt.com "Pipeline"
[2]: https://tekton.dev/docs/pipelines/?utm_source=chatgpt.com "Tasks and Pipelines | Tekton"
[3]: https://github.com/tektoncd/cli?utm_source=chatgpt.com "tektoncd/cli: A CLI for interacting with Tekton!"
[4]: https://tekton.dev/docs/triggers/?utm_source=chatgpt.com "Triggers and EventListeners - Tekton"
[5]: https://developer.harness.io/docs/?utm_source=chatgpt.com "Harness Documentation"
[6]: https://developer.harness.io/docs/continuous-delivery/get-started/cd-pipeline-modeling-overview/?utm_source=chatgpt.com "CD pipeline modeling overview"
[7]: https://www.jenkins.io/doc/book/pipeline/syntax/?utm_source=chatgpt.com "Pipeline Syntax"
[8]: https://developer.harness.io/docs/continuous-delivery/?utm_source=chatgpt.com "Continuous Delivery & GitOps Documentation"
[9]: https://www.jenkins.io/doc/book/pipeline/shared-libraries/?utm_source=chatgpt.com "Extending with Shared Libraries"
[10]: https://plugins.jenkins.io/configuration-as-code/?utm_source=chatgpt.com "Configuration as Code | Jenkins plugin"
[11]: https://www.redhat.com/en/blog/filtering-tekton-trigger-operations?utm_source=chatgpt.com "Filtering Tekton trigger operations"
[12]: https://www.jenkins.io/doc/book/managing/casc/?utm_source=chatgpt.com "Configuration as Code"
[13]: https://developer.harness.io/docs/category/pipelines/?utm_source=chatgpt.com "Pipelines"
[14]: https://github.com/tektoncd/pipeline/blob/main/docs/api-spec.md?utm_source=chatgpt.com "pipeline/docs/api-spec.md at main · tektoncd ..."
