1. Adapt **Jenkins**, **Tekton**, and **Harness** for **machine learning (ML) and data workflows**.
2. Automate **data ingestion, preprocessing, training, evaluation, and deployment**.
3. Integrate with **model registries** and **dataset versioning** tools for reproducibility.

---

## **Step 1 — Why AI/Data Pipelines Need Different CI/CD**

Traditional CI/CD focuses on application code.
AI/ML pipelines add extra complexity:

* **Data versioning** (datasets change independently of code).
* **Model artifacts** (instead of JAR/WAR, we deploy `.pkl`, `.onnx`, `.pt` files).
* **Model evaluation** (accuracy, precision, recall gates before deploy).
* **Special infra needs** (GPU, large storage).
* **Continuous Training (CT)** and **Continuous Deployment of Models (CD4ML)**.

---

## **Step 2 — AI/ML Pipeline Stages in CI/CD**

**1. Data Stage**

* Pull dataset from S3, GCS, or data warehouse.
* Run preprocessing & feature engineering scripts.
* Store processed dataset in a **dataset registry** (e.g., DVC, Delta Lake).

**2. Model Stage**

* Train model (GPU-enabled agent).
* Log metrics (MLflow, Weights & Biases).
* Save model to **model registry**.

**3. Evaluation Stage**

* Run unit tests on model code.
* Run accuracy tests (fail if below threshold).
* Generate explainability reports (SHAP, LIME).

**4. Deployment Stage**

* Deploy model as REST/gRPC service or push to edge device.
* Canary rollout for model version.
* Rollback if accuracy/latency degrades in production.

---

## **Step 3 — Jenkins for AI/ML CI/CD**

### Jenkinsfile Example:

```groovy
pipeline {
    agent { label 'gpu-agent' }
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Data Preprocessing') { steps { sh 'python scripts/preprocess.py' } }
        stage('Train Model') { steps { sh 'python scripts/train.py --epochs=10' } }
        stage('Evaluate Model') {
            steps {
                sh 'python scripts/evaluate.py --threshold=0.85'
            }
        }
        stage('Register Model') {
            steps { sh 'mlflow register-model --model-uri runs:/latest/model --name OrdersModel' }
        }
        stage('Deploy Model') {
            steps { sh './scripts/deploy_model.sh' }
        }
    }
}
```

* Use **Jenkins GPU Nodes** (Docker image with CUDA).
* Store model in **MLflow** or **S3**.

---

## **Step 4 — Tekton for AI/ML CI/CD**

### Pipeline Tasks:

1. `git-clone` (model code).
2. `data-preprocess` (Python + Pandas/Numpy).
3. `model-train` (GPU container with TensorFlow/PyTorch).
4. `model-evaluate` (fail if accuracy < 0.85).
5. `model-register` (MLflow/S3).
6. `model-deploy` (KServe/Seldon).

**Example GPU-enabled Task:**

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: model-train
spec:
  steps:
    - name: train
      image: nvcr.io/nvidia/tensorflow:23.03
      script: |
        python scripts/train.py --epochs=10
  nodeSelector:
    accelerator: nvidia
  tolerations:
    - key: nvidia.com/gpu
      operator: Exists
```

---

## **Step 5 — Harness for AI/ML CI/CD**

* **CI Stage**:

  * Data preprocessing & training in a GPU-enabled build environment.
  * Evaluate metrics → fail pipeline if accuracy is below target.
  * Push model to S3/MLflow.
* **CD Stage**:

  * Deploy via KServe, Seldon, or custom inference service.
  * Canary rollout with **real-time metric checks** (latency, accuracy drift).
  * Rollback to previous model version if drift detected.

---

## **Step 6 — Dataset & Model Versioning**

* Use **DVC** (Data Version Control):

  ```bash
  dvc add data/dataset.csv
  dvc push
  ```
* Use **MLflow** for model registry:

  ```bash
  mlflow models serve -m "models:/OrdersModel/1" -p 5000
  ```

---

## **Step 7 — Monitoring Deployed Models**

* Capture **real-time inference metrics** (Prometheus + Grafana).
* Monitor **data drift** & **concept drift** using EvidentlyAI.
* Trigger retraining pipeline automatically when drift exceeds threshold.

---

## ✅ Lesson 12 Completion Criteria

* Jenkins, Tekton, and Harness pipelines handle **data + model workflows**.
* Models are evaluated & only deployed if metrics pass thresholds.
* Dataset & model versions are tracked for reproducibility.
* Canary rollouts with rollback for models are in place.
