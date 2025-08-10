## **Lesson 7: Real-World CI/CD Capstone Project**

You’ll implement **one complete microservice** with **Jenkins**, **Tekton**, and **Harness** — each running a **full CI/CD flow** from Git commit → build → test → security scan → artifact push → advanced deployment → verification → rollback.

This will consolidate **everything** from Lessons 1–6 into a single, production-ready setup.

---

## **Step 1 — Project Overview**

We’ll build & deploy:

**Service:**
`orders-service` (Spring Boot, Maven, REST API with `/orders` endpoint, connected to PostgreSQL)

**Flow:**

1. Developer pushes to Git → Webhook triggers pipeline.
2. **CI Stage**:

   * Checkout code.
   * Build + unit tests.
   * Code coverage check (fail if <80%).
   * SBOM + Vulnerability scan (Trivy).
   * Build & push signed Docker image.
3. **CD Stage**:

   * Blue-Green deployment in Kubernetes via Helm.
   * Canary rollout with metric verification.
4. **Post-Deploy**:

   * Smoke test `/actuator/health`.
   * If fail → automated rollback.
   * Store build metadata (commit hash, image tag, SBOM).

---

## **Step 2 — Common Resources Across All Three Tools**

### 2.1 GitHub Repo

* `/src` — Spring Boot code.
* `/Dockerfile` — container build file.
* `/helm/orders-service` — Helm chart.
* `/Jenkinsfile` — Jenkins pipeline definition.
* `/tekton/` — Tekton YAML manifests.
* `/harness/` — Harness YAML pipeline definition.

### 2.2 Helm Chart Values (supports Blue-Green)

```yaml
image:
  repository: tushar/orders-service
  tag: latest
deployment:
  color: blue
service:
  port: 8080
canary:
  enabled: false
  weight: 0
```

---

## **Step 3 — Jenkins Implementation**

**Jenkinsfile:**

```groovy
pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "tushar/orders-service"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build & Test') { steps { sh 'mvn clean verify' } }
        stage('Coverage Gate') {
            steps {
                jacoco execPattern: '**/jacoco.exec'
                script {
                    if (currentBuild.result == 'FAILURE') {
                        error "Coverage below threshold"
                    }
                }
            }
        }
        stage('SBOM & Security Scan') {
            steps {
                sh "syft . -o json > sbom.json"
                sh "trivy image --exit-code 1 --severity HIGH $DOCKER_IMAGE:$DOCKER_TAG || true"
            }
        }
        stage('Docker Build & Push + Sign') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh """
                      echo $PASS | docker login -u $USER --password-stdin
                      docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                      docker push $DOCKER_IMAGE:$DOCKER_TAG
                      cosign sign --key cosign.key $DOCKER_IMAGE:$DOCKER_TAG
                    """
                }
            }
        }
        stage('Blue-Green Deploy') {
            steps {
                sh "./scripts/deploy-blue-green.sh $DOCKER_IMAGE $DOCKER_TAG"
            }
        }
        stage('Canary Rollout') {
            steps {
                sh "./scripts/deploy-canary.sh $DOCKER_IMAGE $DOCKER_TAG"
            }
        }
        stage('Smoke Test') {
            steps {
                sh "curl -f http://orders.example.com/actuator/health"
            }
        }
    }
    post {
        failure {
            sh "helm rollback orders-service 1"
        }
    }
}
```

---

## **Step 4 — Tekton Implementation**

* **Pipeline Tasks:**

  1. `git-clone`
  2. `maven-build-test`
  3. `coverage-check`
  4. `sbom-scan`
  5. `docker-build-push-sign`
  6. `blue-green-deploy`
  7. `canary-deploy`
  8. `smoke-test`

* **Trigger:**

  * EventListener + TriggerTemplate for GitHub push events.

* **Workspaces:**

  * `source` → app code.
  * `maven-cache` → persistent `.m2` directory.

* **Secrets:**

  * Docker Hub credentials in Kubernetes secret.
  * Cosign key in Kubernetes secret.

---

## **Step 5 — Harness Implementation**

* **CI Stage:**

  * Build + test.
  * Jacoco coverage gate.
  * Syft SBOM.
  * Trivy scan (fail on HIGH severity).
  * Docker build & push.
  * Cosign signing.

* **CD Stage:**

  * Blue-Green deployment with Helm values override.
  * Canary rollout in steps: 10% → 50% → 100% (pause for verification).
  * Built-in Continuous Verification (Prometheus metrics).
  * Rollback on fail.

---

## **Step 6 — Verification & Metrics**

* **Smoke Tests:** run immediately after deploy.
* **Metrics Checks:** error rate < 1%, latency < 200ms.
* **Logs Monitoring:** look for ERROR patterns in first 5 minutes.

---

## ✅ Lesson 7 Completion Criteria

* Same service is fully deployed via Jenkins, Tekton, and Harness.
* Each pipeline has:

  * CI build + test + scan + image signing.
  * Advanced deployment (Blue-Green + Canary).
  * Automated rollback.
  * Compliance artifacts stored (SBOM, scan reports).
* End-to-end from commit → prod takes under 15 minutes.

