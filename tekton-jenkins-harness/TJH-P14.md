1. Design pipelines that **run across multiple cloud providers** (AWS, Azure, GCP).
2. Deploy services to **different environments in different clouds**.
3. Dynamically switch or failover between clouds.

---

## **Step 1 — Why Multi-Cloud CI/CD**

Enterprises go multi-cloud for:

* **Resilience** → Avoid single-cloud outages.
* **Regulatory compliance** → Data locality laws.
* **Best-of-breed services** → Use GCP’s AI + AWS’s databases + Azure’s security.
* **Cost optimization** → Spot instances in one cloud, reserved in another.

Challenges:

* Secrets & credentials for multiple clouds.
* Different deployment tools per cloud.
* Network & latency considerations.
* Keeping environments consistent.

---

## **Step 2 — Pipeline Design Principles**

* **Cloud-agnostic pipeline logic** (build once, deploy anywhere).
* **Cloud-specific deployment templates** (Helm values, Terraform scripts).
* **Dynamic credentials loading**.
* **Failover automation** between clouds.

---

## **Step 3 — Jenkins Multi-Cloud Setup**

### 3.1 Credentials Management

* Store AWS, Azure, GCP credentials in Jenkins Credentials store.
* Load dynamically:

```groovy
withCredentials([file(credentialsId: 'aws-creds', variable: 'AWS_CRED_FILE')]) {
    sh 'export AWS_SHARED_CREDENTIALS_FILE=$AWS_CRED_FILE && ./deploy_aws.sh'
}
```

### 3.2 Deployment Logic

* Use **matrix** build for multi-cloud deployments:

```groovy
stage('Deploy Multi-Cloud') {
    matrix {
        axes {
            axis { name 'CLOUD'; values 'aws', 'azure', 'gcp' }
        }
        stages {
            stage('Deploy') {
                steps {
                    sh "./deploy_${CLOUD}.sh"
                }
            }
        }
    }
}
```

---

## **Step 4 — Tekton Multi-Cloud**

* Create `Task`s for each cloud deployment (`deploy-aws`, `deploy-azure`, `deploy-gcp`).
* Pass `CLOUD_PROVIDER` as a param to the pipeline.
* Example:

```yaml
params:
  - name: CLOUD_PROVIDER
    default: aws
```

* Conditional execution:

```yaml
when:
  - input: "$(params.CLOUD_PROVIDER)"
    operator: in
    values: ["aws"]
```

---

## **Step 5 — Harness Multi-Cloud**

* Create **Service** once, but **Environments** for each cloud:

  * AWS EKS cluster
  * Azure AKS cluster
  * GCP GKE cluster
* In pipeline:

  * Stage 1: Deploy to AWS Dev.
  * Stage 2: Deploy to Azure Staging.
  * Stage 3: Deploy to GCP Prod.
* Use **Infrastructure Definitions** to map cloud-specific settings.

---

## **Step 6 — Terraform for Cloud Provisioning**

Integrate Terraform into pipelines:

* Module for AWS EKS, Azure AKS, GCP GKE.
* Jenkins:

  ```groovy
  sh 'terraform init && terraform apply -auto-approve'
  ```
* Tekton: Terraform task from Tekton Hub.
* Harness: Terraform step in pipeline.

---

## **Step 7 — Failover Between Clouds**

* Maintain active-active or active-passive deployment patterns.
* Use **Global DNS (Cloudflare, AWS Route53, Azure Traffic Manager)** to switch traffic.
* Example Failover Plan:

  1. Detect outage in AWS Prod.
  2. Run pipeline that deploys latest image to Azure Prod.
  3. Update DNS to route all traffic to Azure.

---

## **Step 8 — Security & Compliance**

* Use **Vault** or **Secrets Manager** in each cloud, injected at runtime.
* Apply same policy-as-code rules across clouds with **OPA**.

---

## ✅ Lesson 13 Completion Criteria

* Single pipeline that can deploy to AWS, Azure, and GCP.
* Cloud-specific secrets handled securely.
* Failover between clouds tested and documented.

