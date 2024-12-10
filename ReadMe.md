# Credit Card Behavior: A Data-Driven Insight into Customer Spending


![Alt Text](results/EDA_results//cover.2024-12-09%20202121.png)

## Context

Delimiting credit card spending behavior is a common challenge in the financial industry. This task involves analyzing multiple aspects of credit card usage, such as balance, purchases, cash advances, credit limits, and total payments. Although credit card usage may seem random at first glance, there are patterns that can help infer and trace behavioral profiles based on these variables.

## Feature description

This study is based on a real-world dataset containing information from 9,000 credit card accounts that have been active in the last six months at a financial institution. The dataset includes 18 behavioral variables related to card usage. The marketing team aims to apply different marketing strategies tailored to specific customer groups.

#### Features

- **CUST_ID**: Unique identifier for each credit card holder.
- **BALANCE**: Monthly average balance (calculated based on daily balance averages).
- **BALANCE_FREQUENCY**: Ratio of months in the last year with a balance.
- **PURCHASES**: Total purchase amount spent over the last 12 months.
- **ONEOFF_PURCHASES**: Total amount spent on one-off purchases.
- **INSTALLMENTS_PURCHASES**: Total amount spent on installment purchases.
- **CASH_ADVANCE**: Total cash advance amount taken by the customer.
- **PURCHASES_FREQUENCY**: Frequency of purchases (percentage of months with at least one purchase).
- **ONEOFF_PURCHASES_FREQUENCY**: Frequency of one-off purchases over the period.
- **PURCHASES_INSTALLMENTS_FREQUENCY**: Frequency of installment purchases.
- **CASH_ADVANCE_FREQUENCY**: Frequency of cash advances.
- **AVERAGE_PURCHASE_TRX**: Average amount per purchase transaction.
- **CASH_ADVANCE_TRX**: Average amount per cash advance transaction.
- **PURCHASES_TRX**: Number of purchase transactions.
- **CREDIT_LIMIT**: Maximum credit limit for the cardholder.
- **PAYMENTS**: Total amount paid by the customer to reduce their balance during the period.
- **MINIMUM_PAYMENTS**: Total minimum payments required over the period.
- **PRC_FULL_PAYMENT**: Percentage of months where the customer fully paid off the balance.
- **TENURE**: Number of months the customer has held the credit card.

## Objectives
The goal of this analysis is to derive meaningful insights from customer behavior data by creating new Key Performance Indicators (KPIs) and clustering clients based on their behavior. This will enable the marketing team to design targeted strategies for different customer segments. 

Our key objectives are:
- Construct enhanced customer behavior profiles by deriving new KPIs from the existing variables.
- Cluster customers into distinct behavioral segments based on their credit card usage patterns.
- Identify key characteristics of each cluster using detailed behavioral profiles.
- Provide strategic recommendations for targeted marketing based on the clustering results.

## Project outline
This project will cover the following key techniques:
- **Data exploration and preprocessing**: Initial analysis and cleaning of the dataset.
- **Handling missing data**: Managing incomplete information effectively.
- **Outlier detection and treatment**: Identifying and mitigating outliers that could skew results.
- **KPI construction**: Developing new metrics to better capture customer behaviors.
- **Clustering model construction**: Grouping clients based on similar usage patterns using clustering algorithms.
- **Model evaluation**: Assessing the quality and effectiveness of the clustering model.

## Exploratory Data Analysis (EDA)
### Dealing with Missing Values

Upon analyzing the missing data, we found that 3.5% of the values are missing in the **Minimum Payment** column, while the **Credit Limit** column has only one missing value (0.011%). Since the missing value in the **Credit Limit** column is isolated, it is more efficient to drop this single entry.


| Column Name                          | Percent of Missing Values | Count of Missing Values |
|--------------------------------------|---------------------------|--------------------------|
| BALANCE                              | 0.000                     | 0                        |
| BALANCE_FREQUENCY                    | 0.000                     | 0                        |
| PURCHASES                            | 0.000                     | 0                        |
| ONEOFF_PURCHASES                     | 0.000                     | 0                        |
| INSTALLMENTS_PURCHASES               | 0.000                     | 0                        |
| CASH_ADVANCE                         | 0.000                     | 0                        |
| PURCHASES_FREQUENCY                  | 0.000                     | 0                        |
| ONEOFF_PURCHASES_FREQUENCY           | 0.000                     | 0                        |
| PURCHASES_INSTALLMENTS_FREQUENCY     | 0.000                     | 0                        |
| CASH_ADVANCE_FREQUENCY               | 0.000                     | 0                        |
| CASH_ADVANCE_TRX                     | 0.000                     | 0                        |
| PURCHASES_TRX                        | 0.000                     | 0                        |
| CREDIT_LIMIT                         | 0.011                     | 1                        |
| PAYMENTS                             | 0.000                     | 0                        |
| MINIMUM_PAYMENTS                     | 3.497                     | 313                      |
| PRC_FULL_PAYMENT                     | 0.000                     | 0                        |
| TENURE                               | 0.000                     | 0                        |


For the **Minimum Payment** column, the distribution is highly skewed to the left, with a high concentration of values around the median. Given this skewness and **without having context of why this data is missing** imputing the missing values with the median is a practical and parsimonious solution.

![Eda Dist](results/EDA_results/eda_dist.png)

### Treating outliers

![Feature Boxplot](results/EDA_results/feature_boxplot.png)

We observe from the boxplots above that outliers are indeed present across several features. Addressing these outliers is crucial for maintaining the integrity of our analysis and improving model performance.

To address outliers in the dataset, I propose applying the **Interquartile Range (IQR) Method** for the following reasons:

**1. Robustness:** The IQR method focuses on the middle 50% of the data (between the 1st and 3rd quartiles). This makes it **less sensitive to extreme values** compared to methods that rely on the mean and standard deviation. By using this method, we are better able to detect genuine patterns in the data without being skewed by outliers.

**2. Efficiency:** The IQR method is **straightforward and computationally efficient**, allowing us to easily identify and remove or cap outliers. It involves calculating the range between the 25th percentile (Q1) and the 75th percentile (Q3), and then flagging any values that fall below Q1 - 1.5 * IQR or above Q3 + 1.5 * IQR as outliers.

**3. Broad Applicability:** This approach works well for a wide range of data distributions, including those with **skewness**, which is common in our dataset. The IQR method is non-parametric, meaning it does not assume a specific distribution, making it widely applicable.

## Understanding customer behaviour profiles via Key Performance Indicators (KPI's)

#### **Monthly Average Purchase**

Measures the average amount spent by a customer on purchases each month. It is calculated by dividing the total purchases made by the customer (`PURCHASES`) by the total tenure of the customer relationship in months (`TENURE`).

##### Interpretation
- **Customer Spending Behavior**: This KPI provides insights into the spending behavior of customers. A higher value indicates that customers are spending more on average each month.

#### **Monthly Average Cash Advance Amount**
Measures the average amount of cash withdrawn by a customer each month. It is calculated by dividing the total cash advances made by the customer (`CASH_ADVANCE`) by the total tenure of the customer relationship in months (`TENURE`).

##### Interpretation
- **Customer Cash Withdrawal Behavior**: This KPI provides insights into how often and how much customers are utilizing cash advances. A higher average indicates that customers are relying more on cash advances, which may reflect their financial needs or spending behavior.

##### Division of Customer By Type of Purchases

These categories were created to classify customers based on their purchasing behavior. By separating them into distinct types (no purchases, one-off only, installment only, or both), one can analyze trends and target marketing or customer retention strategies based on how individuals prefer to make purchases.

**Types of Purchase Categories**

1. **None_Of_the_Purchases**:
   - **Condition**: `(df['ONEOFF_PURCHASES'] == 0) & (df['INSTALLMENTS_PURCHASES'] == 0)`
   - **Explanation**: This category represents customers who made neither one-off purchases nor installment purchases.

2. **One_Of_Purchase**:
   - **Condition**: `(df['ONEOFF_PURCHASES'] > 0) & (df['INSTALLMENTS_PURCHASES'] == 0)`
   - **Explanation**: This category includes customers who made only one-off purchases but no installment purchases.

3. **Installment_Purchases**:
   - **Condition**: `(df['ONEOFF_PURCHASES'] == 0) & (df['INSTALLMENTS_PURCHASES'] > 0)`
   - **Explanation**: This category represents customers who only made installment purchases without any one-off purchases.

4. **Both_the_Purchases**:
   - **Condition**: `(df['ONEOFF_PURCHASES'] > 0) & (df['INSTALLMENTS_PURCHASES'] > 0)`
   - **Explanation**: This category represents customers who made both one-off and installment purchases.

![Percetage Purches](results/EDA_results/purchase_types.png)

Approximately 31% of customers engage in both one-off and installment purchases, while 25.3% exclusively make installment purchases

##### Estimating the limit usage of customers 

- **Formula**: `df['Limit_Usage'] = df['BALANCE'] / df['CREDIT_LIMIT']`
  
- **Explanation**: 
   - This variable represents the proportion of the available credit that a customer has utilized. It is calculated by dividing the current balance (`BALANCE`) by the total credit limit (`CREDIT_LIMIT`).
   - A higher value indicates that a customer is using a larger portion of their credit, which may suggest higher financial activity or potential risk in terms of credit management.
   - On the other hand, a lower value indicates that the customer is using less of their credit limit, which might suggest financial stability or less reliance on credit.

##### Interpretation 

- **Credit Utilization**: The `Limit_Usage` variable helps in understanding credit utilization, a critical factor in financial analysis and risk assessment.
- **Behavior Insight**: It allows us to gauge how much of their credit line customers are using. This feature can be useful in segmentation and clustering analysis to group customers based on their credit usage behavior.

##### Payment to Minimum Payments Ratio

- **Formula**: `df['Pay_to_MinimumPay'] = df['PAYMENTS'] / df['MINIMUM_PAYMENTS']`
  
- **Explanation**: 
   - This variable represents the ratio of the total payments made by a customer (`PAYMENTS`) to the minimum payments required (`MINIMUM_PAYMENTS`). 
   - A value greater than 1 means the customer is paying more than the minimum, indicating strong financial discipline and a desire to reduce their outstanding balance faster.
   - A value equal to 1 means the customer is paying exactly the minimum, which is meeting the requirements but not reducing the balance significantly.
   - A value less than 1 indicates that the customer is paying less than the minimum, which could signal financial difficulties or delinquency.

##### Interpretation?
- **Payment Behavior Insight**: The `Pay_to_MinimumPay` variable helps assess a customer's payment behavior, offering insights into their creditworthiness and financial responsibility.
- **Risk Assessment**: This feature is crucial for risk profiling and can be useful in identifying customers who may be at risk of default if they consistently pay less than the minimum required.

##### Minimun Pay per Purchase Type

We can assess how minimum payments are related to each purchase type:

![Alt Text](results/EDA_results/minimunPay_purchase_type.png)

##### **Insights**

- **Highest Payments in Installment Purchases**:  
  Customers who make installment purchases show the highest average payments relative to their minimum payments, with a ratio of 13.93. This indicates that customers in this group tend to pay significantly more than the required minimum, which might reflect a preference for long-term payment strategies with larger financial commitments.

- **Moderate Payments in 'None' and 'Both' Purchase Types**:  
  Both "None_Of_the_Purchases" and "Both_the_Purchases" groups have moderate payments above the minimum, with ratios of 10.46 and 7.31, respectively. These customers likely manage their payments in a more balanced way, avoiding both extremes of paying too little or too much beyond the minimum.

- **Lowest Payments in One-Off Purchases**:  
  The "One_Of_Purchase" group shows the lowest average payments relative to the minimum, with a ratio of 5.74. This suggests that customers making one-off purchases are more likely to pay closer to the minimum amount, which could indicate a more cautious approach to credit use, or possibly more conservative financial behavior.

- **Variance in Installment Purchases**:  
  The high average for installment purchases, coupled with significant variability, could indicate the presence of outliers or individuals who make large payments beyond the minimum. This may point to a smaller group of financially aggressive payers or require further investigation to assess whether such high payments are common in this group.

##### Credit Limit Balance per Purchase Type

![Limit PT](results/EDA_results/limit_balance_PT.png)

**Balance-to-Limit Ratios**:  
  The **Both_the_Purchases** group has a balance-to-limit ratio of **0.3535**, indicating a moderate level of credit utilization. This suggests that these customers are managing their credit effectively.  
  The **Installment_Purchases** group displays the lowest balance-to-limit ratio of **0.2717**, reflecting a favorable credit risk profile, as lower ratios are desirable. This indicates that customers making installment purchases maintain lower utilization rates, potentially signaling better credit management.  
  The **None_Of_the_Purchases** group has a higher ratio of **0.5740**, which could indicate a more significant credit risk, as they are utilizing over half of their available credit.  
  Finally, the **One_Of_Purchase** group shows a balance-to-limit ratio of **0.3811**, suggesting a moderate level of credit use that could warrant further monitoring.

- **KPI Summary**:  
  - **Both_the_Purchases**: Average Payment = **2774.0**, Balance-to-Limit Ratio = **0.3535**  
  - **Installment_Purchases**: Average Payment = **2260.0**, Balance-to-Limit Ratio = **0.2717**  
  - **None_Of_the_Purchases**: Average Payment = **2041.0**, Balance-to-Limit Ratio = **0.5740**  
  - **One_Of_Purchase**: Average Payment = **1874.0**, Balance-to-Limit Ratio = **0.3811**  

- **Credit Risk Insight**:  
  A lower balance-to-limit ratio is desirable, indicating lower credit risk. The customers who make installment purchases have the lowest utilization rate, suggesting better financial health and management of credit.

##### Monthly Average Purchase per Purchase Type

![Montly Purchases per Purchase Type](results/EDA_results/monthly_purchases_PT.png)

##### Insights:

- **Highest Average Purchase Amount**:  
  Customers who made both one-off and installment purchases have achieved the highest total average purchase amount over the last 12 months, with an average of **$192.69**. This suggests a strong purchasing behavior and possibly a higher engagement with the credit system.

- **Engagement and Spending Patterns**:  
  The high average for customers making both purchase types suggests a strong engagement with their credit, reflecting positive financial behavior. The installment purchases, while lower in average, still indicate a responsible approach to spending, as evidenced by the significantly lower standard deviation.

- **Credit Usage Insight**:  
  Understanding these purchasing behaviors can inform strategies for credit offers and customer engagement, focusing on the different spending patterns of customers based on their purchase types.

##### Monthly Cash Advance per Purchase Type  

![text](results/EDA_results/cash_advance_PT.png)

- **Highest Average Purchase Amount**:  
  Customers who made both one-off and installment purchases have achieved the highest total average purchase amount over the last 12 months, with an average of **$192.69**. This suggests a strong purchasing behavior and possibly a higher engagement with the credit system.

- **Engagement and Spending Patterns**:  
  The high average for customers making both purchase types suggests a strong engagement with their credit, reflecting positive financial behavior. The installment purchases, while lower in average, still indicate a responsible approach to spending, as evidenced by the significantly lower standard deviation.

- **Credit Usage Insight**:  
  Understanding these purchasing behaviors can inform strategies for credit offers and customer engagement, focusing on the different spending patterns of customers based on their purchase types.

## Investigating covariables correlations

To reduce redundancy in the dataset, we will drop the original variables: **'BALANCE'**, **'PURCHASES'**, **'PAYMENTS'**, **'MINIMUM_PAYMENTS'**, **'TENURE'**, and **'CASH_ADVANCE'**. These variables have been utilized to create new derived variables, referred to as KPIs, and may introduce correlations that can complicate analysis. Removing them will streamline the dataset and improve the clarity of our insights.

![Correlations](results/EDA_results/correlations.png)

A correlation value above 0.50 is considered highly correlated. From the heatmap, we can observe the following:

1. **ONEOFF_PURCHASES** is highly positively correlated with **MONTHLY_AVG_PURCHASES** with a correlation of **0.91**.
2. **INSTALLMENTS_PURCHASES** is correlated with **PURCHASES_TRX**, showing a correlation of **0.63**, and with **MONTHLY_AVG_PURCHASES**, having a correlation of **0.68**.
3. **PURCHASES_FREQUENCY** is positively correlated with **PURCHASES_INSTALLMENTS_FREQUENCY**, showing a correlation of **0.86**, and also with **PURCHASES_TRX**, which has a correlation of **0.57**.
4. **CASH_ADVANCE_FREQUENCY** is highly positively correlated with **CASH_ADVANCE_TRX**, with a correlation of **0.80**, and with **AVG_CASH_ADVANCE**, which has a correlation of **0.63**.
5. **CASH_ADVANCE_TRX** is also positively correlated with **AVG_CASH_ADVANCE**, with a correlation of **0.63**.
6. **PURCHASES_TRX** is positively correlated with **MONTHLY_AVG_PURCHASES**, showing a correlation of **0.68**.

This indicates the presence of multicollinearity in the data, suggesting the need to utilize dimension reduction techniques, as redundant information is being fed into the model.

![Pairpllot](results/EDA_results/pairplot.png)

Through the readability of the pairplot is kind of difficult, we can observe some linear and nonlinear relationships between variables.

## Feature preparing for Modeling

For our unique categorical variable ['PURCHASE_TYPE']  we will use pandas to get dummy variables 

### Dimensionality Reduction through Principal Component Analysis (PCA)
1. Standardize our numerical variables using Standard Scaler.
2. Calculating eigenvalues and eigenvectors from the covariance matrix.
3. Plotting the explained variance by principal components

![PCA plot](results/EDA_results/explained_variance.png)

| Principal Component | Eigen Values | Cumulative Variance |
|----------------------|--------------|----------------------|
| 1                    | 5.5121       | 25.5985             |
| 2                    | 4.3831       | 45.9538             |
| 3                    | 1.7927       | 54.2794             |
| 4                    | 1.5916       | 61.6708             |
| 5                    | 1.1306       | 66.9214             |
| 6                    | 1.0927       | 71.9959             |
| 7                    | 0.9784       | 76.5397             |
| 8                    | 0.8769       | 80.6122             |
| 9                    | 0.7394       | 84.0461             |
| 10                   | 0.6660       | 87.1388             |
| 11                   | 0.6070       | 89.9576             |
| 12                   | 0.4693       | 92.1372             |
| 13                   | 0.4223       | 94.0985             |
| 14                   | 0.3008       | 95.4955             |
| 15                   | 0.2726       | 96.7613             |
| 16                   | 0.1887       | 97.6377             |
| 17                   | 0.1538       | 98.3521             |
| 18                   | 0.1792       | 99.1843             |
| 19                   | 0.0888       | 99.5966             |
| 20                   | 0.0381       | 99.7738             |
| 21                   | 0.0281       | 99.9043             |
| 22                   | 0.0044       | 99.9247             |
| 23                   | 0.0000       | 99.9247             |
| 24                   | 0.0162       | 100.0000            |

### Chosing Seven Principal Components

Our decision to retain seven principal components is based on a balance between maximizing explained variance and maintaining interpretability. Although all 17 components explain variance, focusing on a smaller subset allows us to simplify our analysis without losing significant information.

### Rationale for Retaining Seven Principal Components

1. **Variance Explained**: 
   - From both manual computation of PCA components and PCA via sklearn, we observe that all 17 components explain varying amounts of variance.
   - The first seven principal components cumulatively explain approximately **85%** of the total variance, with each individual component contributing more than **0.7** to the explained variance. This indicates that these components effectively capture the essential variability in the dataset.

2. **Diminishing Returns**:
   - Analyzing the eigenvalues reveals diminishing returns as we progress beyond the seventh component. The eighth component explains only a small increase in variance, with cumulative variance rising to approximately **80.61%**.
   - Retaining more components beyond the seventh introduces minimal additional explanatory power, making those components less meaningful for our analysis.

3. **Information Loss**:
   - By selecting seven dimensions out of the 17 variables, we are only losing about **15%** of the variation (information) from the data.
   - This slight reduction in explained variance is acceptable given the substantial amount of information retained for further analysis.

In conclusion, selecting **seven principal components** provides a strong balance between capturing significant variance and maintaining interpretability. This decision aligns with data science best practices for dimensionality reduction, ensuring we retain crucial information while minimizing the risk of overfitting. These components will serve as a solid foundation for subsequent modeling and analysis.

## Analysing Feature Importance in PCA loadings

![Loadings](results/EDA_results/PCA_loadings.png)

1. **Principal Component 1 (PC_1)**:
   - **High Positive Loadings**: 
     - **PURCHASES (0.39)**: Indicates that higher purchases correlate positively with this component.
     - **ONEOFF_PURCHASES (0.34)**: Suggests that one-off purchases also contribute significantly.
   - **High Negative Loadings**:
     - **None significant** in this component.

2. **Principal Component 2 (PC_2)**:
   - **High Positive Loadings**:
     - **CASH_ADVANCE (0.41)**: Reflects a strong relationship between cash advances and this component.
     - **BALANCE_FREQUENCY (0.13)** and **CREDIT_LIMIT (0.20)** also have noteworthy contributions but are lower than other variables.
   - **High Negative Loadings**:
     - **PURCHASES_FREQUENCY (-0.14)**: Indicates that higher purchase frequency negatively affects this component.

3. **Principal Component 3 (PC_3)**:
   - **High Positive Loadings**:
     - **BALANCE_FREQUENCY (0.45)**: Indicates a strong relationship, suggesting that the frequency of balance impacts this component significantly.
     - **PURCHASES_INSTALLMENTS_FREQUENCY (0.25)** also indicates a notable influence but is less than the others.
   - **High Negative Loadings**:
     - **CREDIT_LIMIT (-0.13)**: Suggests that a higher credit limit inversely affects this component.

4. **Principal Component 4 (PC_4)**:
   - **High Positive Loadings**:
     - **PURCHASES_INSTALLMENTS_FREQUENCY (0.49)**: Indicates a significant positive impact.
     - **MINIMUM_PAYMENTS (0.31)** also suggests a strong relationship, highlighting its relevance to this component.
   - **High Negative Loadings**:
     - **ONEOFF_PURCHASES (-0.32)**: Indicates that one-off purchases negatively impact this component.

5. **Principal Component 6 (PC_6)**:
   - **High Positive Loadings**:
     - **CREDIT_LIMIT (0.42)**: Indicates a strong positive association, suggesting that higher credit limits are relevant to this component.
   - **High Negative Loadings**:
     - **INSTALLMENTS_PURCHASES (-0.28)**: Suggests an inverse relationship.

6. **Principal Component 7 (PC_7)**:
   - **High Positive Loadings**:
     - **Pay_to_MinimumPay (0.92)**: Demonstrates a very strong positive relationship, indicating that this variable significantly defines this component.

Overall, the loadings suggest that **PC_1** and **PC_4** are heavily influenced by **PURCHASES** and **PURCHASES_INSTALLMENTS_FREQUENCY**.

**PC_2** and **PC_3** are significantly impacted by **CASH_ADVANCE** and **BALANCE_FREQUENCY**.

**PC_6** is dominated by **CREDIT_LIMIT**.

**PC_7** is primarily defined by **Pay_to_MinimumPay**.

## Machine Learning Clustering
In this section, we will explore the clustering model used in our analysis. We will review key insights from EDA, including important KPIs and findings from the PCA. 

We will implement two clustering algorithms: **K-means** and **Hierarchical Clustering**, and determine the optimal number of clusters by comparing **silhouette scores** across different cluster numbers. Additionally, we will perform profiling on the resulting clusters to ensure meaningful segmentation.

The main objectives of this section are:
- Segment customers into distinct groups based on their credit card usage behaviors.
- Identify the key characteristics of each cluster through detailed behavioral profiling.
- Provide strategic recommendations for targeted marketing initiatives based on the clustering results.

### Calculating Cophenetic Correlations

The cophenetic distance correlation coefficient is a valuable metric in evaluating the accuracy of different linkage methods in hierarchical clustering. For our Agglomerative Clustering algorithm, this coefficient helps us determine which linkage method best preserves the original pairwise distances in the dataset.

For this i made this small function, that will take the output from our PCA and calculate the cophenetic correlation coefficient for various linkages and distance metrics.


```python
def calculate_cophenetic_distances(pca_data, linkage_methods, distance_metrics):
    """
    Calculate the cophenetic distance correlation coefficient for various linkage methods 
    and distance metrics, including Ward's linkage.

    Parameters:
    - pca_data: PCA-transformed data (numpy array or DataFrame)
    - linkage_methods: List of linkage methods to evaluate (e.g., ['single', 'complete', 'average'])
    - distance_metrics: List of distance metrics to evaluate (e.g., ['euclidean', 'cityblock', 'cosine'])

    Returns:
    - results: Dictionary containing cophenetic correlation coefficients for each method-metric combination
    """
    results = {}

    for method in linkage_methods:
        print(f"\nLinkage Method: {method}")
        results[method] = {}
        
        for metric in distance_metrics:
            Z = linkage(pca_data, method=method, metric=metric)
            c, _ = cophenet(Z, pdist(pca_data, metric=metric))
            results[method][metric] = c
            print(f"Cophenetic Correlation (Metric: {metric}): {c:.4f}")
    
    # Ward's linkage (default distance metric is Euclidean)
    Z_ward = linkage(pca_data, method='ward')
    c_ward, _ = cophenet(Z_ward, pdist(pca_data))
    results['ward'] = {'euclidean': c_ward}
    print(f"\nCophenetic Correlation for Ward's Linkage (Euclidean Distance): {c_ward:.4f}")
    
    return results
  ```

Using this results we achieve this table 

| Linkage Method | Metric      | Cophenetic Correlation |
|----------------|-------------|-------------------------|
| Single         | Euclidean   | 0.8054                 |
| Single         | Cityblock   | 0.8079                 |
| Single         | Cosine      | 0.0558                 |
| Complete       | Euclidean   | 0.7557                 |
| Complete       | Cityblock   | 0.7616                 |
| Complete       | Cosine      | 0.4585                 |
| Average        | Euclidean   | 0.8855                 |
| Average        | Cityblock   | 0.8652                 |
| Average        | Cosine      | 0.6121                 |
| Ward           | Euclidean   | 0.4392                 |

Based on the output of our analysis, the **Average Linkage Method** combined with **Euclidean Distances** provides the highest cophenetic correlation, making it the most reliable choice for clustering in this particular dataset.

### Finding optimal number of clusters for K-means and Agglomerative Clustering

'find_optimal_clusters' will take our PCA dataframe and a cluster range and compute these metrics for Agglomerative Clustering and K-means:
- Inertia (WCSS)
- Silhouette Score

Number of Clusters: 3
K-Means Inertia (WCSS): 99397.8355487756
Silhouette Score for K-Means: 0.2980
Silhouette Score for Agglomerative Clustering: 0.8689

Number of Clusters: 4
K-Means Inertia (WCSS): 84676.22005514555
Silhouette Score for K-Means: 0.2516
Silhouette Score for Agglomerative Clustering: 0.8205

Number of Clusters: 5
K-Means Inertia (WCSS): 75906.79619660013
Silhouette Score for K-Means: 0.2444
Silhouette Score for Agglomerative Clustering: 0.8200

Besides the better perfomance of selecting 3 clusters, i want to explore more on 4 and 5, i want to see some more delailed segmentation and try to capture some different patters that might be overlookeed by a smaller cluster size. 

By using elbow and sillhouete plot we see that indeed 4 and 5 are less cohesive, lets explore this by plotting the agglomerative clustering dendogram.

![dendogram](results/ML_results/figures/dendrogram_plot.png)

The dendogram show that inded by using the best distance compute, we have arguments to retain 4 clusters, lets visualize these against 5 clusters.

##### Selecting for 4 Clusters

![4CL](results/ML_results/figures/kmeans_clusters4_plot.png)

##### Selecting for 5 Clusters

![5CL](results/ML_results/figures/kmeans_clusters5_plot.png)

By visualy inspecting the plots of all PCA components and their clusters we see that by selecting for 5 we are capturing some more of the scatter (pink dots) data then with 4.

Lets explore more of this pattern using Segment Distribution Analysis 

### Segment Distribution Analysis
Segment distribution will repturn to us the proportion of data points assigned to each cluster in our cluster analysis
'calculate_segment_distribution' function should callculate the segment distribution for each cluster for a specified cluster size.


```python
def calculate_segment_distribution(updated_df, target_clusters):
    """
    Calculate the segment distribution for the specified target cluster size.

    Parameters:
    - updated_df: DataFrame containing cluster labels.
    - target_clusters: Number of clusters for which to return the segment distribution.

    Returns:
    - None
    """
    if f'Cluster_{target_clusters}' in updated_df.columns:
        segment_distribution = (
            pd.Series.sort_index(updated_df[f'Cluster_{target_clusters}'].value_counts())
            / sum(updated_df[f'Cluster_{target_clusters}'].value_counts())
        )
        print(f"Segment Distribution for cluster K={target_clusters}:\n", segment_distribution)
    else:
        print(f"Target cluster size {target_clusters} does not exist in the DataFrame.")
```


For K=4 

Segment Distribution for cluster K=4:
 0    0.455917
1    0.126718
2    0.028383
3    0.388982
Name: Cluster_4, dtype: float64

Cluster 0 contains 45.59% of the total data points.
Cluster 1 contains 12.67% of the total data points.
Cluster 2 contains 2.84% of the total data points.
Cluster 3 contains 38.90% of the total data points.

For K=5

Segment Distribution for cluster K=5:
 0    0.119902
1    0.362946
2    0.087161
3    0.003352
4    0.426640
Name: Cluster_5, dtype: float64

Cluster 0 contains 11.99% of the total data points.
Cluster 1 contains 36.29% of the total data points.
Cluster 2 contains 8.72% of the total data points.
Cluster 3 contains 0.34% of the total data points.
Cluster 4 contains 42.66% of the total data points.

We will follow this analysis using profiling to try to differentiate better the groups based upon their behaviour characteristics

### Profiling Process


The goal of Profiling is to differentiate between groups (clusters) based on distinct characteristics. An ideal cluster should represent a group of observations with unique traits that distinguish them from other clusters.

The profilling will follow these steps: 

1. **Count the number of observations per segment (value_counts)**:
    - For each cluster, calculate how many observations (or records) are present. This gives the size of each cluster.

2. **Calculate the average for each variable**:
    - **Overall average**: Compute the average of each variable across the entire dataset.
    - **Segment-wise average**: Compute the average of each variable for each cluster (segment). This helps identify how clusters differ from the overall dataset.

3. **Repeat the above steps for different cluster values (K)**:
    - Apply the same steps for each K value (different numbers of clusters) to determine which clustering solution provides the most distinct and meaningful segmentation of the data.

By following these steps, the best clustering solution can be identified, allowing for effective segmentation and better differentiation between clusters.

This is what we achieve after the steps 

| **Segment** | **K4 Cluster 1** | **K4 Cluster 2** | **K4 Cluster 3** | **K4 Cluster 4** | **K5 Cluster 1** | **K5 Cluster 2** | **K5 Cluster 3** | **K5 Cluster 4** | **K5 Cluster 5** | **Overall** |
|-------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|------------------|-------------|
| **Seg_size** | 4080.000000 | 1134.000000 | 254.000000 | 3481.000000 | 1073.000000 | 3248.000000 | 780.000000 | 30.000000 | 3818.000000 | 8949.000000 |
| **Seg_Pct** | 0.455917 | 0.126718 | 0.028383 | 0.388982 | 0.119902 | 0.362946 | 0.087161 | 0.003352 | 0.426640 | 1.000000 |
| **Installment_Purchases** | 0.180637 | 0.103175 | 0.035433 | 0.401321 | 0.105312 | 0.497537 | 0.023077 | 0.066667 | 0.133840 | 0.252542 |
| **None_Of_the_Purchases** | 0.375490 | 0.448854 | 0.000000 | 0.000000 | 0.449208 | 0.000000 | 0.000000 | 0.000000 | 0.408329 | 0.228070 |
| **One_Of_Purchase** | 0.327451 | 0.211640 | 0.098425 | 0.078426 | 0.208760 | 0.069273 | 0.094872 | 0.100000 | 0.353064 | 0.209409 |
| **BALANCE** | 1106.624986 | 4542.902787 | 4046.210995 | 950.190178 | 4585.833917 | 668.038082 | 2646.309946 | 5567.142164 | 1225.905658 | 1564.647593 |
| **BALANCE_FREQUENCY** | 0.801298 | 0.961876 | 0.984082 | 0.931165 | 0.960927 | 0.892737 | 0.982140 | 0.957273 | 0.818736 | 0.877350 |
| **PURCHASES** | 268.213414 | 541.627628 | 9765.340236 | 1375.975404 | 510.233840 | 920.741438 | 4720.194897 | 24957.905000 | 264.574382 | 1003.316936 |
| **ONEOFF_PURCHASES** | 209.217733 | 343.399594 | 6673.724370 | 679.162594 | 318.277987 | 358.283787 | 3075.924603 | 18186.875667 | 223.223937 | 592.503572 |
| **INSTALLMENTS_PURCHASES** | 59.329130 | 198.317690 | 3091.615866 | 697.164573 | 192.050606 | 562.803030 | 1645.039526 | 6771.029333 | 41.576524 | 411.113579 |
| **CASH_ADVANCE** | 585.817361 | 4860.109536 | 637.624213 | 200.302788 | 5003.391108 | 157.746907 | 439.394980 | 1858.844605 | 649.873119 | 978.959616 |
| **PURCHASES_FREQUENCY** | 0.178564 | 0.295700 | 0.936883 | 0.886757 | 0.294823 | 0.835306 | 0.940305 | 0.910556 | 0.156748 | 0.490405 |
| **ONEOFF_PURCHASES_FREQUENCY** | 0.090526 | 0.146485 | 0.761531 | 0.311149 | 0.140819 | 0.217872 | 0.712005 | 0.773889 | 0.098132 | 0.202480 |
| **PURCHASES_INSTALLMENTS_FREQUENCY** | 0.085359 | 0.190750 | 0.761411 | 0.719259 | 0.190818 | 0.689006 | 0.735534 | 0.754444 | 0.058335 | 0.364478 |
| **CASH_ADVANCE_FREQUENCY** | 0.121176 | 0.488064 | 0.066929 | 0.041515 | 0.491533 | 0.035089 | 0.065122 | 0.083333 | 0.134807 | 0.135141 |
| **CASH_ADVANCE_TRX** | 2.269853 | 14.668430 | 1.901575 | 0.775065 | 14.940354 | 0.646860 | 1.457692 | 3.666667 | 2.539811 | 3.249078 |
| **PURCHASES_TRX** | 3.068382 | 8.252205 | 99.570866 | 24.270325 | 7.931034 | 17.935961 | 63.430769 | 148.000000 | 2.873494 | 14.711476 |
| **CREDIT_LIMIT** | 3291.949346 | 7511.071028 | 10427.362205 | 4488.241957 | 7568.550369 | 3857.143961 | 8246.987179 | 15570.000000 | 3319.020430 | 4494.449450 |
| **PAYMENTS** | 954.168270 | 3754.745798 | 9022.795872 | 1456.176389 | 3807.870653 | 1056.474880 | 4433.834686 | 25178.882690 | 990.203059 | 1733.336511 |
| **MINIMUM_PAYMENTS** | 644.082143 | 1895.101032 | 2293.372330 | 632.725874 | 1928.053101 | 505.118404 | 1391.490714 | 3475.059479 | 697.458083 | 845.003358 |
| **PRC_FULL_PAYMENT** | 0.069020 | 0.040946 | 0.310001 | 0.278360 | 0.041244 | 0.286041 | 0.275032 | 0.478409 | 0.045456 | 0.153732 |
| **TENURE** | 11.456127 | 11.302469 | 11.940945 | 11.629704 | 11.296365 | 11.563116 | 11.912821 | 11.933333 | 11.457831 | 11.517935 |
| **MONTHLY_AVG_PURCHASES** | 23.918537 | 48.708372 | 820.291078 | 117.808371 | 46.138778 | 79.635722 | 397.479664 | 2094.593383 | 23.633348 | 86.184802 |
| **AVG_CASH_ADVANCE** | 53.642893 | 442.961347 | 55.609205 | 17.528269 | 456.246036 | 14.196478 | 37.619720 | 154.903717 | 59.368560 | 88.984447 |
| **Limit_Usage** | 0.424776 | 0.633578 | 0.396603 | 0.266648 | 0.632107 | 0.238067 | 0.338388 | 0.391508 | 0.459225 | 0.388926 |
| **Pay_to_MinimumPay** | 8.196323 | 5.413539 | 16.183057 | 10.740687 | 5.525342 | 11.732130 | 12.210327 | 25.218608 | 7.009826 | 9.060094 |



###  Cluster Categorization

### Insights on Choosing 5 Clusters

The decision to categorize the customer segments into **5 clusters** instead of **4** stems from the desire for a more nuanced understanding of customer behavior. Here are the key insights supporting this choice:

1. **Diverse Spending Behavior**: The additional cluster allows for a clearer distinction between **Medium Tickets** and **Beginners**. Without this separation, valuable insights about customers who are moderately active and those who are just starting could be lost.

2. **Identification of Risk**: By creating a dedicated **Risk** category, we can better identify customers who have minimal engagement. This highlights the need for targeted strategies to re-engage these customers, which may not be as effective if they were grouped with more active segments.

3. **Enhanced Marketing Strategies**: The extra cluster enables more tailored marketing approaches for each group. For example, understanding the specific needs of **Rare Purchasers** versus **Beginners** allows for customized campaigns that can improve customer retention and sales.

4. **Actionable Insights**: More clusters lead to actionable insights. For example, distinguishing between **Big Tickets** and **Medium Tickets** helps in strategizing promotions and loyalty programs specifically aimed at high-value customers.

5. **Data-Driven Decisions**: Finally, using five clusters is supported by the data characteristics and distributions observed in the analysis. It provides a more granular view of customer segments, aligning with best practices in customer segmentation methodologies.

### Cluster Behavioral Profiling and Strategic Recommendations

By defining five distinct clusters, we gain a detailed view of customer behaviors, enabling targeted marketing strategies for each segment.

---

### Key Characteristics and Targeted Marketing Strategies

#### **1. Big Tickets**
- **Characteristics**: Customers in this group show high purchase frequency and substantial spending, indicating a loyal and high-value customer base.
- **Relevant KM5 Features**: High values in **PURCHASES**, **PURCHASES_FREQUENCY**, and **CREDIT_LIMIT**.

- **Marketing Strategy**: 
  - **Loyalty Programs**: Implement exclusive rewards or tiered loyalty programs to encourage further engagement.
  - **Personalized Recommendations**: Offer premium products or tailored bundles, as this group is likely to respond to high-value offerings.
  - **VIP Experiences**: Consider VIP access or early product releases to solidify brand loyalty among these high-value customers.

---

#### **2. Medium Tickets**
- **Characteristics**: Moderate spenders with a tendency toward installment purchases; these customers frequently engage but at lower amounts than Big Tickets.
- **Relevant KM5 Features**: Moderate to high values in **INSTALLMENTS_PURCHASES** and **PURCHASES_FREQUENCY**.

- **Marketing Strategy**: 
  - **Installment-Based Discounts**: Offer installment purchase options with reduced interest or extra benefits to appeal to their purchase patterns.
  - **Flexible Payment Plans**: Encourage higher spending through limited-time, flexible payment options that increase accessibility.
  - **Engagement Campaigns**: Run frequent, small-scale promotions to maintain regular engagement and increase purchase frequency.

---

#### **3. Rare Purchasers**
- **Characteristics**: Tend to make infrequent but high-value one-off purchases, reflecting a selective but valuable spending behavior.
- **Relevant KM5 Features**: High values in **ONEOFF_PURCHASES** and low purchase frequency overall.

- **Marketing Strategy**: 
  - **Seasonal Promotions**: Use timed sales events to entice these customers to purchase.
  - **Exclusive Product Launches**: Highlight one-off, high-value items or limited-edition products that align with their selective purchasing style.
  - **Reminders and Re-Engagement**: Utilize targeted reminders, such as abandoned cart emails or "missed you" messages, to prompt renewed interaction.

---

#### **4. Beginners**
- **Characteristics**: Show low to moderate engagement, suggesting they are new customers or in the early stages of brand loyalty.
- **Relevant KM5 Features**: Low values across most metrics, with possible moderate engagement in **CASH_ADVANCE** or minimal purchases.

- **Marketing Strategy**: 
  - **Onboarding Campaigns**: Provide onboarding incentives, such as welcome discounts, to help convert them into regular customers.
  - **Educational Content**: Share product benefits, reviews, or usage tips to build familiarity and trust with the brand.
  - **Encourage First-Time Purchases**: Offer discounts on first-time purchases to encourage further engagement and solidify their customer journey.

---

#### **5. Risk**
- **Characteristics**: Very low purchasing frequency and low amounts, with minimal engagement across purchase metrics.
- **Relevant KM5 Features**: Low values across **PURCHASES**, **PURCHASES_FREQUENCY**, **CASH_ADVANCE**, etc.

- **Marketing Strategy**: 
  - **Re-Engagement Campaigns**: Use targeted ads or personalized emails with enticing offers to draw these customers back.
  - **Dormancy Incentives**: Provide substantial discounts or perks to encourage reactivation.
  - **Feedback Collection**: Gather insights on why they’re disengaged, which can guide adjustments in product or service offerings.

---

### Summary

By segmenting customers into these five distinct groups, the marketing team can employ tailored strategies that better align with customer preferences and behaviors, ultimately boosting customer retention, engagement, and revenue growth.
