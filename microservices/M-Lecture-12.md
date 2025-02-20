# **Phase 4 - Step 17: Cloud-Native Deployment with Kubernetes and CI/CD Pipelines**  

We have successfully containerized and deployed all microservices using **Docker Compose**. Now, it’s time to take it to the next level by deploying the entire system using **Kubernetes** with CI/CD pipelines. This phase will cover:  
1. **Deploying Microservices on Kubernetes** using **Kubernetes (K8s)**.  
2. **Configuring Kubernetes Objects**: Deployments, Services, ConfigMaps, and Secrets.  
3. **Setting up Ingress for API Gateway**.  
4. **Implementing CI/CD Pipelines** using **GitLab CI**.  
5. **Deploying on Cloud Platforms**: Preparing for cloud deployment on **AWS EKS** (Amazon Elastic Kubernetes Service).  

---

## **17.1. Why Use Kubernetes?**  
- **Scalability**: Easily scale microservices up and down based on demand.  
- **Resilience**: Self-healing with automatic restarts and load balancing.  
- **Portability**: Deploy on any cloud platform supporting Kubernetes.  
- **CI/CD Integration**: Seamlessly integrate with CI/CD pipelines for continuous delivery.  

---

## **17.2. Prerequisites**  
Ensure the following are installed:  
1. **Docker Desktop** (with Kubernetes enabled) or **Minikube** for local K8s cluster.  
2. **kubectl**: Command-line tool for interacting with Kubernetes.  
3. **Helm**: For managing Kubernetes packages.  
4. **GitLab CI/CD**: GitLab account with CI/CD pipeline configuration.  

---

## **17.3. Setting Up Kubernetes Cluster**  

### **Option 1: Using Docker Desktop**  
- Enable Kubernetes in Docker Desktop settings.  
- Confirm setup using:  
```bash
kubectl get nodes
```

### **Option 2: Using Minikube**  
```bash
minikube start
kubectl get nodes
```

---

## **17.4. Creating Kubernetes YAML Manifests**  
We need the following Kubernetes objects for each microservice:  
1. **Deployment**: To manage replicas and scaling.  
2. **Service**: To expose each microservice internally within the cluster.  
3. **ConfigMap**: For environment variables and configurations.  
4. **Secret**: For sensitive data (e.g., database passwords).  
5. **Ingress**: For external access via the API Gateway.  

---

### **Step 1: Create Namespace**  
We'll organize all microservices under a dedicated namespace.  

```yaml
# namespace.yml
apiVersion: v1
kind: Namespace
metadata:
  name: cashflowapp
```

Apply the namespace:  
```bash
kubectl apply -f namespace.yml
```

---

### **Step 2: Create ConfigMap**  
Centralized configuration for all microservices.

```yaml
# configmap.yml
apiVersion: v1
kind: ConfigMap
metadata:
  name: cashflowapp-config
  namespace: cashflowapp
data:
  EUREKA_SERVER: http://eureka-server:8761/eureka
  JWT_SECRET: MySecretKey
```

```bash
kubectl apply -f configmap.yml
```

---

### **Step 3: Create Secret**  
For sensitive data like database passwords.

```yaml
# secret.yml
apiVersion: v1
kind: Secret
metadata:
  name: cashflowapp-secret
  namespace: cashflowapp
type: Opaque
data:
  POSTGRES_PASSWORD: cG9zdGdyZXM=  # base64 encoded value of 'postgres'
```

```bash
kubectl apply -f secret.yml
```

---

### **Step 4: Create Deployment and Service for Each Microservice**  
We’ll create **Deployment** and **Service** for each microservice. Here is an example for **User Service**:

```yaml
# user-service-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: user-service
  namespace: cashflowapp
spec:
  replicas: 2
  selector:
    matchLabels:
      app: user-service
  template:
    metadata:
      labels:
        app: user-service
    spec:
      containers:
        - name: user-service
          image: cashflowapp/user-service:latest
          ports:
            - containerPort: 8081
          env:
            - name: EUREKA_CLIENT_SERVICEURL_DEFAULTZONE
              valueFrom:
                configMapKeyRef:
                  name: cashflowapp-config
                  key: EUREKA_SERVER
            - name: JWT_SECRET
              valueFrom:
                configMapKeyRef:
                  name: cashflowapp-config
                  key: JWT_SECRET
            - name: POSTGRES_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: cashflowapp-secret
                  key: POSTGRES_PASSWORD
---
# user-service-service.yml
apiVersion: v1
kind: Service
metadata:
  name: user-service
  namespace: cashflowapp
spec:
  ports:
    - port: 8081
      targetPort: 8081
  selector:
    app: user-service
  type: ClusterIP
```

Apply the YAML files:  
```bash
kubectl apply -f user-service-deployment.yml
kubectl apply -f user-service-service.yml
```

---

### **Step 5: Repeat for Other Microservices**  
Repeat the above step for all other microservices:  
- **Course Service**  
- **Enrollment Service**  
- **Payment Service**  
- **Notification Service**  
- **Eureka Server**  
- **API Gateway**  

---

### **Step 6: Create Ingress for API Gateway**  
We’ll expose the API Gateway using **Ingress** for external access.

```yaml
# api-gateway-ingress.yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-gateway-ingress
  namespace: cashflowapp
  annotations:
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - host: cashflowapp.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: api-gateway
                port:
                  number: 8080
```

Apply the ingress:  
```bash
kubectl apply -f api-gateway-ingress.yml
```

Update your **hosts** file:  
```plaintext
127.0.0.1 cashflowapp.local
```

Access the application via **http://cashflowapp.local**.  

---

## **17.5. Verify Deployment**  
1. **Check Running Pods and Services**  
```bash
kubectl get pods -n cashflowapp
kubectl get svc -n cashflowapp
```

2. **Check Ingress**  
```bash
kubectl get ingress -n cashflowapp
```

3. **Access Application**:  
- **API Gateway**: `http://cashflowapp.local`  
- Verify routing to all services through the gateway.  

---

## **17.6. Implement CI/CD with GitLab CI**  
We'll use **GitLab CI** to automate the build, test, and deployment process.

### **Step 1: .gitlab-ci.yml**  
Create a `.gitlab-ci.yml` file at the root of your project:

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  script:
    - mvn clean package -DskipTests
    - docker build -t cashflowapp/user-service:latest .
    - docker push cashflowapp/user-service:latest

test:
  stage: test
  script:
    - mvn test

deploy:
  stage: deploy
  script:
    - kubectl apply -f k8s/
```

- **build**: Builds the Docker image and pushes it to the Docker registry.  
- **test**: Runs unit tests.  
- **deploy**: Deploys to Kubernetes using the YAML files in the `k8s/` directory.  

---

## **Next Step**  
1. Test the **Kubernetes Deployment** with **GitLab CI/CD**.  
2. Next, we’ll **Deploy on Cloud Platforms** using **AWS EKS**.  
