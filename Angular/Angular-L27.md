### **Lesson 27: Angular + GraphQL Integration**

---

## **What You Will Learn:**
1. **Introduction to GraphQL and Apollo Client**
   - What is GraphQL?
   - Why Use GraphQL with Angular?
   - Core Concepts: Query, Mutation, and Subscription
   - Overview of Apollo Client for Angular

2. **Setting up GraphQL in Angular**
   - Installing Apollo Client and GraphQL Modules
   - Configuring Apollo Client with Angular
   - Connecting to a GraphQL Endpoint

3. **Querying Data with GraphQL**
   - Writing GraphQL Queries in Angular
   - Using Apollo Query Component and Query Hook
   - Handling Query Results and Errors
   - Pagination and Filtering with GraphQL Queries

4. **Mutations: Creating and Updating Data**
   - Writing GraphQL Mutations
   - Executing Mutations with Apollo
   - Optimistic UI Updates and Cache Management
   - Error Handling and Retry Mechanisms

5. **Subscriptions: Real-Time Updates**
   - Introduction to GraphQL Subscriptions
   - Setting up WebSocket with Apollo
   - Implementing Real-Time Updates in Angular
   - Managing Subscription Lifecycles

6. **GraphQL Caching and State Management**
   - Apollo Client In-Memory Cache
   - Normalized Caching for Efficient Data Updates
   - Using Apollo Local State for Global State Management
   - Integrating GraphQL with NgRx Store

7. **Performance Optimization and Best Practices**
   - Batch Querying and Caching Strategies
   - Using Fragments for Reusable GraphQL Queries
   - Pagination Techniques with Relay and Cursor Pagination
   - Securing GraphQL Endpoints

8. **Hands-on Exercises:**
   - Building a Product Catalog with GraphQL and Angular
   - Real-World Scenario: Live Notifications with GraphQL Subscriptions

9. **Expert Insights and Best Practices**
10. **Common Mistakes to Avoid**
11. **Recap and Next Steps**

---

## **1. Introduction to GraphQL and Apollo Client**

### **1.1 What is GraphQL?**
- **GraphQL** is a **query language** for APIs that enables clients to request exactly the data they need.
- Developed by **Facebook**, it provides:
  - **Single Endpoint**: All data is accessed via a single endpoint.
  - **Declarative Data Fetching**: Clients specify data requirements declaratively.
  - **No Over-fetching or Under-fetching**: Clients receive exactly what they request.
  - **Strongly Typed Schema**: Data is defined with a schema that enforces structure.

---

### **1.2 Why Use GraphQL with Angular?**
- **Efficient Data Fetching**:
  - Request only the required fields, reducing payload size.
- **Real-Time Updates**:
  - Enable real-time communication with **GraphQL Subscriptions**.
- **Flexible Queries**:
  - Fetch complex data structures with a single request.
- **Simplified Client-Side State Management**:
  - Apollo Client manages state with **Normalized Caching**.
- **Enhanced Developer Experience**:
  - Auto-generated TypeScript types with GraphQL Code Generator.

---

### **1.3 Core Concepts of GraphQL**

1. **Query**:
   - Fetches data from the server.
   - Example:
     ```graphql
     query {
       products {
         id
         name
         price
       }
     }
     ```

2. **Mutation**:
   - Modifies data on the server (create, update, delete).
   - Example:
     ```graphql
     mutation {
       addProduct(name: "Laptop", price: 1000) {
         id
         name
         price
       }
     }
     ```

3. **Subscription**:
   - Real-time updates via WebSocket connection.
   - Example:
     ```graphql
     subscription {
       productAdded {
         id
         name
         price
       }
     }
     ```

---

### **1.4 Overview of Apollo Client for Angular**
- **Apollo Client** is a **GraphQL client** that manages data fetching, caching, and state management.
- Key Features:
  - **Query, Mutation, and Subscription** management.
  - **Normalized Caching** for efficient data updates.
  - **Apollo Link** for request lifecycle control.
  - **In-Memory Cache** for state management.
  - **TypeScript Support** and **Code Generation**.

---

## **2. Setting up GraphQL in Angular**

