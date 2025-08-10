
1. **Jenkins**, **Tekton**, and **Harness** automatically triggered on Git commits.
2. Quality gates that fail builds if tests fail or coverage drops below a set threshold.

---

## **Step 1 — Jenkins: Webhooks + Quality Gates**

### 1.1 GitHub Webhook Setup

1. Go to your GitHub repo → **Settings → Webhooks → Add Webhook**

   * **Payload URL:** `http://<your-public-ip>:8080/github-webhook/`
     *(Use [ngrok](https://ngrok.com/) if Jenkins is local: `ngrok http 8080`)*
   * Content type: `application/json`
   * Trigger: Just the push event (for now).
2. Install **GitHub Integration** plugin in Jenkins.
3. In your Jenkins job:

   * General → Check **GitHub project** (add repo URL)
   * Build Triggers → Select **GitHub hook trigger for GITScm polling**.

---

### 1.2 Quality Gates in Jenkins

Add **Jacoco** plugin for coverage reports:

1. Install **Jacoco Plugin**.
2. Update your `Jenkinsfile`:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') { steps { checkout scm } }
        stage('Build') { steps { sh 'mvn clean package' } }
        stage('Test') { steps { sh 'mvn test' } }
        stage('Coverage') {
            steps {
                jacoco execPattern: '**/jacoco.exec'
                script {
                    def coverage = 75 // minimum required %
                    def result = sh(returnStdout: true, script: "grep -oP '(?<=<counter type=\"LINE\" missed=\")[0-9]+(?=\" covered=\")' target/site/jacoco/jacoco.xml | awk '{sum+=\$1} END {print sum}'").trim()
                    if (result.toInteger() < coverage) {
                        error "Coverage below ${coverage}%!"
                    }
                }
            }
        }
    }
    post { always { junit '**/target/surefire-reports/*.xml' } }
}
```

This fails the build if coverage drops below 75%.

---

## **Step 2 — Tekton: Webhooks + Quality Gates**

### 2.1 Tekton Triggers for GitHub

Install Tekton Triggers:

```bash
kubectl apply -f https://storage.googleapis.com/tekton-releases/triggers/latest/release.yaml
```

Create `trigger-binding.yaml`:

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerBinding
metadata:
  name: github-push-binding
spec:
  params:
    - name: gitrepositoryurl
      value: $(body.repository.clone_url)
    - name: gitrevision
      value: $(body.head_commit.id)
```

Create `trigger-template.yaml`:

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: TriggerTemplate
metadata:
  name: github-push-template
spec:
  params:
    - name: gitrepositoryurl
    - name: gitrevision
  resourcetemplates:
    - apiVersion: tekton.dev/v1
      kind: PipelineRun
      metadata:
        generateName: petclinic-run-
      spec:
        pipelineRef:
          name: petclinic-pipeline
        workspaces:
          - name: source
            emptyDir: {}
        params:
          - name: gitrepositoryurl
            value: $(tt.params.gitrepositoryurl)
          - name: gitrevision
            value: $(tt.params.gitrevision)
```

Create EventListener:

```yaml
apiVersion: triggers.tekton.dev/v1beta1
kind: EventListener
metadata:
  name: github-listener
spec:
  serviceAccountName: tekton-triggers-admin
  triggers:
    - name: github-trigger
      bindings:
        - ref: github-push-binding
      template:
        ref: github-push-template
```

Expose via `kubectl port-forward` or Ingress, then configure GitHub webhook to point to `/`.

---

### 2.2 Quality Gates in Tekton

Add a `coverage-check` step to your `Task`:

```yaml
- name: coverage-check
  image: alpine:3.18
  script: |
    # Check if coverage file exists
    if [ ! -f target/site/jacoco/index.html ]; then
      echo "Coverage report missing"
      exit 1
    fi
    echo "Coverage OK"
```

You can parse coverage XML to enforce thresholds.

---

## **Step 3 — Harness: Webhooks + Quality Gates**

### 3.1 GitHub Trigger in Harness

1. In Harness → **Triggers** → New Trigger.
2. Select **Webhook** → GitHub → Event: Push.
3. Link to pipeline (CI or CD).

---

### 3.2 Quality Gates in Harness

Harness supports **Run Step** for custom scripts:

* Add a step after Test:

```bash
#!/bin/bash
coverage=$(grep -oP '(?<=<counter type="LINE" missed=")[0-9]+' target/site/jacoco/jacoco.xml | awk '{sum+=$1} END {print sum}')
if [ "$coverage" -lt 75 ]; then
  echo "Coverage below 75%"
  exit 1
fi
```

You can also integrate **SonarQube** as a quality gate.

---

## ✅ Lesson 2 Completion Criteria

* Each tool triggers a build automatically when you push code.
* Coverage < 75% fails the build.
* Build reports show coverage results.
