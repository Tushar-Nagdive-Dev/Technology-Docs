# **Phase 12 - Step 26: Multi-Tenancy and SaaS Enablement**  

We have successfully implemented **Security Enhancements** and ensured **Compliance** with industry standards like **GDPR**, **CCPA**, and **PCI DSS**. Now, it's time to transform the platform into a **SaaS (Software as a Service)** product by implementing **Multi-Tenancy**. This phase involves:  
1. **Multi-Tenancy Architecture**: Implementing different multi-tenancy models (Database-per-tenant, Schema-per-tenant, and Table-per-tenant).  
2. **Tenant Isolation and Security**: Ensuring complete data isolation and security between tenants.  
3. **Tenant Management**: Dynamic tenant provisioning, onboarding, and lifecycle management.  
4. **SaaS Billing and Subscription**: Implementing subscription plans, billing cycles, and payment gateways.  
5. **Customizable User Experience**: Enabling tenant-specific branding and configuration.  
6. **API Gateway and Routing Enhancements**: Multi-tenant routing and domain management.  

---

## **26.1. Why Multi-Tenancy and SaaS Enablement?**  
- **Multi-Tenancy** allows multiple customers (tenants) to share the same application and infrastructure while maintaining data isolation.  
- **SaaS Enablement** transforms the platform into a scalable, subscription-based service.  
- **Tenant Isolation and Security** ensure data privacy and security across tenants.  
- **Customizable User Experience** provides tenant-specific branding, settings, and configurations.  
- **API Gateway and Routing Enhancements** manage tenant routing and custom domain mappings.  

---

## **26.2. Multi-Tenancy Architecture**  

We will implement three different multi-tenancy models:  
1. **Database-per-tenant**: Each tenant gets a separate database (Maximum Isolation).  
2. **Schema-per-tenant**: Shared database with a separate schema for each tenant.  
3. **Table-per-tenant**: Shared database and schema with a tenant identifier in each table.  

### **Recommended Strategy**: **Schema-per-tenant**  
- Balances isolation and scalability.  
- Easier to manage compared to Database-per-tenant.  
- Better performance and maintainability compared to Table-per-tenant.  

---

## **26.3. Implementing Schema-per-Tenant Multi-Tenancy in Spring Boot**  

We will implement **Schema-per-tenant** multi-tenancy for the **Course Service**. Each tenant will have its own schema, ensuring data isolation.  

### **Step 1: Dynamic DataSource Routing**  
We will use **AbstractRoutingDataSource** to switch schemas dynamically based on the tenant.  

### **1. TenantContext**  
A **ThreadLocal** context to store the current tenant information.  

**src/main/java/com/cashflowapp/courseservice/tenant/TenantContext.java**  
```java
package com.cashflowapp.courseservice.tenant;

public class TenantContext {
    private static final ThreadLocal<String> CURRENT_TENANT = new ThreadLocal<>();

    public static String getCurrentTenant() {
        return CURRENT_TENANT.get();
    }

    public static void setCurrentTenant(String tenantId) {
        CURRENT_TENANT.set(tenantId);
    }

    public static void clear() {
        CURRENT_TENANT.remove();
    }
}
```

---

### **2. TenantInterceptor**  
An **Interceptor** to extract the tenant ID from the request header.

**src/main/java/com/cashflowapp/courseservice/tenant/TenantInterceptor.java**  
```java
package com.cashflowapp.courseservice.tenant;

import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;
import org.springframework.stereotype.Component;
import org.springframework.web.servlet.HandlerInterceptor;

@Component
public class TenantInterceptor implements HandlerInterceptor {

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) {
        String tenantId = request.getHeader("X-Tenant-ID");
        if (tenantId == null) {
            response.setStatus(HttpServletResponse.SC_BAD_REQUEST);
            return false;
        }
        TenantContext.setCurrentTenant(tenantId);
        return true;
    }
}
```

---

### **3. Dynamic DataSource Configuration**  
**AbstractRoutingDataSource** implementation to switch schemas dynamically.

**src/main/java/com/cashflowapp/courseservice/config/TenantRoutingDataSource.java**  
```java
package com.cashflowapp.courseservice.config;

import com.cashflowapp.courseservice.tenant.TenantContext;
import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;

public class TenantRoutingDataSource extends AbstractRoutingDataSource {
    @Override
    protected Object determineCurrentLookupKey() {
        return TenantContext.getCurrentTenant();
    }
}
```

---

### **4. Multi-Tenant DataSource Configuration**  
**src/main/java/com/cashflowapp/courseservice/config/DataSourceConfig.java**  
```java
package com.cashflowapp.courseservice.config;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.boot.jdbc.DataSourceBuilder;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.jdbc.datasource.lookup.AbstractRoutingDataSource;

import javax.sql.DataSource;
import java.util.HashMap;
import java.util.Map;

@Configuration
public class DataSourceConfig {

    @Value("${spring.datasource.url}")
    private String dbUrl;

    @Value("${spring.datasource.username}")
    private String dbUsername;

    @Value("${spring.datasource.password}")
    private String dbPassword;

    @Bean
    public DataSource dataSource() {
        Map<Object, Object> dataSources = new HashMap<>();
        dataSources.put("tenant1", createDataSource("tenant1"));
        dataSources.put("tenant2", createDataSource("tenant2"));

        TenantRoutingDataSource routingDataSource = new TenantRoutingDataSource();
        routingDataSource.setDefaultTargetDataSource(createDataSource("public"));
        routingDataSource.setTargetDataSources(dataSources);
        routingDataSource.afterPropertiesSet();
        return routingDataSource;
    }

    private DataSource createDataSource(String schema) {
        return DataSourceBuilder.create()
                .url(dbUrl + "?currentSchema=" + schema)
                .username(dbUsername)
                .password(dbPassword)
                .build();
    }
}
```

### **Explanation**:  
- **TenantRoutingDataSource** switches the schema based on the `X-Tenant-ID` header.  
- **createDataSource()** configures the schema dynamically using `currentSchema` parameter.  
- **Map<Object, Object> dataSources** maps tenant IDs to schemas.  

---

### **5. Register TenantInterceptor**  
**src/main/java/com/cashflowapp/courseservice/config/WebConfig.java**  
```java
package com.cashflowapp.courseservice.config;

import com.cashflowapp.courseservice.tenant.TenantInterceptor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.InterceptorRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Autowired
    private TenantInterceptor tenantInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(tenantInterceptor);
    }
}
```

---

## **26.4. Dynamic Tenant Provisioning and Onboarding**  

### **1. Tenant Management Service**  
- **Create Tenant**: Provision new tenant schema dynamically.  
- **Tenant Onboarding**: Send onboarding email and set up default configurations.  

```java
public void createTenant(String tenantId) {
    jdbcTemplate.execute("CREATE SCHEMA " + tenantId);
    // Create default tables and configurations for the new tenant
}
```

### **2. Tenant Lifecycle Management**  
- **Activate Tenant**: Enable tenant access.  
- **Deactivate Tenant**: Disable access for inactive tenants.  
- **Delete Tenant**: Archive and delete tenant data securely.  

---

## **26.5. API Gateway and Routing Enhancements**  
- **Tenant-based Routing**: Route requests to the appropriate schema based on the `X-Tenant-ID` header.  
- **Custom Domain Mapping**: Allow tenants to map their custom domains (e.g., `tenant1.cashflowapp.com`).  

---

## **Next Step**  
1. Test all multi-tenancy features with schema isolation and dynamic tenant provisioning.  
2. Next, we’ll move on to **Phase 13: Microservices Resilience and Event-Driven Architecture**.  
