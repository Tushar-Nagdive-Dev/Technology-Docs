### **Lesson 21: Angular Deployment and CI/CD**

---

## **What You Will Learn:**
1. **Introduction to Angular Deployment and CI/CD**
   - Why Deploy Angular Applications?
   - Understanding CI/CD Pipelines
   - Popular Hosting Platforms for Angular:
     - Firebase Hosting
     - Netlify
     - Vercel
     - GitHub Pages

2. **Deploying Angular on Firebase Hosting**
   - Setting up Firebase Hosting for Angular
   - Deploying Angular with Firebase CLI
   - Custom Domain Setup and HTTPS Configuration

3. **Deploying Angular on Netlify and Vercel**
   - Netlify Setup and Deployment
   - Vercel Setup and Deployment
   - Automatic Deployments with GitHub Integration

4. **CI/CD Pipelines for Angular**
   - Introduction to CI/CD for Angular Applications
   - Setting up CI/CD Pipelines with:
     - GitHub Actions
     - GitLab CI
   - Automating Builds and Deployments

5. **Dockerizing Angular Applications**
   - Why Dockerize Angular Applications?
   - Creating a Dockerfile for Angular
   - Building and Running Angular Docker Containers
   - Deploying Dockerized Angular on AWS, Azure, and GCP

6. **Performance Optimization and Caching**
   - Using Service Workers for Offline Caching
   - Gzip Compression and HTTP/2 for Faster Loading
   - Angular Lazy Loading and Code Splitting

7. **Hands-on Exercises:**
   - Deploying Angular on Firebase and Netlify
   - CI/CD Pipeline Setup with GitHub Actions

8. **Expert Insights and Best Practices**
9. **Common Mistakes to Avoid**
10. **Recap and Next Steps**

---

## **1. Introduction to Angular Deployment and CI/CD**

### **1.1 Why Deploy Angular Applications?**
- **Make Applications Publicly Accessible**:
  - Deploying an Angular application allows users to access it via the internet.
- **Continuous Delivery and Integration**:
  - CI/CD pipelines automate testing, building, and deployment processes.
- **Scalable Hosting Solutions**:
  - Cloud hosting platforms provide scalability, high availability, and global distribution.
- **Version Control and Rollbacks**:
  - CI/CD pipelines enable version control, rollback, and deployment history.

---

### **1.2 Understanding CI/CD Pipelines**

1. **Continuous Integration (CI)**:
   - Automates the process of building and testing code.
   - Ensures that new changes integrate smoothly into the main branch.
   - Tools: **GitHub Actions**, **GitLab CI**, **CircleCI**, **Travis CI**.

2. **Continuous Delivery (CD)**:
   - Automates the deployment process to staging or production environments.
   - Ensures that the application is always ready for deployment.
   - Deployments can be triggered manually or automatically after CI passes.

3. **Continuous Deployment (CD)**:
   - Fully automates the deployment process.
   - Every successful build is automatically deployed to production.

---

### **1.3 Popular Hosting Platforms for Angular**

1. **Firebase Hosting**:
   - Fast and secure static hosting with built-in CDN.
   - Supports **custom domains**, **HTTPS**, and **server-side rendering** with Firebase Functions.

2. **Netlify**:
   - Continuous deployment from GitHub, GitLab, and Bitbucket.
   - **Custom domain support**, **forms**, and **serverless functions**.

3. **Vercel**:
   - Serverless deployment and **automatic CDN caching**.
   - Optimized for **Jamstack** and **static site generation**.

4. **GitHub Pages**:
   - Free static hosting for open-source projects.
   - Ideal for personal projects and documentation sites.

---

## **2. Deploying Angular on Firebase Hosting**

### **2.1 Setting up Firebase Hosting for Angular**

1. **Install Firebase CLI**

```bash
npm install -g firebase-tools
```

2. **Login to Firebase**

```bash
firebase login
```

3. **Initialize Firebase Hosting**

```bash
firebase init
```

- Select **Hosting**.
- Select **Existing project** or create a new one.
- Specify `dist/your-app-name` as the **public directory**.
- Choose **Single Page Application** and select **Yes** for 404 rewrite.

