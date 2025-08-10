1. Monitor CI/CD pipelines and deployments in **Jenkins**, **Tekton**, and **Harness**.
2. Set up logging, tracing, and metrics collection.
3. Implement automated incident response and notifications.

---

## **Step 1 — Why Post-Deployment Observability Matters**

After your application is deployed, the work isn’t “done.”
You need to ensure:

* The deployment succeeded without issues.
* The service is healthy and meeting SLAs.
* You can detect and roll back quickly if something goes wrong.

In CI/CD terms:

* **Jenkins** → monitor build times, success/failure trends, and stage performance.
* **Tekton** → track PipelineRun and TaskRun status, cluster resource usage.
* **Harness** → leverage built-in Continuous Verification and metrics dashboards.

---

## **Step 2 — Monitoring Build & Deployment Pipelines**

### 2.1 Jenkins

* Install **Build Monitor View** plugin for visual dashboard.
* Enable **Prometheus plugin** for pipeline metrics:

  * Metrics like `jenkins_job_duration_seconds` and `jenkins_job_success_total`.
* Export to **Grafana** for custom dashboards.

### 2.2 Tekton

* Install **Tekton Dashboard**:

```bash
kubectl apply -f https://storage.googleapis.com/tekton-releases/dashboard/latest/tekton-dashboard-release.yaml
```

* Monitor PipelineRuns visually.
* Enable metrics:

  * Tekton exposes Prometheus metrics via controller pods.
  * Create Grafana dashboards for pipeline run durations, success rates.

### 2.3 Harness

* Built-in metrics dashboard per pipeline.
* Track:

  * Deployment frequency.
  * Lead time for changes.
  * Change failure rate (DORA metrics).

---

## **Step 3 — Application-Level Monitoring**

### 3.1 Metrics (Prometheus + Grafana)

* Deploy **Prometheus Operator** to your cluster.
* Scrape:

  * `/actuator/prometheus` (Spring Boot Micrometer metrics).
  * Ingress/controller metrics.
* Create Grafana alerts for error rate, latency, and throughput.

### 3.2 Logging (ELK or Loki Stack)

* **ELK Stack (Elasticsearch, Logstash, Kibana)**:

  * Centralized logging with query capabilities.
* **Grafana Loki**:

  * Lighter alternative for Kubernetes logs.
* Use `kubectl logs` only for ad-hoc checks; production needs centralized logging.

### 3.3 Tracing (Jaeger / OpenTelemetry)

* Add **OpenTelemetry** instrumentation to the app.
* Trace request flows across services.
* Detect latency bottlenecks post-deployment.

---

## **Step 4 — Automated Incident Response**

### 4.1 Jenkins

* Post-build actions:

  * Send Slack/Teams messages on failure:

    ```groovy
    slackSend channel: '#alerts', message: "Jenkins build failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}"
    ```
  * Trigger rollback job automatically.

### 4.2 Tekton

* Add `finally` tasks for rollback:

```yaml
finally:
  - name: rollback
    taskRef:
      name: helm-rollback
```

* Integrate with Knative Eventing or Argo Events for alert triggers.

### 4.3 Harness

* Built-in:

  * On Failure → Rollback to last successful deployment.
  * Send notifications via Email, Slack, MS Teams.

---

## **Step 5 — Health & SLA Alerts**

* Alert Types:

  * **Error Budget Burn** → alert if error rate is increasing.
  * **Latency Spike** → alert if p95 latency > threshold.
  * **Deployment Failure** → alert if a pipeline fails > N times consecutively.
* Response Actions:

  * Auto-scale pods.
  * Rollback deployment.
  * Trigger incident ticket in Jira/ServiceNow.

---

## ✅ Lesson 8 Completion Criteria

* All pipelines (Jenkins, Tekton, Harness) are monitored with metrics dashboards.
* Application metrics, logs, and traces are visible in real-time.
* Alerts trigger Slack notifications and rollback automation.
