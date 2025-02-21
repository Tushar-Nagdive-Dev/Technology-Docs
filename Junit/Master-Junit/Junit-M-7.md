---

# **Phase 7: CI/CD Integration with GitHub Actions and GitLab CI**

---

## **Objective**

In this phase, we will:
- Integrate **CI/CD pipelines** using:
  - **GitHub Actions** (for GitHub repositories)
  - **GitLab CI** (for GitLab repositories)
- Automate:
  - **Building** the application.
  - **Running all tests** (Unit, Integration, E2E).
  - **Generating code coverage reports** with **JaCoCo**.
  - **Deploying** the application to:
    - **Docker Hub** (containerized deployment)
    - **Kubernetes** (optional for advanced deployment)
- Ensure **continuous testing**, **fast feedback**, and **automated deployment**.

---

## **1. Why Use CI/CD for JUnit Testing?**

- **Continuous Testing:** Automatically runs all tests on every push and pull request.
- **Fast Feedback Loop:** Developers get instant feedback on test failures.
- **Consistent Environments:** Tests run in isolated environments, ensuring consistent results.
- **Automated Deployment:** Automatically builds and deploys the application on successful tests.
- **Integration with JaCoCo:** Generates and publishes **code coverage reports**.

---

## **2. Choosing the CI/CD Platform**

### **1. GitHub Actions**
- **Ideal for:** Projects hosted on **GitHub**.
- **Key Features:**
  - Native integration with GitHub.
  - Easy configuration using YAML files.
  - Marketplace with reusable actions.

### **2. GitLab CI/CD**
- **Ideal for:** Projects hosted on **GitLab**.
- **Key Features:**
  - Built-in CI/CD with advanced features.
  - Powerful **Pipeline Editor**.
  - Native support for Docker and Kubernetes.

---

## **3. CI/CD Workflow Overview**

The CI/CD workflow is divided into the following stages:

### **1. Build Stage:**
- **Compile the application** using Maven or Gradle.
- **Build Docker images** for each microservice.

### **2. Test Stage:**
- Run **Unit Tests** and **Integration Tests** using JUnit.
- Run **End-to-End Tests** using Selenium.
- Generate **Code Coverage Reports** using JaCoCo.

### **3. Package Stage:**
- Package the application as Docker images.
- **Push Docker images to Docker Hub** for containerized deployment.

### **4. Deploy Stage:**
- Deploy the application to:
  - **Docker Compose** (for local or staging environments)
  - **Kubernetes** (for production or advanced environments)

---

## **4. CI/CD with GitHub Actions**

### **1. Creating the Workflow File**

Create a new file in your repository:
```
.github/workflows/ci-cd.yml
```

### **2. Example CI/CD Pipeline Configuration**

```yaml
name: CI/CD Pipeline

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: '17'

      - name: Cache Maven dependencies
        uses: actions/cache@v2
        with:
          path: ~/.m2/repository
          key: ${{ runner.os }}-maven-${{ hashFiles('**/pom.xml') }}

      - name: Build with Maven
        run: mvn clean install -DskipTests

  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Set up JDK 17
        uses: actions/setup-java@v2
        with:
          java-version: '17'

      - name: Run Unit Tests and Integration Tests
        run: mvn test

      - name: Run End-to-End Tests
        run: mvn verify -P e2e

      - name: Generate JaCoCo Report
        run: mvn jacoco:report

  package:
    runs-on: ubuntu-latest
    needs: test
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: Build Docker Images
        run: |
          docker build -t your-dockerhub-username/user-service ./user-service
          docker build -t your-dockerhub-username/product-service ./product-service
          docker build -t your-dockerhub-username/order-service ./order-service

      - name: Login to Docker Hub
        uses: docker/login-action@v2
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      - name: Push Docker Images
        run: |
          docker push your-dockerhub-username/user-service
          docker push your-dockerhub-username/product-service
          docker push your-dockerhub-username/order-service
```

---

### **3. Explanation:**
- **Triggers:**
  - On every `push` and `pull request` to the `main` branch.
- **Build Stage:**
  - Checks out the code and sets up JDK 17.
  - **Caches Maven dependencies** for faster builds.
  - Builds the application using `mvn clean install`.
- **Test Stage:**
  - Runs **Unit Tests**, **Integration Tests**, and **End-to-End Tests**.
  - Generates **JaCoCo Code Coverage Report**.
- **Package Stage:**
  - Builds Docker images for:
    - `user-service`
    - `product-service`
    - `order-service`
  - **Pushes Docker images** to Docker Hub.

### **4. Environment Variables:**
- **DOCKER_USERNAME** and **DOCKER_PASSWORD** are stored as **GitHub Secrets**.
  - Go to **Settings → Secrets → Actions** and add:
    - `DOCKER_USERNAME`: Your Docker Hub username.
    - `DOCKER_PASSWORD`: Your Docker Hub password.

---

## **5. CI/CD with GitLab CI/CD**

### **1. Creating the Pipeline File**

Create a new file in your repository:
```
.gitlab-ci.yml
```

---

### **2. Example CI/CD Pipeline Configuration**

```yaml
stages:
  - build
  - test
  - package
  - deploy

variables:
  MAVEN_OPTS: "-Dmaven.repo.local=$CI_PROJECT_DIR/.m2/repository"

cache:
  paths:
    - .m2/repository

build:
  stage: build
  image: maven:3.8.7-jdk-17
  script:
    - mvn clean install -DskipTests

test:
  stage: test
  image: maven:3.8.7-jdk-17
  script:
    - mvn test
    - mvn jacoco:report
  artifacts:
    paths:
      - target/site/jacoco/index.html

package:
  stage: package
  image: docker:latest
  services:
    - docker:dind
  script:
    - docker build -t $CI_REGISTRY_IMAGE/user-service ./user-service
    - docker build -t $CI_REGISTRY_IMAGE/product-service ./product-service
    - docker build -t $CI_REGISTRY_IMAGE/order-service ./order-service
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY_IMAGE/user-service
    - docker push $CI_REGISTRY_IMAGE/product-service
    - docker push $CI_REGISTRY_IMAGE/order-service

deploy:
  stage: deploy
  image: alpine:latest
  script:
    - echo "Deployment step (e.g., Docker Compose or Kubernetes)"
```

---

### **3. Explanation:**
- **Stages:** `build`, `test`, `package`, and `deploy`.
- **Docker in Docker (`docker:dind`):** Used for building and pushing Docker images.
- **Artifacts:** Stores the **JaCoCo report** for later viewing.

---

## **Next Steps: Project Completion and Mastery Validation**

Congratulations on reaching the final phase! Next, we will:
- **Validate Mastery** by ensuring:
  - 80%+ code coverage
  - All tests pass consistently
- **Review Key Learnings** and **Best Practices**.
- **Plan Deployment** to a **staging** or **production** environment.
- **Finalize Documentation** and **publish the repository**.
