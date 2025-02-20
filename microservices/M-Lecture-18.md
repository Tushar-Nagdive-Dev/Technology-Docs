# **Phase 9 - Step 23: Advanced API Management and GraphQL Integration**  

We have successfully implemented **API Versioning**, **Documentation**, and **Backward Compatibility**. Now, it’s time to enhance the **API Management** and introduce **GraphQL** for advanced querying and flexibility. This phase involves:  
1. **API Management** using **AWS API Gateway** and **Kong** for rate limiting, authorization, and analytics.  
2. **GraphQL Integration**: Implementing GraphQL for flexible data querying and efficient communication.  
3. **Hybrid REST and GraphQL**: Coexistence of REST and GraphQL for backward compatibility.  
4. **API Gateway Enhancements**: Centralized GraphQL routing and schema stitching.  
5. **Performance Optimization**: Using **DataLoader** for batching and caching GraphQL requests.  

---

## **23.1. Why Advanced API Management and GraphQL?**  
- **API Management** ensures secure, scalable, and monitored APIs.  
- **GraphQL** provides flexible and efficient data querying, reducing over-fetching and under-fetching.  
- **Hybrid REST and GraphQL** maintain backward compatibility while leveraging GraphQL benefits.  
- **Performance Optimization** ensures high performance and fast response times for complex queries.  

---

## **23.2. API Management with AWS API Gateway and Kong**  

We will use **AWS API Gateway** and **Kong** for advanced API management, including:  
- **Rate Limiting**: Preventing abuse and overuse of APIs.  
- **Authentication and Authorization**: Using **OAuth2** and **JWT**.  
- **API Analytics**: Monitoring API usage and performance.  
- **GraphQL Routing and Schema Stitching**: Centralized routing for REST and GraphQL endpoints.  

---

### **Option 1: AWS API Gateway**  

### **Step 1: Create an API in AWS API Gateway**  
- Go to **AWS Management Console** → **API Gateway** → **Create API**.  
- Select **HTTP API** for faster performance and lower cost.  
- Name the API (e.g., `cashflowapp-api-gateway`).  

### **Step 2: Define Routes for Versioned APIs**  
- **GET /api/v1/courses** → Proxy to `Course Service v1`  
- **GET /api/v2/courses** → Proxy to `Course Service v2`  
- **POST /graphql** → Proxy to `GraphQL Gateway`

### **Step 3: Configure CORS**  
- Allow origins from the frontend application URL (e.g., `https://app.cashflowapp.com`).  
- Enable **HTTP methods**: `GET`, `POST`, `PUT`, `DELETE`.  

### **Step 4: Set Up Authentication with Cognito**  
- Use **AWS Cognito** for OAuth2 authentication and authorization.  
- Configure the **JWT Authorizer** in API Gateway with **Cognito User Pool**.  

### **Step 5: Deploy the API**  
- Deploy the API to **prod** stage.  
- Access the API using the following URL format:  
  ```text
  https://<api-id>.execute-api.us-east-1.amazonaws.com/prod
  ```

---

### **Option 2: Kong API Gateway**  

We can also use **Kong** as a self-hosted alternative to AWS API Gateway.

### **Step 1: Deploy Kong on Kubernetes**  
```bash
helm repo add kong https://charts.konghq.com
helm repo update
helm install kong kong/kong --namespace cashflowapp --create-namespace
```

### **Step 2: Configure Kong Routes**  
Create **Kong Routes** for versioned APIs and GraphQL.  

Example Route for **Course Service v1**:  
```yaml
apiVersion: configuration.konghq.com/v1
kind: KongIngress
metadata:
  name: course-service-v1
  namespace: cashflowapp
spec:
  rules:
    - host: api.cashflowapp.com
      paths:
        - path: /api/v1/courses
          backend:
            serviceName: course-service
            servicePort: 8082
```

