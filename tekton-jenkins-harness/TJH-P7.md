1. Run **parallel builds** and split workloads efficiently.
2. Implement **build caching** for faster pipelines.
3. **Reuse pipeline components** across multiple projects.
4. Scale Jenkins, Tekton, and Harness to handle thousands of builds/day.

---

## **Step 1 — Parallelization**

### 1.1 Jenkins

* Use `parallel` block to run multiple stages at once:

```groovy
pipeline {
    agent any
    stages {
        stage('Parallel Tests') {
            parallel {
                stage('Unit Tests') { steps { sh 'mvn test -Dgroups=unit' } }
                stage('Integration Tests') { steps { sh 'mvn test -Dgroups=integration' } }
            }
        }
    }
}
```

* **When to use:** multiple test suites, multi-platform builds, microservices.

---

### 1.2 Tekton

* Create multiple `Tasks` in a `Pipeline` without `runAfter` so they execute in parallel:

```yaml
tasks:
  - name: unit-tests
    taskRef: name: run-unit-tests
  - name: integration-tests
    taskRef: name: run-integration-tests
```

* Tekton automatically runs them in parallel if no dependency.

---

### 1.3 Harness

* Add **parallel stages** in pipeline editor.
* Good for multi-environment deployments or multi-service testing.

---

## **Step 2 — Build Caching**

### 2.1 Jenkins

* Use `stash/unstash` to reuse build artifacts between stages:

```groovy
stage('Build') {
    steps {
        sh 'mvn package'
        stash includes: 'target/*.jar', name: 'app-jar'
    }
}
stage('Deploy') {
    steps {
        unstash 'app-jar'
        sh 'kubectl apply -f deployment.yaml'
    }
}
```

* For Maven/Gradle, use persistent workspace volumes.

---

### 2.2 Tekton

* Use **Workspaces** with PersistentVolumeClaim (PVC):

```yaml
workspaces:
  - name: maven-cache
    persistentVolumeClaim:
      claimName: maven-cache-pvc
```

* Mount `.m2` directory to speed up Maven builds.

---

### 2.3 Harness

* Enable **Cache Intelligence** in CI stage → automatically caches dependencies between runs.

---

## **Step 3 — Reusable Pipelines**

### 3.1 Jenkins

* Use **Shared Libraries**:

  * Store Groovy pipeline functions in Git.
  * Load in Jenkinsfile:

    ```groovy
    @Library('my-shared-lib') _
    buildApp()
    ```
* Avoid copy-pasting stages in every repo.

---

### 3.2 Tekton

* Use **Tekton Catalog** tasks:

  * Example: `git-clone`, `maven`, `kaniko` tasks from [https://github.com/tektoncd/catalog](https://github.com/tektoncd/catalog).
* Create your own task YAMLs and reuse across multiple pipelines.

---

### 3.3 Harness

* Use **Pipeline Templates** or **Stage Templates**.
* Change once, propagate to all pipelines.

---

## **Step 4 — Scaling the Infrastructure**

### Jenkins

* Use **Kubernetes plugin** to spin up ephemeral build agents per job.
* Distribute load with multiple controllers in HA mode.

### Tekton

* Horizontal pod autoscaling for controllers.
* Use multiple PVCs for high I/O workloads.

### Harness

* Deploy multiple delegates in different clusters/regions for load distribution.

---

## **Step 5 — Monitoring & Optimization**

* Track **build time per stage**.
* Identify slow steps and cache results.
* Set **timeouts** for failing builds quickly.
* Monitor **queue times** and add more agents if needed.

---

## ✅ Lesson 6 Completion Criteria

* Pipelines execute stages in parallel.
* Builds are cached to avoid redundant work.
* Common steps are reusable across projects.
* System can handle high concurrency without bottlenecks.

