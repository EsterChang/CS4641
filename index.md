<img src="https://user-images.githubusercontent.com/41976165/94980506-6d795200-04f8-11eb-885b-65e707be2036.png" alt="drawing" width="400"/>

## Overview

The United States has seen an unsustainable increase in the cost of health care in recent
decades. Many Americans are now afraid of seeking treatment, and those that do are often
left with unexpectedly high medical bills that become a major financial burden.

Our goal for this project is to offer quick and accurate predictions of health insurance costs
for an individual without reliance on his or her previous medical expenses. We hope that these
predictions will help individuals choose appropriate health insurance plans and make more
informed financial decisions.

To accomplish this task, we trained multiple regression models with the dataset below and evaluated
their performance by comparing each R2 Score and MSE. We repeated this process for both high costs only and
low-to-medium costs only in an attempt to make better predictions for individuals who are likely to belong in
one of these categories. Through our exploratory data analysis and unsupervised learning, we identified
the factors that would make an individual more likely to experience high costs, and a dollar amount to split
the dataset on to distinguish between high and low-to-medium costs.

### Dataset
Our [dataset](https://www.kaggle.com/mirichoi0218/insurance) is from Kaggle and contains a mix of numerical and categorical data. The features include:
- Age: integer
- Sex: male or female
- BMI: decimal
- Number of children: integer
- Smoker: yes or no
- Region (US): southeast, southwest, northeast, or northwest
- Charges: decimal (ground truth)

## Data Cleaning

Before working with our dataset, we wanted to ensure that it was thoroughly cleaned and usable so that it would not affect the results of our unsupervised or supervised learning.  The steps we took to clean our data were:
- Removing rows with missing values
- Removing duplicate rows
- Removing rows with bad values (ex: charges less than zero)

In addition to these steps, we also performed a one-hot encoding on the data, converting features such as smoker from "yes" and "no" to 1 and 0.  One of our features, region, was encoded in two different ways for use in different parts of our project.  First, our four regions were encoded as 0, 1, 2, and 3 for use in data exploration.  However, this encoding would become a problem in our unsupervised learning, since when measuring euclidean distance, the regions encoded as 0 and 3 would be seen as further apart than 0 and 1, even though there is no true meaning for this larger difference between the regions.  Therefore, our second encoding had region split into four separate features, one for each region, so that no pair of regions would be seen as further apart than another.

## Data Exploration

To start our data exploration, we created a heat map to analyze the correlation between the different features and targets.  From this heat map we noticed that smoking was highly correlated with charges, and both bmi and age were moderately correlated with charges.  Since smoking, bmi, and age were the features most correlated with charges, we decided to look at these relationships more closely.

<img width="400" alt="correlation" src="https://user-images.githubusercontent.com/46691358/98428361-1cb4c600-206f-11eb-9f7e-45fc9045d0b9.png">

Smoking appears to have a significant impact on charges.  We found that smokers tend to have charges from about $20,000 - $40,000, while non-smokers have much lower charges from about $5000 - $10000.

<img width="400" alt="smoker" src="https://user-images.githubusercontent.com/46691358/98428471-95b41d80-206f-11eb-90fc-42a77309eefb.png">

When analyzing only BMI and charges, it appears that upon reaching an obese BMI, the population separates significantly. Some members of the population had charges that remained fairly consistent, while others saw charges double or triple.  After applying a hue to the plot indicating smoking status, it showed charges for smokers increase linearly with BMI, while non-smoker charges remained fairly consistent. This again shows the significant impact of smoking, which is only worsened by a higher BMI.

<img width="400" alt="bmi-scatter" src="https://user-images.githubusercontent.com/46691358/98428482-aebcce80-206f-11eb-9875-b633e7ef5a92.png">

Since poor BMI seems to lead to higher charges, we decided to further analyze this trend. Any member of the population with a BMI greater than 25 was labeled overweight, and others not overweight.  After comparing the two groups, it appears that most members of each group have charges around $8000. However, it is clear that those who are overweight are far more likely to suffer high charges compared to those who are not.

<img width="400" alt="bmi-violin" src="https://user-images.githubusercontent.com/46691358/98428490-bda38100-206f-11eb-901e-900253b2da28.png">

The last relationship we chose to look at was between age and charges. As expected, increased age generally leads to increased charges, but like bmi, there appears to be distinct groups with vastly different charges.  When applying the smoker hue once again, it can be seen that charges for both smokers and non-smokers increase with age, but smokers have much higher charges.

<img width="400" alt="age" src="https://user-images.githubusercontent.com/46691358/98428500-cd22ca00-206f-11eb-8a4f-74607d822914.png">

The results of this data exploration lead us to believe that smoking is by far the most important feature in determing insurance cost, with BMI and age also being helpful. We will be using unsupervised learning to confirm these conclusions in subsequent sections.

## Unsupervised Learning

### Principal Component Analysis

Despite already having a low number of features, we decided to use PCA on our data to see if we could further reduce the dimensionality. Before performing PCA, we scaled our dataset so that larger features would not affect the outcome. We then applied PCA to our dataset and plotted the principal components against the cumulative explained variance to determine how many would be needed to achieve our desired variance. The results indicated that we would need 8 out of 9 components to explain 99% of the total variance. Since this reduction from our regular dataset is extremely minor, we believe that it is not valuable to reduce the dimensionality.

<img width="400" alt="pca-variance" src="https://user-images.githubusercontent.com/46691358/98429880-7a4d1080-2077-11eb-8ee9-461c53cd6c9e.png">

### Clustering
#### KMeans
We decided to use KMeans as our primary clustering method. After scaling our data, we used the elbow method and silhouette coefficient to determine the optimal k to use when clustering. While we were unable to find an exact optimal k, we found that 15 allowed us to extract meaningful information from our data and achieve a silhouette score of 0.52. Also, after tuning the random_state parameter on sklearn's KMeans implementation, we found that a random_state of 20 achieved the best results for our data.

<img width="300" alt="kmeans-elbow" src="https://user-images.githubusercontent.com/46691358/98430674-3ceb8180-207d-11eb-9277-53b621d6c4ce.png">

<img width="300" alt="kmeans-sil" src="https://user-images.githubusercontent.com/46691358/98430682-496fda00-207d-11eb-8e8a-384eaa24f6ba.png">

#### KPrototype

Another method that we attempted to use was KPrototype clustering. Since we have a mix of numerical and categorical data, many traditional clustering algorithms are difficult to apply to our data.  In KPrototype clustering, the dissimiliarity between features is determined by comparing numerical and categorical data differently.  After specifying which features are categorical, the algorithm will cluster the data using, for example, euclidean distance for numerical features, and hamming distance for categorical features. By using this method, we were able to achieve slighly improved distortion values, but ultimately we found our KMeans clustering to be more useful for understanding our data.

<img width="300" alt="kproto-elbow" src="https://user-images.githubusercontent.com/46691358/98430691-58568c80-207d-11eb-9882-3af78e71128d.png">

#### Cluster Analysis

Even though we could not achieve great clustering on our dataset, we were still able to gain further insight into the importance of our features by using 15 clusters.  When observing the distribution of charges within each cluster, it is clear that some clusters have lower charges on average, while others have higher charges. The average charges of Clusters 0-7 were around $8,000 ± $6,000, and the average charges of Clusters 8-14 were around $30,000 ± $11,000. 

<img width="507" alt="Screen Shot 2020-11-06 at 11 10 25 PM" src="https://user-images.githubusercontent.com/32435018/98432345-4c71c700-208b-11eb-9b31-0477f329d17b.png">

When these same clusters are shown in terms of smoker status, the influence of smoking is again very apparent. Recall that a value of 1 indicates a smoker and 0 a non-smoker. It can be seen that the clusters with high charges contain exclusively smokers, while the clusters with low charges contain exclusively non-smokers. Similar to the charges, there were two main divisions in the 15 clusters for the smoker feature. Clusters 0-7 had exclusively non-smokers, and clusters 8-14 had exclusively smokers, delineated by 0 standard deviation. This is a remarkable result, and again shows us that smoking status should be weighted much more than any other feature.

A surprising finding was that the majority of clusters were strictly male or female. Cluster 9 is the only cluster with both males and females, with a slightly larger number of males. Another thing we found was that each cluster belongs to a specific region, and there are no clusters with patients from multiple regions. We will be double-checking the impact of sex and region on charges before moving on to supervised learning.

<img width="816" alt="Screen Shot 2020-11-06 at 11 04 36 PM" src="https://user-images.githubusercontent.com/32435018/98431571-8ab7b800-2084-11eb-8dce-542cfb163658.png">

The age, bmi, and children features were plotted against the clusters as well, but they all showed similar averages for each cluster of around 38 ± 14, 30 ± 5, and 1.1 ± 1.2. When plotting all the data points of each cluster against the features, the data points varied signficantly across the full range for almost all the clusters. Therefore, these features could not give as much information about representing each cluster.

## Supervised Learning

### Linear Regression

Brief explanation and why this model was chosen.  Brief discussion of results and why they were what they were.

### Random Forest

Brief explanation of model and why this model was chosen. Hyperparameter tuning.  Brief discussion of results and why they were what they were.

### XGBoost

Brief explanation and why this model was chosen. Hyperparameter tuning.  Brief discussion of results and why they were what they were.

### Artificial Neural Network

Brief explanation and why this model was chosen. Hyperparameter tuning.  Brief discussion of results and why they were what they were.

### Results

Metric overview and predictive ability of best models for all, high, and low cost data.

------ Remove the stuff below once finished with the stuff above ------

## Results
### Expected
For our unsupervised portion, we expect to gain a deeper understanding of the importance of each feature in our dataset. For touchpoint 2, we are trying to achieve a silhouette coefficient >0.5 using K-means. For the supervised portion, we expect to see an improvement in performance over Linear Regression when utilizing Random Forest, XGBoost, and ANN methods. By touchpoint 3, we are aiming to obtain a lower MSE value from Random Forest, XGBoost, or ANN than Linear Regression on either the combined or separated dataset. Overall, we want to be able to predict the insurance cost of anyone, given their specific features, with an accuracy >90%.

### Current
Before starting our unsupervised portion, we expected to gain further insight into our data and achieve a silhouette coefficent >0.5 for K-means. We achieved this goal, and also went further by using PCA and KPrototype. This information will be helpful for us when deciding how to split our data into high cost and low-to-medium cost, and then seeing whether supervised learning can be improved. It is clear that smokers suffer signficant charges, and other factors such as BMI, age, and possiblity sex can also have an impact.

## Next Steps
For the supervised learning portion of our project, we will be attempting to obtain better performance than Linear Regression when predicting insurance costs since it is the most common approach.  The models we have chosen to utilize are Random Forest, XGBoost, and ANN.  While we believe that our chosen models will lead to better performance than Linear Regression for the total dataset, we will also be training our models with low-to-medium and high cost data separately.  The reason for this is we would like to see if we are able to make better predictions for specifically low-to-medium cost and specifically high cost insurance.  To compare the performance of our models, we will be using the mean squared error (MSE) metric to evaluate each individually.

## Discussion

The best outcome would be being able to predict anyone's health insurance given specific features with high accuracy. This outcome would mean that patients could choose the correct insurance for them, and it would allow insurance companies to better allocate their resources and plan their business model. Predicting insurance cost with high accuracy would also allow the government and its citizens to decide on correct policies to help reduce cost and increase coverage. A potential next step would be predicting an individual's insurance cost over time.

## References
Bertsimas, D., Bjarnadóttir, M. V., Kane, M. A., Kryder, J. C., Pandey, R., Vempala, S., &amp; Wang, G. (2008). Algorithmic Prediction of Health-Care Costs. Operations Research, 56(6), 1382-1392. doi:10.1287/opre.1080.0619

Jödicke, A. M., Zellweger, U., Tomka, I. T., Neuer, T., Curkovic, I., Roos, M., . . . Egbring, M. (2019). Prediction of health care expenditure increase: How does pharmacotherapy contribute? BMC Health Services Research, 19(1). doi:10.1186/s12913-019-4616-x

Morid, M., Kawamoto, K., Ault, T., Dorius, J., &amp; Abdelrahman, S. (2018, April 16). Supervised Learning Methods for Predicting Healthcare Costs: Systematic Literature Review and Empirical Evaluation.
