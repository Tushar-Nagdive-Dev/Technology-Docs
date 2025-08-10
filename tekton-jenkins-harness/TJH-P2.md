## **Lesson 1 – Step 1: Prepare the Playground**

We need:

* A local Kubernetes cluster for Tekton
* A Docker setup for Jenkins and app builds
* A Git repo with a small app to run through CI/CD

---

### **1. Install Core Tools**

**a) Docker**

* [Download Docker Desktop](https://www.docker.com/products/docker-desktop/) and install.
* Verify:

```bash
docker --version
```

**b) Kubernetes (Kind)**
We’ll use Kind (Kubernetes-in-Docker) because it’s light.

```bash
# Install Kind (Mac/Linux)
curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-$(uname)-amd64
chmod +x ./kind
sudo mv ./kind /usr/local/bin/kind

# Create cluster
kind create cluster --name cicd-lab
```

Verify:

```bash
kubectl cluster-info --context kind-cicd-lab
```

---

### **2. Sample App Setup**

Let’s pick a small **Spring Boot REST API** with tests (works well across all CI tools).

```bash
git clone https://github.com/spring-projects/spring-petclinic.git
cd spring-petclinic
```

* This has Maven build, unit tests, Dockerfile (we’ll add), and Helm chart (we’ll add later).

---

### **3. Jenkins Quickstart**

Run Jenkins in Docker:

```bash
docker run -d --name jenkins \
  -p 8080:8080 -p 50000:50000 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

* Open [http://localhost:8080](http://localhost:8080)
* Unlock Jenkins using the printed admin password:

```bash
docker exec -it jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

* Install **recommended plugins**.

---

**Create Jenkins Pipeline job:**

1. New Item → “Pipeline” → name: `petclinic-ci`.
2. Pipeline script from SCM → Git → your repo URL.
3. Create a `Jenkinsfile` in repo:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build') { steps { sh 'mvn -B -DskipTests clean package' } }
        stage('Test') { steps { sh 'mvn test' } }
        stage('Package') { steps { archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true } }
    }
    post {
        always { junit '**/target/surefire-reports/*.xml' }
    }
}
```

4. Commit + push.
5. Run the job and ensure all stages go green.

---

### **4. Tekton Quickstart**

Install Tekton Pipelines in Kind cluster:

```bash
kubectl apply --filename https://storage.googleapis.com/tekton-releases/pipeline/latest/release.yaml
```

Install Tekton CLI:

```bash
brew install tektoncd-cli   # Mac
# or
sudo apt-get install tektoncd-cli  # Ubuntu/Debian
```

Create `petclinic-task.yaml`:

```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: petclinic-build
spec:
  steps:
    - name: build
      image: maven:3.9-eclipse-temurin-17
      workingDir: /workspace/source
      script: |
        mvn -B -DskipTests clean package
    - name: test
      image: maven:3.9-eclipse-temurin-17
      workingDir: /workspace/source
      script: |
        mvn test
```

Create `petclinic-pipeline.yaml`:

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: petclinic-pipeline
spec:
  tasks:
    - name: run-build
      taskRef:
        name: petclinic-build
```

Apply:

```bash
kubectl apply -f petclinic-task.yaml
kubectl apply -f petclinic-pipeline.yaml
```

Run:

```bash
tkn pipeline start petclinic-pipeline --workspace name=source,emptyDir=""
```

---

### **5. Harness Quickstart**

1. Sign up for [Harness Free Plan](https://app.harness.io).
2. Create Project → “PetClinic CI”.
3. Add **Git Connector** (GitHub repo URL).
4. Add **CI Pipeline** → Source from Git → Build Stage:

   * Step 1: Run Maven build/test
   * Step 2: Archive artifacts
5. Run pipeline and verify.

---

✅ **Lesson 1 Completion Criteria**

* Jenkins pipeline runs green.
* Tekton pipeline executes successfully.
* Harness pipeline builds/tests successfully.
