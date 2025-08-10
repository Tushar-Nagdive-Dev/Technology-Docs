## **CI/CD Core Theory**

**Continuous Integration (CI)** → Merging code frequently into a shared branch, then automatically building & testing it to detect issues early.
**Continuous Delivery (CD)** → Preparing the app so it’s always deployable, including approvals, artifact storage, and environment promotion.
**Continuous Deployment** → Fully automating deployment to production without manual approval.

**Pipeline** = A sequence of automated steps: **Source → Build → Test → Package → Deploy → Verify**.

Key CI/CD concepts:

* **Stages** → Logical sections of the pipeline (e.g., Build, Test).
* **Steps** → Commands inside a stage.
* **Artifacts** → Built output (e.g., `.jar`, `.war`, `.docker image`).
* **Triggers** → Events that start a pipeline (e.g., Git push).
* **Agents/Workers** → Machines or containers that execute jobs.
* **Environment** → The deployment target (dev, staging, prod).
* **Rollback** → Restoring to a previous stable version.

---

## **Jenkins — Theory Overview**

* **Type:** General-purpose automation server (oldest, most mature).
* **Architecture:**

  * **Controller (Master):** Orchestrates jobs, stores config, UI.
  * **Agents (Slaves):** Run actual builds, can scale horizontally.
* **Pipeline Types:**

  * **Declarative** (`pipeline { ... }`) → structured, opinionated syntax.
  * **Scripted** → more flexibility but more complex.
* **Strengths:**

  * Huge plugin ecosystem (SAST, deployments, notifications, SCM integrations).
  * Works with any tech stack.
  * Mature ecosystem & large community.
* **Weaknesses:**

  * Plugin dependency can cause maintenance issues.
  * UI is older, can be slow if overloaded.
* **Best Use:**

  * Legacy projects, multi-language builds, self-hosted pipelines, when you need full control.

---

## **Tekton — Theory Overview**

* **Type:** Kubernetes-native CI/CD framework (built as CRDs).
* **Key CRDs:**

  * **Task** → smallest unit of execution (like a Jenkins stage).
  * **Step** → a container that does one job inside a Task.
  * **Pipeline** → ordered list of Tasks.
  * **PipelineRun / TaskRun** → execution instance.
  * **Workspace** → shared storage between steps.
  * **Params & Results** → pass data in/out.
  * **Triggers** (EventListener, TriggerBinding, TriggerTemplate) → start pipelines from events.
* **Strengths:**

  * Cloud-native, portable, highly composable.
  * No central controller bottleneck (scales with K8s).
  * Secure by design (least privilege, no root in containers).
* **Weaknesses:**

  * YAML-heavy and verbose.
  * Needs Kubernetes skills.
* **Best Use:**

  * Cloud-native microservices, GitOps, Kubernetes-heavy orgs.

---

## **Harness — Theory Overview**

* **Type:** SaaS CI/CD + Deployment Verification + Feature Flags platform.
* **Architecture:**

  * **Harness SaaS UI/Control Plane** — your pipelines, policies, templates.
  * **Delegates** — lightweight agents running in your infra to execute tasks.
* **Pipeline Stages:**

  * **CI:** Build, test, artifact creation.
  * **CD:** Deploy (Rolling, Blue/Green, Canary), approvals, rollback.
* **Special Features:**

  * **Continuous Verification:** Automated analysis of metrics/logs after deploy.
  * **Secrets Management** (built-in).
  * **Governance & RBAC** (enterprise-ready).
* **Strengths:**

  * Minimal setup — SaaS handles complexity.
  * Built-in cloud integrations, GitOps, and verification.
  * YAML + UI editing.
* **Weaknesses:**

  * Paid for enterprise; free tier has limits.
  * Less customizable than raw Jenkins/Tekton.
* **Best Use:**

  * Modern cloud-native orgs, teams wanting speed & compliance without building their own infra.

---

## **Comparative Mental Model**

| Feature                   | Jenkins                       | Tekton                        | Harness                            |
| ------------------------- | ----------------------------- | ----------------------------- | ---------------------------------- |
| **Hosting**               | Self-hosted (VMs, containers) | K8s-native (self-hosted)      | SaaS + on-prem delegate            |
| **Pipeline Definition**   | Groovy (Declarative/Scripted) | YAML CRDs (Tasks, Pipelines)  | YAML or UI                         |
| **Scalability**           | Controller-Agent model        | Horizontal with Kubernetes    | SaaS auto-scales via delegates     |
| **Ease of Setup**         | Medium                        | Harder (needs K8s)            | Easiest                            |
| **Plugins/Extensibility** | Massive plugin ecosystem      | Tekton Catalog + custom Tasks | Built-in integrations              |
| **Cloud-Native**          | No                            | Yes                           | Yes                                |
| **Best For**              | Any stack, on-prem            | Cloud-native microservices    | Enterprises wanting speed & safety |

---

## **Real-World Example Flow**

Imagine we have a microservice `orders-service`:

1. **Developer** pushes code to GitHub → webhook triggers CI.
2. **CI Phase**:

   * Jenkins runs `mvn test` + builds `.jar` → archives artifact.
   * Tekton builds Docker image + pushes to registry.
   * Harness builds Docker image, runs tests, pushes to registry.
3. **CD Phase**:

   * Jenkins uses SSH/K8s plugin to deploy to staging.
   * Tekton deploys via Helm to K8s.
   * Harness deploys via Canary with built-in metrics check.
4. **Verification**:

   * Jenkins: manual script or plugin.
   * Tekton: custom task to query Prometheus.
   * Harness: built-in Continuous Verification.

---
