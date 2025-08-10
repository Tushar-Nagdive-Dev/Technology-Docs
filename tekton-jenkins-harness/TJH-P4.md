1. Have **Jenkins**, **Tekton**, and **Harness** build & push Docker images to a registry.
2. Deploy the app automatically to a Kubernetes cluster using Helm.
3. Maintain artifact versioning and rollback capability.

---

## **Step 1 — Preparing the Registry**

We’ll use **Docker Hub** here, but you can swap for ECR/GCR/ACR or a private registry.

1. **Create an account** on [Docker Hub](https://hub.docker.com/).
2. **Create a repository** (e.g., `tushar/petclinic`).
3. Get your credentials (username & PAT).

---

## **Step 2 — Dockerfile for the App**

In your repo root, create:

```dockerfile
FROM eclipse-temurin:17-jdk-alpine
WORKDIR /app
COPY target/*.jar app.jar
ENTRYPOINT ["java","-jar","app.jar"]
```

Build locally to confirm:

```bash
mvn clean package
docker build -t tushar/petclinic:1.0 .
```

---

## **Step 3 — Jenkins: Build & Push Image, Deploy to K8s**

### 3.1 Credentials Setup

* Jenkins → Manage Jenkins → Credentials → Add:

  * **Kind:** Username with password
  * ID: `dockerhub-creds`
  * Username: `<dockerhub-username>`
  * Password: `<dockerhub-PAT>`

### 3.2 Jenkinsfile

```groovy
pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "tushar/petclinic"
        DOCKER_TAG = "${env.BUILD_NUMBER}"
    }
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build JAR') { steps { sh 'mvn clean package -DskipTests' } }
        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                      echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                      docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
                      docker push $DOCKER_IMAGE:$DOCKER_TAG
                    """
                }
            }
        }
        stage('Deploy to K8s') {
            steps {
                sh """
                  helm upgrade --install petclinic ./helm \
                    --set image.repository=$DOCKER_IMAGE \
                    --set image.tag=$DOCKER_TAG
                """
            }
        }
    }
}
```

* Ensure **kubectl** & **helm** are installed in Jenkins agent.

---

## **Step 4 — Tekton: Build & Push Image, Deploy via Helm**

### 4.1 Docker Registry Secret

```bash
kubectl create secret docker-registry dockerhub-creds \
  --docker-username=<username> \
  --docker-password=<PAT> \
  --docker-email=<email>
```

### 4.2 Tekton Task

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: build-push-deploy
spec:
  workspaces:
    - name: source
  steps:
    - name: build
      image: maven:3.9-eclipse-temurin-17
      workingDir: /workspace/source
      script: |
        mvn clean package -DskipTests
    - name: docker-build-push
      image: docker:20.10
      script: |
        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
        docker build -t $DOCKER_IMAGE:$DOCKER_TAG .
        docker push $DOCKER_IMAGE:$DOCKER_TAG
      env:
        - name: DOCKER_IMAGE
          value: tushar/petclinic
        - name: DOCKER_TAG
          value: "latest"
        - name: DOCKER_USER
          valueFrom:
            secretKeyRef:
              name: dockerhub-creds
              key: .dockerconfigjson
    - name: deploy
      image: alpine/helm:3.12.0
      script: |
        helm upgrade --install petclinic ./helm \
          --set image.repository=$DOCKER_IMAGE \
          --set image.tag=$DOCKER_TAG
```

---

## **Step 5 — Harness: Build & Push Image, Deploy to K8s**

1. **Add Docker Hub Connector** in Harness.
2. **Add Kubernetes Connector** pointing to your cluster.
3. **CI Stage**:

   * Build JAR
   * Docker build & push step (use connector)
4. **CD Stage**:

   * Create a **Service** with Docker image reference.
   * Create **Environment** (dev/staging/prod).
   * Add **Rolling Deployment** stage with Helm chart path.

---

## **Step 6 — Artifact Versioning & Rollback**

* **Jenkins:** Use `${BUILD_NUMBER}` or `git commit hash` as tag. Rollback with `helm rollback`.
* **Tekton:** Pass `gitrevision` param to tag images.
* **Harness:** Uses build numbers automatically; rollback is a one-click redeploy of previous artifact.

---

## ✅ Lesson 3 Completion Criteria

* Docker image builds and pushes successfully in all three tools.
* K8s deployment updates automatically with new image.
* Ability to rollback to a previous version.
