# **Phase 6 - Step 20: Performance Optimization and Scaling**  

We have successfully implemented **Monitoring**, **Logging**, **Tracing**, and **Security** for the microservices architecture on **AWS EKS**. Now, it's time to optimize performance and ensure scalability. This phase involves:  
1. **Performance Tuning**: Optimizing application performance and reducing latency.  
2. **Autoscaling**: Implementing horizontal and vertical scaling with **Kubernetes HPA** and **VPA**.  
3. **Load Balancing**: Efficiently distributing traffic using **AWS ALB** and **Nginx Ingress Controller**.  
4. **Caching**: Implementing caching mechanisms with **Redis** and **CDN**.  
5. **Database Optimization**: Fine-tuning PostgreSQL and MongoDB for better performance.  
6. **CI/CD Performance Testing**: Automating performance testing in CI/CD pipelines.  

---

## **20.1. Why Performance Optimization and Scaling?**  
- **Performance Tuning** enhances the user experience by reducing latency and response times.  
- **Scaling** ensures the system can handle increased traffic and workload efficiently.  
- **Load Balancing** distributes requests evenly, preventing overloading of specific services.  
- **Caching** reduces database load and speeds up data retrieval.  
- **Database Optimization** improves query performance and reduces latency.  

---

## **20.2. Performance Tuning**  

### **Step 1: JVM and Container Optimization**  
- Optimize **JVM Memory Settings** for Spring Boot microservices.  
- Use **G1GC (Garbage First Garbage Collector)** for improved garbage collection.  

Example JVM Options:  
```yaml
JAVA_OPTS: "-Xms512m -Xmx1024m -XX:+UseG1GC -XX:MaxGCPauseMillis=200"
```

### **Step 2: Thread Pool Configuration**  
- Configure thread pools for REST endpoints to handle high concurrent requests.  

Example configuration for Spring Boot:  
```yaml
spring.task.execution.pool.core-size=10
spring.task.execution.pool.max-size=50
spring.task.execution.pool.queue-capacity=100
```

### **Step 3: Connection Pooling**  
- Use **HikariCP** as the connection pool for PostgreSQL to manage database connections efficiently.  

Example configuration:  
```yaml
spring.datasource.hikari.maximum-pool-size=30
spring.datasource.hikari.minimum-idle=10
spring.datasource.hikari.idle-timeout=30000
spring.datasource.hikari.max-lifetime=60000
```

### **Step 4: API Gateway Performance**  
- Enable **caching** and **rate limiting** in Spring Cloud Gateway.  
- Enable **Circuit Breaker** for resilience using **Resilience4j**.  

Example configuration:  
```yaml
spring.cloud.gateway.routes[0].id=course-service
spring.cloud.gateway.routes[0].uri=http://course-service:8082
spring.cloud.gateway.routes[0].predicates[0]=Path=/api/courses/**
spring.cloud.gateway.routes[0].filters[0]=RequestRateLimiter=1, 2
spring.cloud.gateway.routes[0].filters[1]=CircuitBreaker=name=courseCB, fallbackUri=/fallback
```

---

## **20.3. Autoscaling with Kubernetes**  

We will implement both **Horizontal Pod Autoscaling (HPA)** and **Vertical Pod Autoscaling (VPA)**.  

### **Step 1: Horizontal Pod Autoscaling (HPA)**  
HPA automatically scales the number of pods based on CPU and memory utilization.

Example HPA for **User Service**:  
```yaml
# hpa-user-service.yml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: user-service-hpa
  namespace: cashflowapp
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: user-service
  minReplicas: 2
  maxReplicas: 10
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 50
```

Apply the HPA:  
```bash
kubectl apply -f hpa-user-service.yml
```

### **Step 2: Vertical Pod Autoscaling (VPA)**  
VPA adjusts CPU and memory requests for pods.

```yaml
# vpa-user-service.yml
apiVersion: autoscaling.k8s.io/v1
kind: VerticalPodAutoscaler
metadata:
  name: user-service-vpa
  namespace: cashflowapp
spec:
  targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: user-service
  updatePolicy:
    updateMode: Auto
```

Apply the VPA:  
```bash
kubectl apply -f vpa-user-service.yml
```

### **Step 3: Monitor Scaling Activity**  
```bash
kubectl get hpa -n cashflowapp
kubectl get vpa -n cashflowapp
```

---

## **20.4. Load Balancing with AWS ALB and Nginx Ingress**  

### **Option 1: AWS ALB Ingress Controller**  
- Already configured in **Phase 4** using `alb-ingress-controller`.  
- Verify ALB status:  
```bash
kubectl get ingress -n cashflowapp
```

### **Option 2: Nginx Ingress Controller**  
We can also use **Nginx Ingress Controller** for advanced routing and load balancing.  

```bash
helm repo add nginx-stable https://helm.nginx.com/stable
helm repo update
helm install nginx-ingress nginx-stable/nginx-ingress \
  --namespace cashflowapp
```

---

## **20.5. Caching with Redis and CDN**  

### **Step 1: Redis Caching for Microservices**  
We will use **Redis** as a caching layer to store frequently accessed data.

```bash
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install redis bitnami/redis --namespace cashflowapp
```

### **Spring Boot Configuration for Redis**:  
```yaml
spring.cache.type=redis
spring.redis.host=redis-master
spring.redis.port=6379
```

Example usage in **Course Service**:  
```java
@Cacheable(value = "courses", key = "#id")
public Course getCourseById(Long id) {
    return courseRepository.findById(id).orElse(null);
}
```

### **Step 2: CDN Integration**  
- Use **AWS CloudFront** as a CDN for static content stored in S3.  
- Configure the origin to point to the S3 bucket.  

---

## **20.6. Database Optimization**  

### **Step 1: Indexing and Query Optimization**  
- Add indexes on frequently queried columns in PostgreSQL.  
- Use **EXPLAIN ANALYZE** to optimize slow queries.

### **Step 2: Connection Pool Tuning**  
- Optimize **HikariCP** settings in Spring Boot as covered in **20.2**.  

### **Step 3: Read Replicas and Caching**  
- Enable **Read Replicas** in **AWS RDS** for horizontal scaling.  
- Implement **Query Caching** using Redis.  

---

## **20.7. CI/CD Performance Testing**  

### **Step 1: Load Testing with JMeter**  
- Integrate **JMeter** scripts in GitLab CI for load testing.  
- Trigger load tests after deployment in the CI/CD pipeline.  

### **Step 2: Integrate with Grafana**  
- Monitor load test metrics in **Grafana** using **Prometheus**.  
- Configure alerts for degraded performance.  

---

## **20.8. Verify and Test**  

1. **Check Autoscaling Activity**:  
```bash
kubectl get hpa -n cashflowapp
kubectl get vpa -n cashflowapp
```

2. **Test Load Balancing and Caching**:  
- Verify load balancing with **Nginx Ingress**.  
- Test caching effectiveness with **Redis**.  

3. **Performance Testing**:  
- Run load tests using **JMeter**.  
- Monitor results in **Grafana**.  

---

## **Next Step**  
1. Test all performance optimization and scaling features.  
2. Next, we’ll move on to **Phase 7: Implementing CI/CD Best Practices and DevOps Automation**.  
