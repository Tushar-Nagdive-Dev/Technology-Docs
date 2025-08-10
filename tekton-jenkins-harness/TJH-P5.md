## **Step 1 — Understanding Deployment Strategies**

### **1.1 Rolling Deployment** (default)

* Updates pods gradually with new version.
* No downtime, but partial users may see mixed versions.

### **1.2 Blue-Green Deployment**

* Two identical environments (Blue=current, Green=new).
* Traffic switches instantly to Green after validation.
* Rollback = instant switch back to Blue.
* Best when you need fast rollback & zero downtime.

### **1.3 Canary Deployment**

* Releases to a subset of users (e.g., 10%, then 50%, then 100%).
* Lets you monitor metrics before full rollout.
* Best for risk reduction & real-user testing.

### **1.4 Progressive Delivery**

* Automated Canary using metric checks (Prometheus, Datadog).
* Stops rollout if errors or latency increase.

---

## **Step 2 — Blue-Green in Jenkins**

**Helm Chart Support**
In `values.yaml`:

```yaml
service:
  type: ClusterIP
  port: 8080

deployment:
  image:
    repository: tushar/petclinic
    tag: latest
  color: blue
```

**Jenkinsfile Deployment Stage**

```groovy
stage('Blue-Green Deploy') {
    steps {
        sh """
          CURRENT_COLOR=$(kubectl get svc petclinic -o=jsonpath='{.spec.selector.color}')
          if [ "$CURRENT_COLOR" = "blue" ]; then
            NEW_COLOR=green
          else
            NEW_COLOR=blue
          fi
          helm upgrade --install petclinic-$NEW_COLOR ./helm \
            --set image.repository=$DOCKER_IMAGE \
            --set image.tag=$DOCKER_TAG \
            --set deployment.color=$NEW_COLOR
          # Switch traffic
          kubectl patch svc petclinic -p '{"spec":{"selector":{"color":"'$NEW_COLOR'"}}}'
        """
    }
}
```

---

## **Step 3 — Canary in Tekton**

Add Canary deployment step:

```yaml
- name: canary-deploy
  image: alpine/helm:3.12.0
  script: |
    helm upgrade --install petclinic ./helm \
      --set image.repository=$DOCKER_IMAGE \
      --set image.tag=$DOCKER_TAG \
      --set canary.enabled=true \
      --set canary.weight=10
```

You can adjust `weight` in multiple runs to gradually increase.

---

## **Step 4 — Harness Advanced Strategies**

Harness has **built-in deployment strategies**:

1. In your CD stage, select **Deployment Type → Kubernetes**.
2. Choose strategy:

   * **Rolling**
   * **Blue-Green**
   * **Canary**
3. For Canary:

   * Stage 1: Deploy to 10% pods → Verify → Pause.
   * Stage 2: Deploy to 50% → Verify → Pause.
   * Stage 3: Deploy 100%.
4. Add **Continuous Verification** step:

   * Connect Prometheus/Datadog.
   * Fail pipeline if error rate/latency exceeds threshold.

---

## **Step 5 — Rollback Automation**

### Jenkins:

```groovy
stage('Rollback') {
    when { expression { currentBuild.result == 'FAILURE' } }
    steps {
        sh "helm rollback petclinic 1" // Rollback to revision 1
    }
}
```

### Tekton:

Add a rollback task triggered by a failure result in pipeline.

### Harness:

Rollback is built-in; configure **On Failure → Rollback Previous Deployment** in pipeline settings.

---

## **Step 6 — Verification Hooks**

* **Jenkins**:

  * Add a stage after deploy to run smoke tests:

    ```groovy
    sh "curl -f http://petclinic.example.com/actuator/health"
    ```
* **Tekton**:

  * Add `curl` or `k6` load test step in pipeline.
* **Harness**:

  * Use built-in Verification step with monitoring tools.

---

## ✅ Lesson 4 Completion Criteria

* You can do Blue-Green, Canary, and Rolling deployments in all three tools.
* Automated rollback is configured.
* Basic verification is in place.
