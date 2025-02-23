### **Lesson 29: Advanced Angular and NestJS Integration**

---

## **What You Will Learn:**
1. **GraphQL Integration with Angular and NestJS**
   - Why Use GraphQL with Angular and NestJS?
   - Setting up Apollo Client in Angular
   - Setting up GraphQL Module in NestJS
   - Defining Schemas, Queries, Mutations, and Subscriptions

2. **Authentication and Authorization with JWT and Passport**
   - Implementing JWT Authentication with Passport in NestJS
   - Securing RESTful and GraphQL Endpoints
   - Role-Based Authorization and Guards in NestJS
   - Protecting Angular Routes with AuthGuard

3. **Role-Based Access Control and Guards**
   - Implementing Role-Based Access Control (RBAC)
   - Creating Custom Guards in NestJS
   - Managing Permissions and Roles in Angular
   - Dynamic Route Guards and Navigation Guards

4. **Microservices Architecture with NestJS and Angular**
   - Introduction to Microservices with NestJS
   - Building Microservices with gRPC, Redis, and RabbitMQ
   - Communication Between Microservices with NestJS Client Proxies
   - Integrating Microservices with Angular Frontend

5. **Advanced Error Handling and Logging**
   - Centralized Error Handling in NestJS
   - Using Global Filters, Interceptors, and Pipes
   - Advanced Logging with Winston and Morgan
   - Error Notification and Monitoring with Sentry

6. **Testing and Debugging Angular + NestJS Applications**
   - Unit Testing and Integration Testing with Jest in NestJS
   - End-to-End Testing with Cypress for Angular + NestJS
   - Debugging Techniques and Tools for Full-Stack Development
   - CI/CD Pipelines for Full-Stack Applications

7. **Performance Optimization and Best Practices**
   - Caching with Redis and In-Memory Cache
   - Lazy Loading and Code Splitting in Angular
   - API Gateway and Load Balancing with NestJS
   - Security Best Practices and Data Validation

8. **Hands-on Exercises:**
   - Building a Real-Time Chat Application with GraphQL Subscriptions
   - Real-World Scenario: Role-Based Admin Dashboard with JWT and RBAC

9. **Expert Insights and Best Practices**
10. **Common Mistakes to Avoid**
11. **Recap and Next Steps**

---

## **1. GraphQL Integration with Angular and NestJS**

### **1.1 Why Use GraphQL with Angular and NestJS?**
- **GraphQL** provides **declarative data fetching** and reduces over-fetching and under-fetching.
- **Angular** uses **Apollo Client** to consume GraphQL APIs.
- **NestJS** acts as a **GraphQL server** using **Apollo Server**.
- **Benefits**:
  - **Single Endpoint** for all queries and mutations.
  - **Flexible Data Fetching**: Clients can specify the shape of the response.
  - **Real-Time Updates**: Using **GraphQL Subscriptions** over WebSocket.

---

### **1.2 Setting up Apollo Client in Angular**

1. **Install Apollo Client**

```bash
npm install @apollo/client graphql apollo-angular
```

2. **Configure Apollo Client**

**app.module.ts**:

```typescript
import { NgModule } from '@angular/core';
import { ApolloModule, APOLLO_OPTIONS } from 'apollo-angular';
import { InMemoryCache, ApolloClientOptions } from '@apollo/client/core';
import { HttpLink } from 'apollo-angular/http';

@NgModule({
  imports: [ApolloModule],
  providers: [
    {
      provide: APOLLO_OPTIONS,
      useFactory: (httpLink: HttpLink): ApolloClientOptions<any> => ({
        cache: new InMemoryCache(),
        link: httpLink.create({ uri: 'http://localhost:3000/graphql' })
      }),
      deps: [HttpLink]
    }
  ]
})
export class AppModule {}
```

- **InMemoryCache**: Caches GraphQL responses for efficient state management.
- **HttpLink**: Connects Apollo Client to the GraphQL endpoint.

---

### **1.3 Setting up GraphQL Module in NestJS**

1. **Install GraphQL Dependencies**

```bash
npm install @nestjs/graphql @nestjs/apollo graphql apollo-server-express
```

2. **Configure GraphQL Module**

**app.module.ts**:

