# **Phase 10 - Step 24: Implementing AI-Powered Recommendations and Analytics**  

We have successfully integrated **Advanced API Management** and **GraphQL** for flexible data querying and efficient communication. Now, it's time to leverage **Artificial Intelligence** and **Analytics** to enhance the platform's user experience and decision-making capabilities. This phase involves:  
1. **AI-Powered Recommendations**: Implementing personalized course recommendations using **Machine Learning**.  
2. **Analytics and Insights**: Real-time analytics using **AWS Kinesis**, **Redshift**, and **QuickSight**.  
3. **User Behavior Tracking**: Tracking user interactions using **AWS Pinpoint** and **Google Analytics**.  
4. **AI/ML Model Deployment**: Using **AWS SageMaker** for training, deploying, and scaling ML models.  
5. **Recommendation Algorithms**: Collaborative Filtering, Content-Based Filtering, and Hybrid Recommendations.  

---

## **24.1. Why AI-Powered Recommendations and Analytics?**  
- **AI-Powered Recommendations** enhance user engagement and satisfaction by providing personalized content.  
- **Analytics and Insights** enable data-driven decision-making and business intelligence.  
- **User Behavior Tracking** provides valuable insights into user interactions and preferences.  
- **AI/ML Model Deployment** ensures scalable and reliable machine learning infrastructure.  

---

## **24.2. Recommendation System Overview**  
We will implement a **Personalized Course Recommendation System** using the following algorithms:  
- **Collaborative Filtering**: Recommends courses based on user-user and item-item similarities.  
- **Content-Based Filtering**: Recommends courses based on course content and user preferences.  
- **Hybrid Recommendations**: Combines both collaborative and content-based filtering for improved accuracy.  

---

## **24.3. Setting Up AWS SageMaker for Machine Learning**  

We will use **AWS SageMaker** to train, deploy, and scale ML models.

### **Step 1: Create a SageMaker Notebook Instance**  
- Go to **AWS Management Console** → **SageMaker** → **Notebook Instances** → **Create Notebook Instance**.  
- Name the instance (e.g., `cashflowapp-ml-notebook`).  
- Choose an instance type (`ml.t2.medium` for development).  
- Attach an **IAM Role** with the following permissions:  
  - **S3 Read/Write Access** for training data and models.  
  - **SageMaker Full Access** for training and deploying models.  

### **Step 2: Prepare Training Data**  
We will use historical enrollment and user behavior data from the **Enrollment Service**.  

### **Step 3: Upload Data to S3**  
- Create an S3 bucket (e.g., `cashflowapp-ml-data`).  
- Upload the training data as CSV files:  
  - **enrollments.csv**: Contains user-course interactions.  
  - **courses.csv**: Contains course metadata (title, description, category).  

---

## **24.4. Implementing Collaborative Filtering**  

We will implement **Collaborative Filtering** using **Amazon SageMaker Built-in Algorithm (Factorization Machines)**.

### **Step 1: Data Preprocessing with Pandas**  
Open the **Jupyter Notebook** in SageMaker.

```python
import boto3
import pandas as pd

# Load Data from S3
s3 = boto3.client('s3')
bucket = 'cashflowapp-ml-data'
enrollments_file = 'enrollments.csv'
courses_file = 'courses.csv'

enrollments_df = pd.read_csv(f's3://{bucket}/{enrollments_file}')
courses_df = pd.read_csv(f's3://{bucket}/{courses_file}')

# Data Preprocessing
enrollments_df['interaction'] = 1  # 1 for enrollment
user_item_matrix = enrollments_df.pivot(index='user_id', columns='course_id', values='interaction').fillna(0)
print(user_item_matrix.head())
```

### **Step 2: Train Collaborative Filtering Model**  
We will use **Amazon SageMaker Factorization Machines** for training the collaborative filtering model.

