# Customer Segmentation Analysis for Marketing Campaign Optimization

🔗 **You can dowload notebook here** https://github.com/omarqanalyst/Customer_Segmentation/blob/main/Capstone_Project_Reference_Notebook_Full_Code_Marketing_Campaign_Analysis_2.0.html

## Objective
Conduct customer segmentation to optimize marketing campaigns by analyzing customer demographics, spending habits, and engagement history.


**Key Questions Addressed:**
* What distinct customer segments exist within the dataset?
* What are the defining characteristics (demographics, spending, engagement) of each segment?
* How do spending patterns differ across segments?
* Which segments represent the highest potential value (and should be focused on)?
* Are there low-engagement or low-value segments that marketing resources should be diverted from?

## Key Findings & Insights (Leveraging DBSCAN results)
![image](https://github.com/user-attachments/assets/b99233a9-aa5f-4b69-8bf8-9e1811318944)


* **Successful Segmentation:** The analysis successfully identified distinct customer segments using DBSCAN on PCA-transformed features.
* **Segment Profiles (Example based on original request - refine with actual cluster analysis):**
    * **Cluster 1 & 4 (High Value/Potential):** Characterized by [e.g., higher spending, frequent purchases, high engagement with specific product categories like Wine/Meat, potentially higher income]. These represent key target groups.
    * **Cluster 0 (Low Engagement/Value):** Characterized by [e.g., lower spending, infrequent purchases, lower campaign acceptance, potentially lower income or different demographic profile]. Resources might be better allocated elsewhere.

![image](https://github.com/user-attachments/assets/ee25134c-aa0c-48fc-b84c-5d30d983545f)
* **DBSCAN Performance:** Achieved a favourable Silhouette Score and generated interpretable clusters that demonstrated clear differences in spending, engagement, and potentially demographic profiles. Outliers were effectively isolated.
* **EDA Insights Reaffirmed:** Clustering often reinforced patterns seen in EDA, such as the importance of income and total spending in differentiating customers.

## What are the diffrent customer segments and how much do they spend on average?
![image](https://github.com/user-attachments/assets/c81671d6-2b8f-44f2-9dea-9b10555e5625)

* Customer profile 0: Represents the largest number of customers, making up 44% of the total customer base.
  
![image](https://github.com/user-attachments/assets/2e01ba9e-46cd-43fd-957e-0520ed417592)

* Customer profile 1: Represents the highest number of purchases, accounting for 26% of all purchases. The overall number of purchases appears to be more consistent compared to revenue.
* Customer profile 2: Demonstrates moderate characteristics across various metrics.
* Customer profile 4: Contributes the highest percentage of revenue, accounting for 35%. They have a moderate number of purchases and a significant average purchase amount..

![image](https://github.com/user-attachments/assets/9b22590d-24d1-4dfa-8d5f-43fb3f8a2c6a)

* Customer profile 3: Exhibits the highest average purchase amount but represents the smallest percentage of customers, only 7%. They make the fewest number of purchases.









## Business Recommendations & Next Steps


Based on the segmentation analysis:

1.  **Targeted Marketing:** Develop distinct marketing campaigns tailored to the specific characteristics and preferences of high-value segments (e.g., Clusters 1 & 4). Focus messaging and offers on products they frequently purchase.
2.  **Resource Optimization:** Reallocate marketing budget and effort away from low-engagement segments (e.g., Cluster 0) towards segments demonstrating higher ROI potential.
3.  **Customer Retention:** For high-value segments, implement loyalty programs or exclusive offers to enhance retention.
4.  **Further Analysis & Refinement:**
    * **Deep Dive Profiling:** Conduct deeper analysis of each segment using demographic data (originally excluded from clustering) to build richer personas.
    * **Track Segment Performance:** Monitor key metrics (conversion rate, CLV, engagement) per segment over time to validate the strategy and refine the model.
    * **A/B Testing:** Test different marketing messages and offers within segments to optimize campaign effectiveness.
    * **Competitor Benchmarking:** Analyze how competitors might be targeting similar segments.

## Technical Stack

* **Programming Language:** Python 3
* **Core Libraries:** Pandas, NumPy, Scikit-learn
* **Visualization:** Matplotlib, Seaborn, Plotly
* **Environment:** Jupyter Notebook

## Dataset Overview

* **Source:** Marketing campaign data.
* **Size:** 2,240 initial customer records, 27 features. (Reduced to 2,208 records after cleaning).
* **Features:** Include customer demographics (Birth Year, Education, Marital Status, Income, Household Composition), spending history (on Wine, Fruits, Meat, etc.), campaign engagement (Accepted Cmp 1-5, Response), purchasing behavior (Web, Catalog, Store purchases, Deals), and engagement metrics (Recency, Web Visits).
* **Data Dictionary:** *(Refer to the notebook or list key features here if preferred)*
    * `ID`: Unique customer identifier
    * `Year_Birth`: Customer's birth year
    * `Education`: Education level
    * `Marital_Status`: Marital status
    * `Income`: Annual household income
    * `Kidhome`/`Teenhome`: Number of children/teens
    * `Dt_Customer`: Enrollment date
    * `Recency`: Days since last purchase
    * `Mnt...Products`: Amount spent on different product categories
    * `Num...Purchases`: Number of purchases via different channels
    * `AcceptedCmp...`/`Response`: Campaign acceptance flags
    * ... *(and other relevant features)*

## Methodology & Analysis Workflow

This project followed a structured data science process:

1.  **Data Loading & Initial Inspection:** Loaded the dataset using `pandas`, checked dimensions, data types, identified initial missing values (`Income`), and checked for duplicates.
2.  **Exploratory Data Analysis (EDA):**
    * **Univariate Analysis:** Examined distributions of numerical features (e.g., `Income`, `Age`, spending amounts, purchase counts) using histograms and boxplots (`matplotlib`, `seaborn`). Analyzed categorical features (`Education`, `Marital_Status`) using count plots. Identified outliers (e.g., `Income`, `Year_Birth`).
    * **Bivariate Analysis:** Investigated relationships between variables using correlation heatmaps (identified correlations like `NumCatalogPurchases` & `MntMeatProducts`) and comparative boxplots (e.g., `Income` across `Education` levels).
3.  **Data Preprocessing & Cleaning:**
    * Handled missing `Income` values by removing affected rows (justified by small percentage ~1%).
    * Addressed outliers identified during EDA (e.g., removed extreme high `Income` value, unrealistic `Year_Birth` values implying age > 115).
    * Converted `Dt_Customer` to datetime objects.
    * Consolidated sparse categories in `Education` ('2n Cycle' grouped with 'Master') and `Marital_Status` ('Alone', 'Absurd', 'YOLO' grouped with 'Single'; 'Married', 'Together' grouped as 'Relationship').
4.  **Feature Engineering:** Created new features to capture richer customer insights:
    * `Age`: Calculated from `Year_Birth`.
    * `Kids`: Combined `Kidhome` and `Teenhome`.
    * `Family_Size`: Derived from `Marital_Status` grouping and `Kids`.
    * `Expenses`: Total spending across all product categories.
    * `NumTotalPurchases`: Sum of purchases across all channels.
    * `Engaged_in_days`: Calculated customer tenure based on `Dt_Customer`.
    * `TotalAcceptedCmp`: Total number of campaigns accepted.
    * `AmountPerPurchase`: Average spending per transaction (handled division-by-zero cases).
5.  **Data Preparation for Modeling:**
    * Selected relevant features for clustering, dropping identifiers, raw date/demographic columns used for feature engineering, and individual campaign flags.
    * Scaled numerical features using `StandardScaler` from `scikit-learn` to ensure equal weighting in distance-based algorithms.
6.  **Dimensionality Reduction (PCA):**
    * Applied Principal Component Analysis (PCA) to the scaled feature set.
    * **Purpose:** To reduce multicollinearity observed in the correlation matrix and simplify the feature space while retaining most of the variance, potentially leading to more stable and interpretable clusters.
7.  **Clustering Algorithm Selection & Application:**
    * Considered multiple algorithms suitable for customer segmentation (K-Means, K-Medoids, Hierarchical, DBSCAN, GMM planned as per notebook).
    * **Selected DBSCAN:** Chosen for its ability to find arbitrarily shaped clusters, robustness to outliers (prevalent in spending/behavioral data), and not requiring the number of clusters (k) to be pre-specified.
8.  **Evaluation:**
    * **Quantitative:** Used the **Silhouette Score** to measure cluster separation and cohesion.
    * **Qualitative:** Analyzed cluster characteristics using **boxplots** across key features (e.g., Income, Expenses, Recency) to assess the meaningfulness and actionability of the identified segments.

