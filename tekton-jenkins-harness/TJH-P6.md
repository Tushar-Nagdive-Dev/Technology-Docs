1. Securely store & use credentials in **Jenkins**, **Tekton**, and **Harness**.
2. Implement **image signing** and **vulnerability scanning**.
3. Add compliance checks (license scan, SBOM generation).

---

## **Step 1 — Secure Secrets Management**

### 1.1 Jenkins

* Go to **Manage Jenkins → Credentials → Global** → Add credentials.
* Reference in `Jenkinsfile`:

```groovy
withCredentials([string(credentialsId: 'dockerhub-token', variable: 'TOKEN')]) {
    sh 'echo $TOKEN'
}
```

* For sensitive data like API keys, always mask output with `withCredentials`.

### 1.2 Tekton

* Store secrets as Kubernetes `Secret`:

```bash
kubectl create secret generic dockerhub-creds \
  --from-literal=username=<user> \
  --from-literal=password=<pass>
```

* Reference in `Task` via `envFrom` or `volumeMounts`.

### 1.3 Harness

* Go to **Security → Secrets Manager**.
* Add secret (Text, File, SSH Key, API Key).
* Reference in pipeline step with `${secrets.getValue("my-secret")}`.

---

## **Step 2 — Image Signing (Provenance)**

### Cosign Setup

* Install [Cosign](https://docs.sigstore.dev/cosign/overview/).

```bash
cosign generate-key-pair
cosign sign --key cosign.key tushar/petclinic:1.0
cosign verify --key cosign.pub tushar/petclinic:1.0
```

* Integrate into pipelines:

  * **Jenkins**: Add stage after Docker push:

    ```groovy
    sh "cosign sign --key cosign.key $DOCKER_IMAGE:$DOCKER_TAG"
    ```
  * **Tekton**: Use `cosign` image in a step.
  * **Harness**: Run cosign in a Shell Script step after push.

---

## **Step 3 — Vulnerability Scanning**

### Using Trivy

* Install [Trivy](https://aquasecurity.github.io/trivy/).
* Scan Docker image:

```bash
trivy image tushar/petclinic:1.0
```

* Fail build on high severity vulnerabilities:

  * **Jenkins**:

    ```groovy
    sh "trivy image --exit-code 1 --severity HIGH $DOCKER_IMAGE:$DOCKER_TAG"
    ```
  * **Tekton**: Add as a step before deploy.
  * **Harness**: Add "Run Step" with same command.

---

## **Step 4 — License Compliance Check**

* Use [Syft](https://github.com/anchore/syft) to generate SBOM:

```bash
syft tushar/petclinic:1.0 -o json > sbom.json
```

* Integrate into pipeline & fail if disallowed licenses found.
* Store SBOM in artifact storage for audits.

---

## **Step 5 — Governance Rules**

* **Jenkins**:

  * Enforce minimum code coverage.
  * Use `pipeline-utility-steps` to validate YAML/JSON.
* **Tekton**:

  * Use OPA Gatekeeper policies on `PipelineRun` resources.
* **Harness**:

  * Use built-in **Governance Policies** to block deployments not meeting criteria.

---

## ✅ Lesson 5 Completion Criteria

* All three tools store secrets securely (no plain-text in repos).
* Pipelines sign Docker images after build.
* Vulnerability scans fail builds on critical findings.
* SBOM is generated & stored for compliance.