```python
from sagemaker import Session
from sagemaker.amazon.amazon_estimator import get_image_uri

sagemaker_session = Session()
role = 'arn:aws:iam::<account-id>:role/<role-name>'

# Get the container image for Factorization Machines
fm_container = get_image_uri(boto3.Session().region_name, 'factorization-machines')

fm = sagemaker.estimator.Estimator(
    fm_container,
    role,
    train_instance_count=1,
    train_instance_type='ml.c4.xlarge',
    output_path=f's3://{bucket}/output',
    sagemaker_session=sagemaker_session
)

# Set Hyperparameters
fm.set_hyperparameters(
    feature_dim=100,
    num_factors=10,
    predictor_type='binary_classifier',
    mini_batch_size=100
)

# Train the Model
fm.fit({'train': f's3://{bucket}/train_data'})
```

### **Step 3: Deploy the Model**  
```python
# Deploy the model as a SageMaker endpoint
fm_predictor = fm.deploy(
    initial_instance_count=1,
    instance_type='ml.m4.xlarge'
)
```

### **Step 4: Make Predictions**  
```python
import json
import numpy as np

# Predict Top 5 Course Recommendations for a User
user_id = 123
user_vector = user_item_matrix.loc[user_id].values
payload = json.dumps(user_vector.tolist())

predicted_scores = fm_predictor.predict(payload)
top_courses = np.argsort(predicted_scores)[-5:][::-1]
print(top_courses)
```

---

## **24.5. Implementing Content-Based Filtering**  

### **Step 1: Text Embedding with BERT**  
We will use **BERT** to embed course descriptions into vector space.

```python
from transformers import BertTokenizer, BertModel
import torch

tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertModel.from_pretrained('bert-base-uncased')

# Encode Course Descriptions
def get_course_embedding(text):
    inputs = tokenizer(text, return_tensors='pt')
    outputs = model(**inputs)
    return outputs.last_hidden_state.mean(dim=1).detach().numpy()

courses_df['embedding'] = courses_df['description'].apply(get_course_embedding)
```

### **Step 2: Content Similarity**  
Calculate similarity using **Cosine Similarity**.

```python
from sklearn.metrics.pairwise import cosine_similarity

def recommend_similar_courses(course_id):
    target_vector = courses_df.loc[courses_df['course_id'] == course_id]['embedding'].values[0]
    similarities = cosine_similarity([target_vector], courses_df['embedding'].tolist())[0]
    recommended_indices = similarities.argsort()[-5:][::-1]
    return courses_df.iloc[recommended_indices]['title'].tolist()

recommended_courses = recommend_similar_courses(course_id=101)
print(recommended_courses)
```

---

## **24.6. Hybrid Recommendation System**  
Combine **Collaborative Filtering** and **Content-Based Filtering** for improved accuracy.

```python
def hybrid_recommendations(user_id, course_id):
    collaborative_scores = fm_predictor.predict(user_item_matrix.loc[user_id].values)
    content_scores = recommend_similar_courses(course_id)
    combined_scores = (np.array(collaborative_scores) + np.array(content_scores)) / 2
    top_courses = np.argsort(combined_scores)[-5:][::-1]
    return top_courses

hybrid_courses = hybrid_recommendations(user_id=123, course_id=101)
print(hybrid_courses)
```

---

## **24.7. Integrate Recommendations with Course Service**  

Create a new GraphQL query for **Course Recommendations**:  

**src/main/resources/graphql/course.graphqls**  
```graphql
type Query {
  getCourseRecommendations(userId: ID!): [Course]
}
```

**src/main/java/com/cashflowapp/courseservice/graphql/CourseQueryResolver.java**  
```java
public List<Course> getCourseRecommendations(Long userId) {
    return recommendationService.getRecommendations(userId);
}
```

---

## **24.8. Verify and Test**  

1. **Test ML Model**:  
   - Confirm accurate predictions and recommendations.  

2. **Test GraphQL Query**:  
   - `POST /graphql` → `getCourseRecommendations(userId: 123)`.  

3. **Test Hybrid Recommendations**:  
   - Validate improved accuracy and relevance.  

---

## **Next Step**  
1. Test all AI-powered recommendations and analytics features.  
2. Next, we’ll move on to **Phase 11: Security Enhancements and Compliance**.  

Ready to test it, or need help with any setup? 🚀 Let's keep building!
