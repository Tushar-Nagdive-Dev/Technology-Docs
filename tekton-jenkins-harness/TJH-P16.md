1. Build a **centralized CI/CD platform** that multiple teams can use without reinventing pipelines.
2. Offer **self-service onboarding** for new projects.
3. Maintain **governance, compliance, and scalability** while enabling developer autonomy.

---

## **Step 1 — Why CI/CD as a Service?**

In large enterprises:

* Every team needs CI/CD, but building pipelines from scratch is repetitive.
* You want **centralized governance**, **security controls**, and **shared infrastructure**.
* Developers should **onboard in minutes**, not weeks.

This approach treats CI/CD like a **shared internal product**.

---

## **Step 2 — Core Principles of CI/CD as a Service**

1. **Centralized Infrastructure** — One platform, many pipelines.
2. **Self-Service Templates** — Prebuilt, approved pipelines.
3. **Governance Built-In** — Policy-as-code, RBAC, audit logging.
4. **Scalability** — Horizontal scaling for builds and deployments.
5. **Pluggable Integrations** — SCM, issue tracking, security scanners, cloud providers.

---

## **Step 3 — Jenkins as a CI/CD Service**

* **Shared Libraries**: Store common pipeline logic (build, test, scan, deploy) in Git.
* **Job DSL + JCasC**:

  * Allow new teams to request a pipeline via a config file.
  * Jenkins automatically provisions the job.
* **Multi-Tenant Isolation**:

  * Separate folders for teams.
  * RBAC plugin to restrict access.
* **Service Catalog**:

  * Docs + forms for developers to pick a pipeline type (Java, Node.js, Python, ML, etc.).

---

## **Step 4 — Tekton as a CI/CD Service**

* **Pipeline Catalog**:

  * Host reusable `Tasks` & `Pipelines` in a Git repo.
  * Developers import via `kubectl apply -f` or Tekton Hub.
* **Parameterization**:

  * Pipelines accept params for repo URL, branch, build type.
* **GitOps Onboarding**:

  * Create a new `PipelineRun` YAML in a Git repo → ArgoCD syncs → Tekton executes.
* **Namespaces per Team**:

  * Limit CPU/memory usage per namespace.

---

## **Step 5 — Harness as a CI/CD Service**

* **Pipeline Templates**:

  * Create master templates for common workflows.
  * Developers clone & customize within boundaries.
* **RBAC**:

  * Allow developers to run pipelines but restrict modifications to sensitive steps.
* **Service Onboarding Wizard**:

  * Input: Repo URL, language, build tool, environments.
  * Output: Fully functional pipeline linked to environments.

---

## **Step 6 — Governance in the Service Model**

* **Policy Enforcement**:

  * Jenkins: OPA or in-pipeline checks.
  * Tekton: Gatekeeper policies for PipelineRuns.
  * Harness: Built-in Governance Policies.
* **Secrets Management**:

  * Integrate with Vault, AWS Secrets Manager, or GCP Secret Manager.
* **Audit Trails**:

  * Track who deployed, what changed, and when.

---

## **Step 7 — Scaling the Platform**

* **Jenkins**: Use Kubernetes plugin for on-demand agents.
* **Tekton**: Horizontal Pod Autoscaling for controller & webhooks.
* **Harness**: Multiple delegates per cluster & region.

---

## **Step 8 — Developer Experience**

* **Service Portal**:

  * Teams browse available pipeline templates.
  * Select and auto-provision via UI or API.
* **Documentation & Training**:

  * Internal wiki with guides.
* **Feedback Loops**:

  * Gather developer feedback to improve templates.

---

## ✅ Lesson 15 Completion Criteria

* Centralized CI/CD service platform running on Jenkins, Tekton, or Harness.
* New projects can onboard via templates in minutes.
* Governance & RBAC in place to ensure compliance.
* Platform scales to serve multiple teams concurrently.
