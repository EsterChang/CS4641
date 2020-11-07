<img src="https://user-images.githubusercontent.com/41976165/94980506-6d795200-04f8-11eb-885b-65e707be2036.png" alt="drawing" width="400"/>

## Introduction/Background

The United States has seen an unsustainable increase in the cost of health care in recent
decades. The accessibility and affordability of health care has been at the forefront of issues that
Americans face on a daily basis. High costs often prevents people from seeking
proper treatment, and medical bills can be a major financial burden on U.S. families,
especially those that are in lower-income households. Our goal is to offer accurate
predictions of health insurance costs so that U.S. families are able to choose the right
insurance and make informed financial decisions. Since our problem involves the prediction
of a continuous value, we will be utilizing regression methods to solve it.

### Dataset
Our [dataset](https://www.kaggle.com/mirichoi0218/insurance) originates from Kaggle and contains a mix of text and numerical data. The features include:
- Age: integer
- Sex: male or female
- BMI: decimal
- Number of children: integer
- Smoker: yes or no
- Region in the US: southeast, southwest, northeast, or northwest
- Charges: decimal (ground truth)

## Data Cleaning

Before working with our dataset, we wanted to ensure that it was thoroughly cleaned and usable so that it would not affect the results of our unsupervised or supervised processes.  The steps we took to clean our data were:
- Removing rows with missing values
- Removing duplicate rows
- Removing rows with bad values (ex: cost less than zero or smoker value not yes or no)

In addition to these actions, we also performed a one-hot encoding on the data, converting features such as smoker from "yes" and "no" to 1 and 0.  One of our features, region, was encoded in two different ways for use in different parts of our project.  First, each of our four regions were encoded as 0, 1, 2, 3, and this encoding was used for our data exploration, such as in the correlation matrix.  However, this encoding would present a problem in our unsupervised learning, since when measuring the euclidean distance, the regions encoded as 0 and 3 would be seen as further apart than 0 and 1, even though there is no true meaning for this increased distance between the regions.  Therefore, our second encoding consisted of splitting region into four separate features, one for each region, so that no pair of regions would be seen as further apart than another.

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

In addition to determining the number of principal components needed, we also visualized the first two and three components separately.  We split our dataset into high-cost and low-to-medium cost based on the median of the charges, and plotted this data in relation to the principal components. It can be seen that there is not much separation between the two groups in both 2D and 3D space, indicating that more principal components are likely necessary.

<img width="400" alt="pca2" src="https://user-images.githubusercontent.com/46691358/98429884-8507a580-2077-11eb-9246-3b2e9cf1f0dd.png">

<img width="400" alt="pca3" src="https://user-images.githubusercontent.com/46691358/98429891-93ee5800-2077-11eb-9fd0-e0a2c7f6d6ec.png">

### Clustering
#### KMeans
We decided to use KMeans as our clustering method. We took out the charges information from the dataset and only used the features dataset. We scaled our data using the scikit-learn preprocessing toolbox. The elbow method and silhouette method were the internal metrics we used to find optimal k. Through the elbow method, we saw the loss starting to flatten out, but we were unable to determine the exact optimal k.

<img width="300" alt="kmeans-elbow" src="https://user-images.githubusercontent.com/46691358/98430674-3ceb8180-207d-11eb-9277-53b621d6c4ce.png">

The silhouette method allowed us to clearly see what would be the optimal number of clusters, k. A silhouette score closer to 1 represents the data points are more well-matched to its own cluster. After running the silhouette method on our scaled data, we saw that the optimal k clusters is 15, with the highest silhouette score of 0.52.

<img width="300" alt="kmeans-sil" src="https://user-images.githubusercontent.com/46691358/98430682-496fda00-207d-11eb-8e8a-384eaa24f6ba.png">

We implemented the KMeans method from sklearn, assigning the number of clusters to 15. To get the same clustering results each time, we made the randomness of the initialization deterministic by defining the parameter random_state as 20. After running the KMeans algorithm, we decided to plot each cluster’s mean and standard deviation against each of the features to get a better understanding of how our data points were being clustered.

<img width="510" alt="Screen Shot 2020-11-06 at 11 11 39 PM" src="https://user-images.githubusercontent.com/32435018/98431670-6f997800-2085-11eb-8809-6897af7d240d.png">

Figure 1a showed the clusters being plotted against the charges. There were 2 main divisions within the charges for the 15 clusters, with about half of the clusters having a lower average charge and the other half having a higher average charge. The average charges of Clusters 0-7 were around $8,000 ± $6,000, and the average charges of Clusters 8-14 were around $30,000 ± $11,000. Figure 1b showed the clusters plotted against the smoker feature. Similar to the charges, there were two main divisions in the 15 clusters for the smoker feature. Clusters 0-7 had exclusively non-smokers, and clusters 8-14 had exclusively smokers, delineated by 0 standard deviation. The division was the exact same as the division of charges, showing that only smokers had the highest charges.

<img width="816" alt="Screen Shot 2020-11-06 at 11 04 36 PM" src="https://user-images.githubusercontent.com/32435018/98431571-8ab7b800-2084-11eb-8dce-542cfb163658.png">

Figure 2a, the plot shows that most of the clusters are uniquely male or female. Cluster 9 is the only cluster with both males and females, with a slightly larger number of males. Figure 2b shows that each cluster belongs uniquely to a specific region, and there are no clusters with patients from multiple regions. Therefore, the different combinations of smoker, sex, and region features can represent each cluster.

The age, bmi, and children features were plotted against the clusters as well, but they all showed similar averages for each cluster of around 38 ± 14, 30 ± 5, and 1.1 ± 1.2. When plotting all the data points of each cluster against the features, the data points very much varied across the full range for almost all the clusters. Therefore, these features could not give as much information about representing each cluster. 

Through KMeans clustering, we were able to see how the patients could be clustered and what that clustering showed in looking at the charges. This silhouette score reached what we were trying to achieve (silhouette score > 0.5). This method was useful in finding out how to divide the charges into lower and higher charges. Furthermore, clustering showed what had the most impact on charges, which was the smoker feature, matching the correlation matrix. Through clustering, we were able to notice a trend in our clusters, with only smokers being charged higher insurance costs. In an effort to reduce insurance charges, we suggest to not smoke because non-smokers clusters have lower charges distributions; by not smoking, patients are much more likely to have lower insurance costs. The clustering will be useful for our regression problem during the supervised learning to show us the possible range for the two divisions/distributions of charges.



#### KPrototype

Another method that we attempted to use was KPrototype clustering. Since we have a mix of numerical and categorical data, many tradional clustering algorithms are difficult to apply to our data.  In KPrototype clustering, the dissimiliarity between features is determined by comparing numerical and categorical data differently.  After specifying which features are categorical, the algorithm will cluster the data using, for example, euclidean distance for numerical features, and hamming distance for categorical features. By using this method, we were able to achieve slighly improved distortion values, but ultimately we found our KMeans clustering to be more useful for understanding our data.

<img width="300" alt="kproto-elbow" src="https://user-images.githubusercontent.com/46691358/98430691-58568c80-207d-11eb-9882-3af78e71128d.png">

#### Cluster Analysis

Even though we could not achieve great clustering on our dataset, we were still able to gain further insight into the importance of our features by using 14 clusters.  When observing the distribution of charges within each cluster, it is clear that some clusters have lower charges on average, while others have higher charges.

<img width="300" alt="cluster-charges" src="https://user-images.githubusercontent.com/46691358/98430718-7ae8a580-207d-11eb-9050-1a407936e1cd.png">

When these same clusters are shown in terms of smoker status, the influence of smoking is again very apparent. Recall that a value of 1 indicates a smoker and 0 a non-smoker. It can be seen that the clusters with high charges contain exclusively smokers, while the clusters with low charges contain exclusively non-smokers. This is a remarkable result, and again shows us that smoking status should be weighted much more than any other feature.

<img width="300" alt="cluster-smoker" src="https://user-images.githubusercontent.com/46691358/98430727-863bd100-207d-11eb-884b-a75af0fb3e29.png">

## Results
### Expected
For our unsupervised portion, we expect to gain a deeper understanding of the importance of each feature in our dataset. For touchpoint 2, we are trying to achieve a silhouette coefficient >0.5 using K-means. For the supervised portion, we expect to see an improvement in performance over Linear Regression when utilizing Random Forest, XGBoost, and ANN methods. By touchpoint 3, we are aiming to obtain a lower MSE value from Random Forest, XGBoost, or ANN than Linear Regression on either the combined or separated dataset. Overall, we want to be able to predict the insurance cost of anyone, given their specific features, with an accuracy >90%.

### Current

## Next Steps
For the supervised learning portion of our project, we will be attempting to obtain better performance than Linear Regression when predicting insurance costs since it is the most common approach.  The models we have chosen to utilize are Random Forest, XGBoost, and ANN.  While we believe that our chosen models will lead to better performance than Linear Regression for the total dataset, we will also be training our models with low-to-medium and high cost data separately.  The reason for this is we would like to see if we are able to make better predictions for specifically low-to-medium cost and specifically high cost insurance.  To compare the performance of our models, we will be using the mean squared error (MSE) metric to evaluate each individually.

## Discussion

The best outcome would be being able to predict anyone's health insurance given specific features with high accuracy. This outcome would mean that patients could choose the correct insurance for them, and it would allow insurance companies to better allocate their resources and plan their business model. Predicting insurance cost with high accuracy would also allow the government and its citizens to decide on correct policies to help reduce cost and increase coverage. A potential next step would be predicting an individual's insurance cost over time.

## References
Bertsimas, D., Bjarnadóttir, M. V., Kane, M. A., Kryder, J. C., Pandey, R., Vempala, S., &amp; Wang, G. (2008). Algorithmic Prediction of Health-Care Costs. Operations Research, 56(6), 1382-1392. doi:10.1287/opre.1080.0619

Jödicke, A. M., Zellweger, U., Tomka, I. T., Neuer, T., Curkovic, I., Roos, M., . . . Egbring, M. (2019). Prediction of health care expenditure increase: How does pharmacotherapy contribute? BMC Health Services Research, 19(1). doi:10.1186/s12913-019-4616-x

Morid, M., Kawamoto, K., Ault, T., Dorius, J., &amp; Abdelrahman, S. (2018, April 16). Supervised Learning Methods for Predicting Healthcare Costs: Systematic Literature Review and Empirical Evaluation.