### **2.1 Installing Apollo Client and GraphQL Modules**

```bash
npm install @apollo/client graphql apollo-angular apollo-angular-link-http apollo-angular-link-ws subscriptions-transport-ws
```

- `@apollo/client`: Apollo Client core package.
- `graphql`: GraphQL query language support.
- `apollo-angular`: Angular integration for Apollo Client.
- `apollo-angular-link-http`: HTTP link for GraphQL queries and mutations.
- `apollo-angular-link-ws`: WebSocket link for GraphQL subscriptions.
- `subscriptions-transport-ws`: WebSocket transport layer for GraphQL subscriptions.

---

### **2.2 Configuring Apollo Client with Angular**

**app.module.ts**:

```typescript
import { NgModule } from '@angular/core';
import { HttpClientModule } from '@angular/common/http';
import { ApolloModule, APOLLO_OPTIONS } from 'apollo-angular';
import { ApolloClientOptions, InMemoryCache, ApolloLink, split } from '@apollo/client/core';
import { HttpLink } from 'apollo-angular-link-http';
import { WebSocketLink } from 'apollo-angular-link-ws';
import { getMainDefinition } from '@apollo/client/utilities';

@NgModule({
  imports: [HttpClientModule, ApolloModule],
  providers: [
    {
      provide: APOLLO_OPTIONS,
      useFactory: (httpLink: HttpLink): ApolloClientOptions<any> => {
        const http = httpLink.create({ uri: 'https://api.example.com/graphql' });
        const ws = new WebSocketLink({
          uri: 'wss://api.example.com/graphql',
          options: { reconnect: true }
        });

        const link = split(
          ({ query }) => {
            const definition = getMainDefinition(query);
            return (
              definition.kind === 'OperationDefinition' &&
              definition.operation === 'subscription'
            );
          },
          ws,
          http
        );

        return {
          link,
          cache: new InMemoryCache()
        };
      },
      deps: [HttpLink]
    }
  ]
})
export class AppModule {}
```

- **HttpLink**: Handles **queries and mutations** over HTTP.
- **WebSocketLink**: Handles **subscriptions** over WebSocket.
- **split**: Directs requests between **HTTP** and **WebSocket** based on the operation type.
- **InMemoryCache**: Caches GraphQL responses for efficient state management.

---

### **2.3 Connecting to a GraphQL Endpoint**

- The `uri` is the **GraphQL endpoint**:
  - **HTTP Endpoint**: `https://api.example.com/graphql`
  - **WebSocket Endpoint**: `wss://api.example.com/graphql`

- **split** function:
  - Routes **subscriptions** to `WebSocketLink`.
  - Routes **queries and mutations** to `HttpLink`.

---

## **3. Querying Data with GraphQL**

### **3.1 Writing GraphQL Queries in Angular**

**product.query.ts**:

```typescript
import { gql } from 'apollo-angular';

export const GET_PRODUCTS = gql`
  query {
    products {
      id
      name
      price
    }
  }
`;
```

- **gql** is a tagged template literal for writing GraphQL queries.
- **GET_PRODUCTS** is a query to **fetch products**.

---

### **3.2 Using Apollo Query Component**

**product.component.ts**:

```typescript
import { Component, OnInit } from '@angular/core';
import { Apollo } from 'apollo-angular';
import { GET_PRODUCTS } from './product.query';

@Component({
  selector: 'app-product',
  templateUrl: './product.component.html'
})
export class ProductComponent implements OnInit {
  products: any[] = [];

  constructor(private apollo: Apollo) {}

  ngOnInit(): void {
    this.apollo
      .watchQuery({ query: GET_PRODUCTS })
      .valueChanges.subscribe(result => {
        this.products = result.data.products;
      });
  }
}
```

- **watchQuery** subscribes to the query and listens for changes.
- **valueChanges** is an Observable for the query result.

---

## **Next Lesson: Angular + NestJS Full-Stack Development**
- **Integrating Angular Frontend with NestJS Backend**
- **GraphQL API Development with NestJS**
- **Authentication and Authorization with JWT**
- **Microservices Architecture with Angular and NestJS**
