# 🚀 **Phase 2 - Lesson 20: Deploying Spring Boot on AWS EKS with Helm & Terraform**  

## **📌 Lesson Objective**  
By the end of this lesson, you will:  
✅ Understand **AWS EKS (Elastic Kubernetes Service) & why it's used**  
✅ Deploy a **Spring Boot app on AWS EKS**  
✅ Use **Helm for Kubernetes package management**  
✅ Automate **infrastructure deployment using Terraform**  
✅ Scale & manage **Spring Boot microservices in production**  

---

## **1️⃣ What is AWS EKS & Why Use It?**  
📌 **AWS EKS (Elastic Kubernetes Service)** is a **managed Kubernetes service** that automates cluster setup, scaling, and maintenance.  

### **🔥 Why Use AWS EKS for Spring Boot?**
✔ **Fully Managed Kubernetes** – No need to manage control plane nodes.  
✔ **Auto Scaling** – Scales microservices based on demand.  
✔ **Secure** – Integrated with AWS IAM, VPC, and Security Groups.  
✔ **Cost-Effective** – Optimized for both small and large-scale applications.  

✅ **Use AWS EKS when:**  
- You run **microservices that need high availability & scalability**.  
- You want **managed Kubernetes infrastructure on AWS**.  

---

# ☁ **Part 1: Setting Up AWS EKS Cluster using Terraform**  

## **2️⃣ Installing Required Tools**  
📌 **Step 1: Install AWS CLI, kubectl, Terraform & Helm**  
- **AWS CLI** → [Install AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html)  
- **kubectl** → [Install kubectl](https://kubernetes.io/docs/tasks/tools/)  
- **Terraform** → [Install Terraform](https://developer.hashicorp.com/terraform/downloads)  
- **Helm** → [Install Helm](https://helm.sh/docs/intro/install/)  

📌 **Step 2: Configure AWS CLI**  
```bash
aws configure
```
Enter:  
- **AWS Access Key ID**  
- **AWS Secret Access Key**  
- **Region (e.g., us-east-1)**  

---

## **3️⃣ Provisioning AWS EKS Cluster with Terraform**  
📌 **Step 1: Create a Terraform Configuration (`eks-cluster.tf`)**  
```hcl
provider "aws" {
  region = "us-east-1"
}

resource "aws_eks_cluster" "eks" {
  name     = "spring-boot-eks"
  role_arn = "arn:aws:iam::your-account-id:role/eks-cluster-role"

  vpc_config {
    subnet_ids = ["subnet-abc123", "subnet-def456"]  # Replace with your AWS subnets
  }
}

resource "aws_eks_node_group" "node_group" {
  cluster_name  = aws_eks_cluster.eks.name
  node_role_arn = "arn:aws:iam::your-account-id:role/eks-node-group-role"
  subnet_ids    = aws_eks_cluster.eks.vpc_config[0].subnet_ids
  instance_types = ["t3.medium"]
  scaling_config {
    desired_size = 2
    max_size     = 4
    min_size     = 1
  }
}
```

📌 **Step 2: Deploy EKS Cluster using Terraform**  
```bash
terraform init
terraform apply -auto-approve
```
✔ **Terraform creates an AWS EKS cluster with worker nodes!** 🎉  

📌 **Step 3: Configure `kubectl` to Use the New EKS Cluster**  
```bash
aws eks update-kubeconfig --name spring-boot-eks --region us-east-1
kubectl get nodes
```
✔ Now **kubectl is connected to AWS EKS!** 🎉  

---

# 🛠 **Part 2: Deploying Spring Boot Microservices on EKS**  

## **4️⃣ Deploying a Spring Boot App on AWS EKS**  
📌 **Step 1: Create a Kubernetes Deployment (`spring-boot-deployment.yaml`)**  
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: spring-boot-app
spec:
  replicas: 3
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

📌 **Step 2: Create a Kubernetes Service (`spring-boot-service.yaml`)**  
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
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

📌 **Step 3: Apply the Deployment & Service**  
```bash
kubectl apply -f spring-boot-deployment.yaml
kubectl apply -f spring-boot-service.yaml
kubectl get services
```
✔ Your **Spring Boot app is now running on AWS EKS!** 🚀  

📌 **Step 4: Access the Spring Boot App**  
```bash
curl -X GET "http://your-eks-loadbalancer/api/hello"
```
✔ Now **Spring Boot is accessible via AWS LoadBalancer!** 🎉  

---

# ⚙ **Part 3: Managing Kubernetes Deployments with Helm**  

## **5️⃣ Using Helm to Manage Spring Boot Deployments**  
📌 **Step 1: Create a Helm Chart**  
```bash
helm create spring-boot-chart
cd spring-boot-chart
```

📌 **Step 2: Modify `values.yaml` for Spring Boot App**  
```yaml
replicaCount: 3

image:
  repository: your-dockerhub-username/spring-boot-app
  tag: latest
  pullPolicy: Always

service:
  type: LoadBalancer
  port: 80
```

📌 **Step 3: Install Spring Boot App using Helm**  
```bash
helm install spring-app ./spring-boot-chart
helm list
```
✔ Helm now **manages deployments easily**! 🎉  

---

# 🔄 **Part 4: Autoscaling Spring Boot with Kubernetes**  

## **6️⃣ Configuring Kubernetes Horizontal Pod Autoscaler**  
📌 **Step 1: Enable Autoscaling for Spring Boot App**  
```bash
kubectl autoscale deployment spring-boot-app --cpu-percent=50 --min=2 --max=5
kubectl get hpa
```
✔ Now Kubernetes will **auto-scale Spring Boot pods** when CPU usage exceeds 50%! 🚀  

---

# 🎯 **Lesson 20 - Summary**  
✅ Deployed **Spring Boot on AWS EKS using Terraform**  
✅ Created **Kubernetes Deployment & Service for Spring Boot**  
✅ Used **Helm to manage Kubernetes applications**  
✅ Configured **Auto Scaling in Kubernetes**  

---
