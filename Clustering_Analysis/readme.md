# Customer Churn Analysis using K-Means Clustering

## Project Overview

This project focuses on customer segmentation for a telecommunications company using the K-Means clustering algorithm. The objective is to identify groups of customers with similar characteristics and provide business insights that can help improve customer retention strategies and reduce customer churn.

This project was completed as part of a team-based Customer Churn Analysis project. My responsibility was the **Clustering Analysis** component.

---

## Project Objectives

- Analyze the preprocessed customer dataset.
- Determine the optimal number of customer clusters.
- Build and train a K-Means clustering model.
- Visualize customer segments using Principal Component Analysis (PCA).
- Interpret each customer cluster.
- Generate business insights to support customer retention strategies.

---

## Technologies Used

- Python 3.x
- Jupyter Notebook
- Pandas
- NumPy
- Matplotlib
- Seaborn
- Scikit-learn
- Joblib

---

## Project Structure

```
Customer_Churn_Project/

│
├── processed_scaled.csv
│
├── Clustering_Analysis/
│   ├── WCSS_Analysis.ipynb
│   ├── Train_Final_KMeans.ipynb
│   ├── cluster_visualization.ipynb
│   ├── trained_kmeans_model.pkl
│   └── Cluster_Visualizations/
│
├── README.md
│

```

---

## Dataset

The project uses a preprocessed telecommunications customer dataset.

The dataset contains customer demographic information, account details, service usage, and other features relevant to customer behaviour.

The **Churn** column was excluded during clustering because K-Means is an unsupervised learning algorithm. It was later used to evaluate customer segments and understand churn patterns.

---

## Installation

Clone the repository

```bash
git clone https://github.com/yourusername/customer-churn-analysis.git
```

Move into the project folder

```bash
cd customer-churn-analysis
```

Install the required libraries

```bash
pip install pandas
pip install numpy
pip install matplotlib
pip install seaborn
pip install scikit-learn
pip install joblib
```

---

## Workflow

### 1. Load Dataset

- Import the processed dataset.
- Review the dataset structure.

### 2. Data Validation

- Check dataset dimensions.
- Verify duplicate records.
- Remove duplicate observations if required.

### 3. Feature Selection

- Remove the **Churn** column.
- Prepare the feature matrix for clustering.

### 4. Feature Scaling

- Apply StandardScaler.
- Standardize numerical features before clustering.

### 5. Determine Optimal Clusters

- Apply the Elbow Method.
- Calculate Within Cluster Sum of Squares (WCSS).
- Select the optimal value of K.

### 6. Train K-Means Model

- Train the clustering model using the selected number of clusters.
- Assign cluster labels to each customer.

### 7. Save Model

- Save the trained model using Joblib.

### 8. Cluster Visualization

- Reduce dimensionality using PCA.
- Visualize customer clusters.
- Label customer segments.

### 9. Cluster Interpretation

- Compare customer behaviour across clusters.
- Analyze churn distribution within each cluster.
- Generate business recommendations.

---

## Key Findings

- Four customer segments were identified using the Elbow Method.
- Customers with similar characteristics were grouped into meaningful clusters.
- PCA visualization confirmed the separation between customer groups.
- Some customer segments showed significantly higher churn than others.
- Customer segmentation provides valuable insights for targeted marketing and customer retention.

---

## Deliverables

The Clustering Analysis folder contains:

- WCSS analysis and Elbow Method visualization
- Trained K-Means model
- K-Means training notebook
- Cluster visualization notebook
- PCA visualization
- Cluster interpretation and business insights

---

## Business Value

Customer segmentation helps the business to:

- Identify customers at high risk of churn.
- Develop personalized marketing campaigns.
- Improve customer retention strategies.
- Allocate business resources more effectively.
- Support data-driven decision-making.

---

## Future Improvements

Possible improvements include:

- Comparing K-Means with Hierarchical Clustering or DBSCAN.
- Hyperparameter optimization.
- Using additional customer behaviour features.
- Building an automated customer segmentation pipeline.
