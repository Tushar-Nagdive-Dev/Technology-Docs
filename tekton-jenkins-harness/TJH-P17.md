1. Identify **where CI/CD costs come from** in Jenkins, Tekton, and Harness.
2. Apply strategies to **reduce build times, storage usage, and compute costs**.
3. Monitor cost trends and enforce **budget controls** without hurting developer productivity.

---

## **Step 1 — Where CI/CD Costs Come From**

* **Compute**

  * Build agents (VMs, containers, GPUs).
  * Over-provisioned runners that sit idle.
* **Storage**

  * Build artifacts, Docker images, logs, cache volumes.
* **Network**

  * Large artifact uploads/downloads between stages.
* **Licensing**

  * Harness usage tiers, Jenkins plugins with enterprise licenses.
* **Cloud Resource Sprawl**

  * Orphaned dev environments not shut down after builds.

---

## **Step 2 — Measuring CI/CD Costs**

* **Jenkins**

  * Track agent utilization with **Cloud Statistics Plugin**.
  * Measure job execution time and queue delays.
* **Tekton**

  * Use Prometheus to track pod CPU/memory usage & job durations.
* **Harness**

  * Use built-in **Usage Analytics** to see cost per pipeline execution.

---

## **Step 3 — Compute Cost Optimization**

### Jenkins

* Use **Kubernetes plugin** to spin up ephemeral agents per build instead of static nodes.
* Use smaller base images for agents.
* Run **parallel stages only where needed** (avoid parallelism overhead).

### Tekton

* Set **resource requests/limits** per `Task` to avoid over-provisioning.
* Use **spot/preemptible instances** for non-critical pipelines.

### Harness

* Assign **delegate profiles** with minimal required resources.
* Use **cloud cost governance** policies to auto-stop idle environments.

---

## **Step 4 — Storage Cost Optimization**

* Artifact Retention Policies:

  * Keep only last N builds' artifacts.
* Docker Image Cleanup:

  * Delete old tags from registry automatically.
* Log Retention:

  * Shorten retention for dev pipelines; keep longer for prod.

Example Jenkins cleanup:

```groovy
buildDiscarder(logRotator(numToKeepStr: '10', artifactNumToKeepStr: '5'))
```

---

## **Step 5 — Build Time Optimization**

* **Caching Dependencies**:

  * Maven: cache `.m2` directory between builds.
  * Node: cache `node_modules`.
* **Selective Builds**:

  * Build only changed modules using Git diff checks.
* **Pipeline Profiling**:

  * Identify slowest stages and optimize or split them.

---

## **Step 6 — Budget Controls**

* Set **build concurrency limits** in Jenkins and Tekton.
* Harness:

  * Set **execution budgets** per environment/team.
  * Alert when nearing monthly cost threshold.

---

## **Step 7 — Automating Cost Optimization**

* Auto-stop preview environments after inactivity.
* Run cleanup jobs nightly to delete old artifacts & images.
* Use AI-driven build scheduling (off-peak hours for cost savings).

---

## ✅ Lesson 16 Completion Criteria

* You know exactly where CI/CD costs come from.
* You’ve applied optimizations for compute, storage, and build time.
* Cost monitoring & budget enforcement is in place.
* Pipelines are faster, cheaper, and still reliable.