```typescript
import { Module } from '@nestjs/common';
import { GraphQLModule } from '@nestjs/graphql';
import { ApolloDriver, ApolloDriverConfig } from '@nestjs/apollo';
import { join } from 'path';

@Module({
  imports: [
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
      playground: true,
      debug: true
    })
  ]
})
export class AppModule {}
```

- **autoSchemaFile**: Automatically generates the schema.
- **playground**: Enables **GraphQL Playground** for testing queries.
- **ApolloDriver**: Uses Apollo Server as the GraphQL engine.

---

### **1.4 Defining Schemas, Queries, Mutations, and Subscriptions**

1. **Define GraphQL Schema**

**product.model.ts**:

```typescript
import { Field, ObjectType, Int, ID } from '@nestjs/graphql';

@ObjectType()
export class Product {
  @Field(() => ID)
  id: string;

  @Field()
  name: string;

  @Field(() => Int)
  price: number;
}
```

2. **Create GraphQL Resolvers**

**product.resolver.ts**:

```typescript
import { Resolver, Query, Mutation, Args } from '@nestjs/graphql';
import { Product } from './product.model';
import { ProductService } from './product.service';

@Resolver(() => Product)
export class ProductResolver {
  constructor(private productService: ProductService) {}

  @Query(() => [Product])
  getAllProducts() {
    return this.productService.getAllProducts();
  }

  @Mutation(() => Product)
  addProduct(
    @Args('name') name: string,
    @Args('price') price: number
  ) {
    return this.productService.addProduct(name, price);
  }
}
```

3. **GraphQL Subscription (Real-Time Updates)**

**product.resolver.ts**:

```typescript
import { Subscription } from '@nestjs/graphql';
import { PubSub } from 'graphql-subscriptions';

const pubSub = new PubSub();

@Subscription(() => Product)
productAdded() {
  return pubSub.asyncIterator('productAdded');
}
```

- **PubSub** is used for **real-time subscriptions**.
- **asyncIterator** listens for events and triggers the subscription.

---

## **2. Authentication and Authorization with JWT and Passport**

### **2.1 Implementing JWT Authentication with Passport**

1. **Install Dependencies**

```bash
npm install @nestjs/passport @nestjs/jwt passport-jwt bcryptjs
```

2. **Configure JWT Module**

**auth.module.ts**:

```typescript
import { JwtModule } from '@nestjs/jwt';

JwtModule.register({
  secret: 'SECRET_KEY',
  signOptions: { expiresIn: '1h' }
});
```

3. **JWT Strategy**

**jwt.strategy.ts**:

```typescript
import { PassportStrategy } from '@nestjs/passport';
import { ExtractJwt, Strategy } from 'passport-jwt';

export class JwtStrategy extends PassportStrategy(Strategy) {
  constructor() {
    super({
      jwtFromRequest: ExtractJwt.fromAuthHeaderAsBearerToken(),
      secretOrKey: 'SECRET_KEY'
    });
  }

  async validate(payload: any) {
    return { userId: payload.sub, username: payload.username };
  }
}
```

- **validate** method validates the JWT payload and returns user info.

---

### **2.2 Securing GraphQL Endpoints with Guards**

**auth.guard.ts**:

```typescript
import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
import { GqlExecutionContext } from '@nestjs/graphql';

@Injectable()
export class GqlAuthGuard implements CanActivate {
  canActivate(context: ExecutionContext): boolean {
    const ctx = GqlExecutionContext.create(context);
    const request = ctx.getContext().req;
    return request.isAuthenticated();
  }
}
```

**product.resolver.ts**:

```typescript
@UseGuards(GqlAuthGuard)
@Mutation(() => Product)
addProduct(@Args('name') name: string, @Args('price') price: number) {
  return this.productService.addProduct(name, price);
}
```

- **GqlAuthGuard** secures GraphQL endpoints.
- **@UseGuards** decorator applies the guard to mutations or queries.

---

## **Next Lesson: Angular Universal and SSR with NestJS**
- **Server-Side Rendering with Angular Universal**
- **SEO Optimization with Angular and NestJS**
- **Dynamic Meta Tags and Open Graph Protocol**
- **Deployment and Hosting of SSR Applications**