### **Step 3: Apply Kong Ingress**  
```bash
kubectl apply -f kong-ingress.yml
```

### **Step 4: Enable Rate Limiting and OAuth2**  
```yaml
apiVersion: configuration.konghq.com/v1
kind: KongPlugin
metadata:
  name: rate-limiting
  namespace: cashflowapp
config:
  minute: 100
  hour: 1000
plugin: rate-limiting
```

```bash
kubectl apply -f kong-plugin-rate-limiting.yml
```

---

## **23.3. GraphQL Integration with Spring Boot**  

We will integrate **GraphQL** with the **Course Service** to enable flexible and efficient data querying.

### **Step 1: Add GraphQL Dependencies**  
**pom.xml**  
```xml
<dependency>
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphql-spring-boot-starter</artifactId>
    <version>11.1.0</version>
</dependency>
<dependency>
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphql-java-tools</artifactId>
    <version>11.1.0</version>
</dependency>
<dependency>
    <groupId>com.graphql-java-kickstart</groupId>
    <artifactId>graphiql-spring-boot-starter</artifactId>
    <version>11.1.0</version>
</dependency>
```

### **Step 2: Define GraphQL Schema**  
**src/main/resources/graphql/course.graphqls**  
```graphql
type Course {
  id: ID!
  title: String!
  description: String!
  instructor: String
  price: Float
}

type Query {
  getAllCourses: [Course]
  getCourseById(id: ID!): Course
}

type Mutation {
  createCourse(title: String!, description: String!, instructor: String, price: Float): Course
}
```

---

### **Step 3: Create GraphQL Resolvers**  
**src/main/java/com/cashflowapp/courseservice/graphql/CourseQueryResolver.java**  
```java
package com.cashflowapp.courseservice.graphql;

import com.cashflowapp.courseservice.model.Course;
import com.cashflowapp.courseservice.service.CourseService;
import com.coxautodev.graphql.tools.GraphQLQueryResolver;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.List;

@Component
public class CourseQueryResolver implements GraphQLQueryResolver {

    @Autowired
    private CourseService courseService;

    public List<Course> getAllCourses() {
        return courseService.getAllCourses();
    }

    public Course getCourseById(Long id) {
        return courseService.getCourseById(id).orElse(null);
    }
}
```

---

### **Step 4: Accessing GraphQL Playground**  
- **GraphiQL Playground**: `http://localhost:8082/graphiql`  
- Example Query:  
```graphql
query {
  getAllCourses {
    id
    title
    description
    instructor
    price
  }
}
```

---

### **Step 5: Integrate with API Gateway**  
- **POST /graphql** → Proxy to `Course Service GraphQL Endpoint`  
- Enable **schema stitching** for centralized GraphQL routing.  

---

## **23.4. Hybrid REST and GraphQL**  
- Coexistence of both REST and GraphQL for backward compatibility.  
- Use **GraphQL** for flexible queries and **REST** for traditional API usage.  

---

## **23.5. Performance Optimization with DataLoader**  
- Implement **DataLoader** to batch and cache GraphQL requests, reducing database queries.  

Example usage:  
```java
DataLoader<Long, Course> courseDataLoader = DataLoader.newMappedDataLoader(courseIds -> 
    CompletableFuture.supplyAsync(() -> courseService.getCoursesByIds(courseIds)));
```

---

## **23.6. Verify and Test**  

1. **Test GraphQL Endpoints**:  
   - Access `http://localhost:8082/graphql` for GraphQL queries.  
   - Confirm flexible and efficient data fetching.  

2. **Test Hybrid REST and GraphQL**:  
   - Confirm coexistence without breaking existing REST APIs.  

3. **Test API Management**:  
   - Verify rate limiting, OAuth2 authentication, and centralized routing.  

---

## **Next Step**  
1. Test all API management and GraphQL features.  
2. Next, we’ll move on to **Phase 10: Implementing AI-Powered Recommendations and Analytics**.  
