# **Phase 5 - Step 19: Advanced Monitoring, Logging, and Security**  

We have successfully deployed the microservices on **AWS EKS** with **GitLab CI/CD** for continuous deployment. Now, it’s time to enhance the system's **observability and security**. This involves:  
1. Implementing **Monitoring and Alerting** with **Prometheus** and **Grafana**.  
2. Setting up **Centralized Logging** with the **ELK Stack (Elasticsearch, Logstash, and Kibana)**.  
3. Enabling **Distributed Tracing** with **Jaeger**.  
4. Implementing **Security Best Practices** including mTLS, OAuth2, and API Gateway security.  

---

## **19.1. Why Monitoring, Logging, and Security?**  
- **Monitoring** provides real-time insights into system health and performance.  
- **Logging** helps in debugging and auditing by maintaining application logs.  
- **Tracing** tracks request flows across microservices.  
- **Security** protects sensitive data and prevents unauthorized access.  

---

## **19.2. Implementing Monitoring and Alerting**  

We will use **Prometheus** for metrics collection and **Grafana** for visualization.

---

### **Step 1: Install Prometheus and Grafana using Helm**  

We will use **Helm** to install **Prometheus** and **Grafana** on the **EKS Cluster**.

```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

```bash
helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
```

### **Step 2: Accessing Prometheus and Grafana**  

Forward the ports to access Prometheus and Grafana dashboards:  

```bash
kubectl port-forward -n monitoring svc/prometheus-kube-prometheus-prometheus 9090
kubectl port-forward -n monitoring svc/prometheus-grafana 3000
```

- Access **Prometheus** at: `http://localhost:9090`  
- Access **Grafana** at: `http://localhost:3000`  
  - **Default Credentials**:  
    - Username: `admin`  
    - Password: `prom-operator`

---

### **Step 3: Configure Dashboards in Grafana**  

- Import the following **Grafana Dashboards**:  
  - **Kubernetes Cluster Monitoring** (ID: 315)  
  - **Node Exporter Full** (ID: 1860)  
  - **API Gateway Monitoring** (custom dashboard for Spring Cloud Gateway)  

### **Step 4: Setting Up Alerts**  

- Configure **Alerting Rules** in Prometheus:  
  - **High CPU Usage**  
  - **Memory Leaks**  
  - **Service Unavailability**  

Example alert rule for **High CPU Usage**:  

```yaml
groups:
- name: kubernetes-cpu-usage
  rules:
  - alert: HighCPUUsage
    expr: sum(rate(container_cpu_usage_seconds_total[1m])) by (pod) > 0.8
    for: 5m
    labels:
      severity: warning
    annotations:
      summary: "High CPU usage on {{ $labels.pod }}"
      description: "{{ $labels.pod }} is using over 80% CPU for the last 5 minutes."
```

Apply the alert rule:  
```bash
kubectl apply -f prometheus-alert-rules.yml
```

---

## **19.3. Centralized Logging with ELK Stack**  

We will use the **ELK Stack** (Elasticsearch, Logstash, and Kibana) for centralized logging.  

### **Step 1: Install ELK Stack using Helm**  

```bash
helm repo add elastic https://helm.elastic.co
helm repo update
```

```bash
helm install elasticsearch elastic/elasticsearch \
  --namespace logging \
  --create-namespace
```

```bash
helm install kibana elastic/kibana \
  --namespace logging
```

### **Step 2: Forward Kibana Port**  

```bash
kubectl port-forward -n logging svc/kibana-kibana 5601
```

Access **Kibana** at: `http://localhost:5601`.

---

### **Step 3: Configuring Logstash**  

We will use **Fluentd** as the log forwarder to **Logstash**.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: fluentd-config
  namespace: logging
data:
  fluent.conf: |
    <source>
      @type tail
      path /var/log/containers/*.log
      pos_file /var/log/es-containers.log.pos
      tag kube.*
      format json
    </source>
    <match kube.**>
      @type elasticsearch
      host elasticsearch-master
      port 9200
      logstash_format true
      include_tag_key true
      type_name access_log
    </match>
```

Apply Fluentd:  
```bash
kubectl apply -f fluentd-config.yml
kubectl apply -f fluentd-daemonset.yml
```

### **Step 4: Analyzing Logs in Kibana**  
- Go to **Kibana Dashboard**.  
- Create an **Index Pattern** for `kube-*`.  
- Analyze logs and set up alerts for errors and warnings.  

---

## **19.4. Distributed Tracing with Jaeger**  

We will use **Jaeger** for distributed tracing of requests across microservices.  

### **Step 1: Install Jaeger using Helm**  

```bash
helm repo add jaegertracing https://jaegertracing.github.io/helm-charts
helm repo update
```

```bash
helm install jaeger jaegertracing/jaeger \
  --namespace tracing \
  --create-namespace
```

### **Step 2: Integrate Jaeger with Spring Boot**  
Add the following dependencies to all microservices:

```xml
<dependency>
  <groupId>io.opentracing.contrib</groupId>
  <artifactId>opentracing-spring-jaeger-cloud-starter</artifactId>
  <version>3.2.0</version>
</dependency>
```

### **Step 3: Accessing Jaeger UI**  
```bash
kubectl port-forward -n tracing svc/jaeger-query 16686
```

Access **Jaeger UI** at: `http://localhost:16686`.  

- Track requests across microservices.  
- Identify performance bottlenecks and latency issues.  

---

## **19.5. Security Best Practices**  

### **1. Mutual TLS (mTLS)**  
- Implement mTLS for secure communication between microservices.  
- Use **Cert-Manager** for automatic certificate management.

### **2. OAuth2 and OpenID Connect**  
- Use **Keycloak** or **AWS Cognito** for centralized authentication and authorization.  
- Integrate with **Spring Security** for role-based access control.

### **3. API Gateway Security**  
- Implement **Rate Limiting** and **IP Whitelisting** in **Spring Cloud Gateway**.  
- Secure API Gateway using **OAuth2** and **JWT Validation**.

---

## **19.6. Verify and Test**  

1. **Check Monitoring and Alerts**: Verify metrics and alerts in **Grafana** and **Prometheus**.  
2. **Check Logs**: Analyze logs in **Kibana**.  
3. **Check Tracing**: View request traces in **Jaeger**.  
4. **Check Security**: Test mTLS and OAuth2 flows using **Postman**.  

---

## **Next Step**  
1. Test the entire monitoring, logging, tracing, and security setup.  
2. Next, we’ll move on to **Phase 6: Performance Optimization and Scaling**.  
