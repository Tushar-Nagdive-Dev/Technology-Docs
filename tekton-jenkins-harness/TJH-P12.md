
1. Design **highly available Jenkins, Tekton, and Harness setups**.
2. Implement **backup & restore strategies**.
3. Ensure CI/CD pipelines survive infra failures with minimal downtime.

---

## **Step 1 — Why DR & HA Matter**

In enterprise CI/CD, downtime means:

* No deployments (impacting releases).
* Blocked bug fixes and hotfixes.
* Delayed incident recovery.

**Goals:**

* **HA (High Availability)** → The system remains operational during component failures.
* **DR (Disaster Recovery)** → The system can be restored after a catastrophic failure.

---

## **Step 2 — Jenkins HA & DR**

### 2.1 HA Setup

* **Controller HA**:

  * Use **active-passive** controllers behind a load balancer.
  * Store `JENKINS_HOME` on shared storage (NFS, EFS, CephFS).
* **Agent HA**:

  * Use Kubernetes plugin to spin up agents on demand in multiple clusters/regions.

### 2.2 Backup Strategy

* **Configuration Backup**:

  ```bash
  tar czf jenkins-home-backup-$(date +%F).tar.gz /var/jenkins_home
  ```
* Store in S3 or similar with versioning enabled.
* **Pipeline as Code** → Store Jenkinsfiles & Job DSL in Git.

### 2.3 Restore Procedure

* Restore `JENKINS_HOME` from backup.
* Restart Jenkins; agents auto-reconnect.

---

## **Step 3 — Tekton HA & DR**

### 3.1 HA Setup

* Deploy Tekton controllers in **multiple replicas**:

```bash
kubectl scale deployment tekton-pipelines-controller --replicas=3
```

* Use Kubernetes HA cluster with control plane redundancy.
* Run Tekton in multiple namespaces for isolation.

### 3.2 Backup Strategy

* Store:

  * `Pipeline` and `Task` YAMLs in Git (GitOps).
  * `PipelineRun` and logs in persistent storage (e.g., PVC snapshots).
* For logs: forward to ELK/Loki.

### 3.3 Restore Procedure

* Reapply CRDs from Git.
* Restore PVC snapshots.

---

## **Step 4 — Harness HA & DR**

### 4.1 HA Setup

* **Harness SaaS** → Control plane is managed by Harness.
* **Delegates** → Run at least **2 delegates per environment** in different availability zones.
* Use **Delegate profiles** for consistent config.

### 4.2 Backup Strategy

* Harness handles pipeline definitions automatically.
* Store YAML pipeline exports in Git for safety.

### 4.3 Restore Procedure

* Reinstall delegates from Harness UI using stored config.
* Import pipeline YAMLs if needed.

---

## **Step 5 — Failover Testing**

* **Jenkins**:

  * Kill primary controller → verify standby takes over.
* **Tekton**:

  * Delete one controller pod → ensure others continue processing.
* **Harness**:

  * Kill one delegate → verify others pick up jobs.

---

## **Step 6 — RPO & RTO Targets**

* **RPO (Recovery Point Objective)**: How much data loss is acceptable.

  * Target: **≤ 15 min** (frequent backups).
* **RTO (Recovery Time Objective)**: How fast to recover.

  * Target: **≤ 30 min** for critical pipelines.

---

## ✅ Lesson 11 Completion Criteria

* CI/CD infra is redundant and resilient.
* Backups are automated and tested.
* Failover is documented and verified.

