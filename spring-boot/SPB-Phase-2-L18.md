# 🚀 **Phase 2 - Lesson 18: Deploying Spring Boot Apps on Kubernetes (K8s for Scaling & Management)**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **what Kubernetes (K8s) is and why it's used**  
✅ Learn how to **deploy a Spring Boot app on Kubernetes**  
✅ Use **Kubernetes YAML files** for defining deployments & services  
✅ Set up **Kubernetes Ingress for external access**  
✅ Scale your **Spring Boot application using Kubernetes**  

---

## **1️⃣ What is Kubernetes & Why Use It?**  
📌 **Kubernetes (K8s)** is a container orchestration system that **automates deployment, scaling, and management** of containerized applications.  

### **🔥 Why Use Kubernetes for Microservices?**
✔ **Automatic Scaling** – Handles traffic spikes automatically.  
✔ **Self-Healing** – Restarts failed containers automatically.  
✔ **Rolling Updates** – Deploys new versions without downtime.  
✔ **Load Balancing** – Distributes traffic across instances.  

✅ **Use Kubernetes when:**  
- You need **auto-scaling & high availability**.  
- You manage **multiple microservices** efficiently.  

---

## **2️⃣ Installing Kubernetes (Minikube for Local Testing)**  
📌 **Step 1: Install Minikube & kubectl**  
- **Install Minikube** → [Minikube Setup Guide](https://minikube.sigs.k8s.io/docs/start/)  
- **Install kubectl** → [Kubectl Setup Guide](https://kubernetes.io/docs/tasks/tools/)  

📌 **Step 2: Start Minikube**  
```bash
minikube start
```

📌 **Step 3: Verify Minikube & kubectl are Running**  
```bash
kubectl version --client
minikube status
```
✔ Expected output: `Minikube running`  

---

## **3️⃣ Writing Kubernetes Deployment & Service for Spring Boot App**  
📌 **Step 1: Create `deployment.yaml` for Spring Boot App**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-boot-app
spec:
  replicas: 2  # Runs 2 instances for high availability
  selector:
    matchLabels:
      app: spring-boot-app
  template:
    metadata:
      labels:
        app: spring-boot-app
    spec:
      containers:
        - name: spring-boot-app
          image: your-dockerhub-username/spring-boot-app:latest
          ports:
            - containerPort: 8080
```

📌 **Step 2: Create `service.yaml` for Internal Networking**  
```yaml
apiVersion: v1
kind: Service
metadata:
  name: spring-boot-service
spec:
  selector:
    app: spring-boot-app
  ports:
    - protocol: TCP
      port: 80  # External port
      targetPort: 8080  # Internal app port
  type: NodePort
```

📌 **Step 3: Apply Kubernetes Configuration**  
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
✔ Now Kubernetes is running **2 instances** of your Spring Boot app! 🚀  

---

## **4️⃣ Exposing the Application with Kubernetes Ingress**  
📌 **Step 1: Enable Ingress in Minikube**  
```bash
minikube addons enable ingress
```

📌 **Step 2: Create `ingress.yaml` for External Access**  
```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: spring-boot-ingress
spec:
  rules:
    - host: springboot.local
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: spring-boot-service
                port:
                  number: 80
```

📌 **Step 3: Apply Ingress Rules**  
```bash
kubectl apply -f ingress.yaml
```

📌 **Step 4: Add an Entry to `/etc/hosts`** (Linux/Mac)  
```bash
sudo nano /etc/hosts
```
Add the following line:  
```
127.0.0.1 springboot.local
```

📌 **Step 5: Access the Application**  
```bash
curl -X GET "http://springboot.local/api/hello"
```
🎉 **Spring Boot is now deployed with Kubernetes Ingress!**  

---

## **5️⃣ Scaling & Managing Spring Boot with Kubernetes**  
📌 **Step 1: Scale Up to 4 Instances**  
```bash
kubectl scale deployment spring-boot-app --replicas=4
```
✔ Kubernetes now runs **4 instances** of the app for high availability.  

📌 **Step 2: View Running Pods**  
```bash
kubectl get pods
```

📌 **Step 3: Delete a Pod (Auto-Recovery Test)**  
```bash
kubectl delete pod <POD_NAME>
```
✔ Kubernetes will **automatically restart** the deleted pod.  

---

## **6️⃣ Rolling Updates & Zero Downtime Deployment**  
📌 **Step 1: Update Image Version in `deployment.yaml`**  
```yaml
containers:
  - name: spring-boot-app
    image: your-dockerhub-username/spring-boot-app:v2  # New Version
```

📌 **Step 2: Apply Rolling Update**  
```bash
kubectl apply -f deployment.yaml
```

📌 **Step 3: Monitor the Rolling Update**  
```bash
kubectl rollout status deployment spring-boot-app
```

📌 **Step 4: Rollback if Needed**  
```bash
kubectl rollout undo deployment spring-boot-app
```
🎉 **Now your app updates without downtime!** 🚀  

---

## 🎯 **Lesson 18 - Summary**  
✅ Deployed **Spring Boot on Kubernetes (K8s)**  
✅ Used **Kubernetes Deployments & Services**  
✅ Set up **Ingress for external access**  
✅ Scaled the **app to multiple instances**  
✅ Performed **rolling updates without downtime**  

---