---

### **2.2 Deploying Angular with Firebase CLI**

1. **Build the Angular Application**

```bash
ng build --prod
```

2. **Deploy to Firebase Hosting**

```bash
firebase deploy
```

- The application is now live on **Firebase Hosting**.
- URL: `https://<your-project-id>.web.app`

---

### **2.3 Custom Domain Setup and HTTPS Configuration**

1. **Add Custom Domain in Firebase Console**
   - Go to **Firebase Console** → **Hosting** → **Add Custom Domain**.
   - Add your domain (e.g., `www.example.com`).
   - Verify ownership using DNS TXT record.

2. **Configure DNS Records**
   - Add **A Records** pointing to Firebase's IP addresses.
   - **Firebase** provides automatic **SSL Certificates** for HTTPS.

---

## **3. Deploying Angular on Netlify and Vercel**

### **3.1 Netlify Setup and Deployment**

1. **Login to Netlify**
   - Go to [Netlify](https://www.netlify.com) and log in with GitHub.

2. **Connect GitHub Repository**
   - Click **New Site from Git**.
   - Select **GitHub** and choose your repository.

3. **Configure Build Settings**
   - **Build Command**: `ng build --prod`
   - **Publish Directory**: `dist/your-app-name`

4. **Deploy and Access**
   - Netlify automatically builds and deploys the site.
   - URL: `https://<project-name>.netlify.app`

---

### **3.2 Vercel Setup and Deployment**

1. **Login to Vercel**
   - Go to [Vercel](https://vercel.com) and log in with GitHub.

2. **Connect GitHub Repository**
   - Click **New Project** and select your GitHub repository.

3. **Configure Build Settings**
   - **Framework Preset**: Angular
   - **Build Command**: `ng build --prod`
   - **Output Directory**: `dist/your-app-name`

4. **Deploy and Access**
   - Vercel automatically builds and deploys the site.
   - URL: `https://<project-name>.vercel.app`

---

## **4. CI/CD Pipelines for Angular**

### **4.1 Setting up CI/CD with GitHub Actions**

1. **Create GitHub Action Workflow**:

In `.github/workflows/deploy.yml`:

```yaml
name: Deploy Angular App

on:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v2

      - name: Set up Node.js
        uses: actions/setup-node@v2
        with:
          node-version: '16'

      - name: Install Dependencies
        run: npm install

      - name: Build Angular App
        run: npm run build --prod

      - name: Deploy to Firebase
        uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: ${{ secrets.GITHUB_TOKEN }}
          firebaseServiceAccount: ${{ secrets.FIREBASE_SERVICE_ACCOUNT }}
          projectId: your-firebase-project-id
```

- Automatically builds and deploys the Angular app to **Firebase Hosting** on every push to the **main** branch.

---

### **4.2 Setting up CI/CD with GitLab CI**

In `.gitlab-ci.yml`:

```yaml
image: node:16

stages:
  - build
  - deploy

build:
  stage: build
  script:
    - npm install
    - npm run build --prod
  artifacts:
    paths:
      - dist/

deploy:
  stage: deploy
  script:
    - npm install -g firebase-tools
    - firebase deploy --token $FIREBASE_DEPLOY_TOKEN
  only:
    - main
```

- **Builds** and **deploys** the Angular app to **Firebase** on the **main** branch.

---

## **5. Dockerizing Angular Applications**

### **5.1 Creating a Dockerfile for Angular**

```dockerfile
# Stage 1: Build
FROM node:16 AS builder
WORKDIR /app
COPY package*.json ./
RUN npm install
COPY . .
RUN npm run build --prod

# Stage 2: Serve
FROM nginx:alpine
COPY --from=builder /app/dist/your-app-name /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### **5.2 Building and Running Docker Container**

```bash
docker build -t angular-app .
docker run -p 8080:80 angular-app
```

- Access the Angular app at `http://localhost:8080`.

---

## **Next Lesson: Angular Advanced Routing and Navigation**
- **Nested Routes and Lazy Loading**
- **Dynamic Route Parameters and Query Params**
- **Route Guards and Role-Based Authorization**
- **Preloading Strategies and Optimizing Navigation**
