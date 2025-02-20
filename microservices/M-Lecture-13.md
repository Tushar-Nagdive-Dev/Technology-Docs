# **Phase 4 - Step 18: Cloud Deployment on AWS EKS (Elastic Kubernetes Service)**  

We have successfully deployed the microservices on a local Kubernetes cluster using **Minikube** or **Docker Desktop**. Now, it's time to **deploy on the cloud** using **AWS EKS (Elastic Kubernetes Service)**. This involves:  
1. Setting up **AWS EKS** Cluster.  
2. Deploying all microservices on **AWS EKS**.  
3. Configuring **AWS RDS** for managed PostgreSQL databases.  
4. Setting up **AWS S3** for static content storage (e.g., course images).  
5. Integrating **CI/CD Pipeline** for automated deployment.  

---

## **18.1. Why AWS EKS?**  
- **Managed Kubernetes**: Fully managed Kubernetes control plane by AWS.  
- **Scalability**: Automatically scales nodes based on demand.  
- **Security**: Integrated with AWS IAM for access control.  
- **High Availability**: Multi-AZ deployment for high availability.  
- **Integration**: Native integration with other AWS services like RDS, S3, and CloudWatch.  

---

## **18.2. Prerequisites**  
Ensure the following are installed and configured:  
1. **AWS Account**: Access to the AWS Management Console.  
2. **AWS CLI**: Configured with `aws configure`.  
3. **kubectl**: Command-line tool for interacting with Kubernetes.  
4. **eksctl**: CLI tool to create and manage EKS clusters.  
5. **Helm**: For managing Kubernetes packages.  

---

## **18.3. Setting Up AWS EKS Cluster**  

### **Step 1: Install eksctl**  
Follow the [official guide](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html) to install `eksctl`. Confirm the installation using:  
```bash
eksctl version
```

### **Step 2: Create EKS Cluster**  
We will create an EKS cluster with 2 nodes using `eksctl`.

```bash
eksctl create cluster \
--name cashflowapp-cluster \
--region us-east-1 \
--nodes 2 \
--nodegroup-name standard-workers \
--node-type t3.medium \
--nodes-min 2 \
--nodes-max 4 \
--managed
```

### **Step 3: Verify Cluster Setup**  
```bash
kubectl get nodes
```

You should see the nodes in a `Ready` state.

---

## **18.4. Configuring AWS RDS for PostgreSQL**  
We will use **AWS RDS** for managed PostgreSQL databases for all microservices.

### **Step 1: Create RDS Instance**  
- Go to **AWS Management Console** → **RDS** → **Create Database**.  
- Choose **PostgreSQL** as the engine.  
- Select **Multi-AZ deployment** for high availability.  
- Set **Public accessibility** to **No** (for security).  
- Configure the **VPC and Subnet** to be the same as the EKS cluster.  
- **Create database**.

### **Step 2: Security Group Configuration**  
- Ensure the **Security Group** allows traffic from the EKS nodes.  
- Allow inbound traffic on **port 5432** (PostgreSQL default port).  

### **Step 3: Update application.properties**  
Update the `application.properties` files in all microservices to point to the RDS instance.  

```properties
spring.datasource.url=jdbc:postgresql://<RDS-ENDPOINT>:5432/<DATABASE-NAME>
spring.datasource.username=<USERNAME>
spring.datasource.password=<PASSWORD>
```

---

## **18.5. Configuring S3 for Static Content**  
We'll use **AWS S3** for storing static content like course images.  

### **Step 1: Create S3 Bucket**  
- Go to **AWS Management Console** → **S3** → **Create Bucket**.  
- Name the bucket (e.g., `cashflowapp-static-content`).  
- Enable **Bucket Versioning** for tracking changes.  
- Enable **Public Access** if you want direct access to images (not recommended for sensitive data).  

### **Step 2: IAM Role and Policy**  
- Create an **IAM Role** for EKS with S3 access.  
- Attach the following policy to allow S3 access:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket",
                "s3:GetObject",
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::cashflowapp-static-content",
                "arn:aws:s3:::cashflowapp-static-content/*"
            ]
        }
    ]
}
```

---

## **18.6. Creating Kubernetes Secrets for AWS Credentials**  
We'll create Kubernetes secrets to securely store AWS credentials for accessing RDS and S3.

```bash
kubectl create secret generic aws-credentials \
--from-literal=AWS_ACCESS_KEY_ID=<YOUR_ACCESS_KEY_ID> \
--from-literal=AWS_SECRET_ACCESS_KEY=<YOUR_SECRET_ACCESS_KEY> \
--namespace cashflowapp
```

Update the microservices' deployment YAMLs to use these secrets.

---

## **18.7. Deploying Microservices on AWS EKS**  

### **Step 1: Update Kubernetes Manifests**  
Update the following in your Kubernetes YAML files:  
1. **Database Connection**: Point to the RDS endpoint.  
2. **Static Content**: Point to the S3 bucket.  
3. **Secrets**: Use the `aws-credentials` secret.  

### **Step 2: Deploy Microservices**  
```bash
kubectl apply -f namespace.yml
kubectl apply -f configmap.yml
kubectl apply -f secret.yml
kubectl apply -f user-service-deployment.yml
kubectl apply -f course-service-deployment.yml
kubectl apply -f enrollment-service-deployment.yml
kubectl apply -f payment-service-deployment.yml
kubectl apply -f notification-service-deployment.yml
kubectl apply -f api-gateway-deployment.yml
kubectl apply -f eureka-server-deployment.yml
```

### **Step 3: Verify Deployment**  
```bash
kubectl get pods -n cashflowapp
kubectl get svc -n cashflowapp
```

---

## **18.8. Configuring Ingress for External Access**  
We’ll use **AWS ALB Ingress Controller** for exposing the API Gateway.  

### **Step 1: Install AWS ALB Ingress Controller**  
```bash
helm repo add eks https://aws.github.io/eks-charts
helm install alb-ingress-controller eks/aws-load-balancer-controller \
    --namespace kube-system \
    --set clusterName=cashflowapp-cluster
```

### **Step 2: Create Ingress for API Gateway**  
```yaml
# api-gateway-ingress.yml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: api-gateway-ingress
  namespace: cashflowapp
  annotations:
    alb.ingress.kubernetes.io/scheme: internet-facing
spec:
  rules:
    - host: api.cashflowapp.com
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

### **Step 3: Verify Ingress**  
```bash
kubectl get ingress -n cashflowapp
```

Access the application via **https://api.cashflowapp.com**.  

---

## **18.9. CI/CD Integration for Cloud Deployment**  
We’ll use **GitLab CI/CD** for continuous deployment to **AWS EKS**.

### **Step 1: Update .gitlab-ci.yml**  
```yaml
deploy:
  stage: deploy
  script:
    - aws eks update-kubeconfig --name cashflowapp-cluster --region us-east-1
    - kubectl apply -f k8s/
```

### **Step 2: GitLab CI/CD Configuration**  
- Add **AWS_ACCESS_KEY_ID** and **AWS_SECRET_ACCESS_KEY** as GitLab CI/CD variables.  
- Configure the **deploy** stage to trigger automatically on `main` branch push.  

---

## **Next Step**  
1. Test the **AWS EKS Deployment** with **GitLab CI/CD**.  
2. Next, we’ll move on to **Phase 5: Advanced Monitoring, Logging, and Security**.  
