1. Enforce **policy-as-code** in Jenkins, Tekton, and Harness.
2. Maintain **audit trails** for every CI/CD action.
3. Apply **security & compliance checks** automatically before deployments.

---

## **Step 1 — Why Governance Matters**

In enterprise CI/CD:

* **Governance** ensures pipelines follow organizational rules (security, naming, approval).
* **Compliance** ensures releases meet regulatory requirements (GDPR, SOC 2, ISO 27001).
* Without this, you risk **security breaches, legal issues, and failed audits**.

---

## **Step 2 — Policy-as-Code**

### 2.1 Jenkins

* Use **Jenkins Job DSL** or **Configuration as Code (JCasC)** so job definitions live in Git.
* Use **OPA (Open Policy Agent)** in a pipeline step to validate rules:

```groovy
stage('Policy Check') {
    steps {
        sh "opa eval --data policies/ --input build.json 'data.cicd.allow'"
    }
}
```

### 2.2 Tekton

* Install **OPA Gatekeeper** in your Kubernetes cluster.
* Define **ConstraintTemplates** to block:

  * Pipelines without SBOM generation.
  * Deployments missing approval annotations.
* Tekton admission webhooks reject non-compliant PipelineRuns.

### 2.3 Harness

* Use **Governance Policies** (OPA/Rego-based) directly in the UI.
* Examples:

  * Block deployment to prod if CVE severity > High.
  * Require approval from security group before rollout.

---

## **Step 3 — Audit Trails**

### Jenkins

* Enable **Audit Trail Plugin** → logs all user actions & changes to jobs.
* Store logs in ELK/Loki for long-term retention.

### Tekton

* Use Kubernetes **Audit Logs** to track CRD changes (PipelineRun, TaskRun).
* Store in centralized logging system.

### Harness

* Built-in audit logs in **Account Settings → Audit Trail**.
* Searchable by user, service, environment.

---

## **Step 4 — Security Checks Before Deployment**

### Security Steps to Enforce:

1. **SBOM Check** — must exist in artifacts.
2. **Vulnerability Scan** — fail on HIGH/CRITICAL.
3. **Signature Verification** — only deploy signed images.
4. **Approval Check** — require manual approval for sensitive environments.

### Enforcement Example — Jenkins:

```groovy
stage('Pre-Deploy Checks') {
    steps {
        sh 'test -f sbom.json || exit 1'
        sh 'trivy image --exit-code 1 --severity HIGH $DOCKER_IMAGE:$DOCKER_TAG'
        sh 'cosign verify --key cosign.pub $DOCKER_IMAGE:$DOCKER_TAG'
    }
}
```

---

## **Step 5 — Role-Based Access Control (RBAC)**

* **Jenkins**:

  * Use **Matrix-based security** to restrict pipeline edits.
* **Tekton**:

  * Separate service accounts per environment.
* **Harness**:

  * Define roles (Dev, QA, Ops) with least-privilege principle.

---

## **Step 6 — Compliance Reporting**

* **Jenkins**:

  * Export build metadata to compliance DB.
* **Tekton**:

  * Store pipeline run results in persistent store.
* **Harness**:

  * Built-in deployment reports for audits.

---

## ✅ Lesson 10 Completion Criteria

* Policies are enforced automatically before deployment.
* Every action is logged and searchable.
* RBAC ensures least-privilege access.
* Compliance checks block non-compliant builds.
