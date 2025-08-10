1. Manage **multiple microservices** in a single CI/CD setup.
2. Handle **multiple environments** (dev, staging, prod) with approvals.
3. Share pipelines and infrastructure efficiently across services.

---

## **Step 1 — Multi-Service CI/CD Challenges**

When you go from **1 service → 20+ microservices**, challenges multiply:

* Each service might have its own tech stack.
* Different deployment cadences.
* Environment promotion rules vary (e.g., dev auto-deploy, prod manual approval).
* Build agents/resources become a bottleneck.

---

## **Step 2 — Multi-Environment Pipeline Design**

### Environments:

* **Dev** — frequent deployments, feature testing, may allow partial failures.
* **Staging** — mirrors production, requires full tests to pass.
* **Prod** — mission-critical, requires approval, rollback safety.

---

## **Step 3 — Jenkins Implementation**

### 3.1 Folder Structure

```
jenkins/
  ├── shared-libs/
  ├── services/
       ├── orders-service/
       │     └── Jenkinsfile
       ├── inventory-service/
       │     └── Jenkinsfile
```

### 3.2 Multi-Branch Pipelines per Service

* Each repo triggers its own pipeline on commit.
* Use **Shared Libraries** for common stages:

```groovy
@Library('microservices-lib') _
ciPipeline(serviceName: 'orders-service')
```

### 3.3 Environment Promotion

```groovy
stage('Deploy to Prod') {
    when { branch 'main' }
    input {
        message "Deploy to production?"
        ok "Yes, Deploy"
    }
    steps {
        sh "./scripts/deploy.sh prod"
    }
}
```

---

## **Step 4 — Tekton Implementation**

### 4.1 Reusable Pipeline with Params

* Parameters:

  * `serviceName`
  * `environment`
  * `gitRevision`
* Example `PipelineRun`:

```yaml
spec:
  params:
    - name: serviceName
      value: orders-service
    - name: environment
      value: staging
```

### 4.2 Environment-Specific Triggers

* Dev branch → auto deploy to dev namespace.
* Main branch → trigger manual approval before prod.

---

## **Step 5 — Harness Implementation**

Harness makes multi-service easier:

* **Service Definition** per microservice.
* **Environment Definition** for dev, staging, prod.
* **Pipeline**:

  * Stage 1: Build & Test service (dynamic based on input).
  * Stage 2: Deploy to dev (auto).
  * Stage 3: Deploy to staging (manual QA approval).
  * Stage 4: Deploy to prod (manual + Continuous Verification).

---

## **Step 6 — Resource & Cost Optimization**

* **Jenkins**: Kubernetes agents on demand (per service build).
* **Tekton**: Use separate namespaces for isolation + limit ranges for CPU/memory.
* **Harness**: Multiple delegates spread across clusters to balance load.

---

## **Step 7 — Deployment Matrix (Optional)**

Deploy multiple services in parallel across environments:

* Jenkins: `parallel` stages.
* Tekton: multiple `PipelineRuns`.
* Harness: parallel stage groups.

---

## ✅ Lesson 9 Completion Criteria

* All three tools handle **multiple microservices**.
* Promotion logic is in place for **dev → staging → prod**.
* Approvals are enforced for production.
* Pipelines share common logic without duplication.

---
