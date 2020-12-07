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
low-to-medium costs only in an attempt to make better predictions for individuals who are highly likely to belong in
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

To start our data exploration, we created a heat map to analyze the correlation between the different features and the target.  From this heat map we noticed that smoking was highly correlated with charges, and both bmi and age were moderately correlated with charges.  Since smoking, bmi, and age were the features most correlated with charges, we decided to look at these relationships more closely.

<img width="400" alt="correlation" src="https://user-images.githubusercontent.com/46691358/98428361-1cb4c600-206f-11eb-9f7e-45fc9045d0b9.png">

Smoking appears to have a significant impact on charges.  We found that smokers tend to have charges from about $20,000 - $40,000, while non-smokers have much lower charges from about $5000 - $10000.

<img width="400" alt="smoker" src="https://user-images.githubusercontent.com/46691358/98428471-95b41d80-206f-11eb-90fc-42a77309eefb.png">

When analyzing BMI and charges, it appears that upon reaching a BMI greater than 30, the population separates significantly. After this point it appears that some members of the population have charges that are double or triple that of others.  After applying a hue to the plot indicating smoking status, we can see that charges for smokers increase linearly with BMI, while non-smoker charges remain fairly constant as BMI increases. This again shows the significant impact of smoking, which is worsened by a higher BMI.

<img width="400" alt="bmi-scatter" src="https://user-images.githubusercontent.com/46691358/98428482-aebcce80-206f-11eb-9875-b633e7ef5a92.png">

Since poor BMI seems to lead to higher charges, we decided to further analyze this trend. Any member of the population with a BMI greater than 25 was labeled overweight, and others not overweight.  After comparing the two groups, it appears that most members of each group have charges around $8000. However, it is clear that those who are overweight are more likely to suffer high charges compared to those who are not.

<img width="400" alt="bmi-violin" src="https://user-images.githubusercontent.com/46691358/98428490-bda38100-206f-11eb-901e-900253b2da28.png">

The last relationship we chose to look at was between age and charges. As expected, increased age generally leads to increased charges, but like bmi, there appears to be distinct groups in the population with vastly different charges.  When applying the smoker hue once again, it can be seen that charges for both smokers and non-smokers increase with age, but smokers have much higher charges overall.

<img width="400" alt="age" src="https://user-images.githubusercontent.com/46691358/98428500-cd22ca00-206f-11eb-8a4f-74607d822914.png">

The results of this data exploration lead us to believe that smoking is by far the most important feature in determing an individual's risk for high insurance costs, with age and BMI still playing a modest role. Since smoking has such strong influence, we believe the best split between high and low-to-medium cost should be defined by the separation between charges for smokers and non-smokers. We will confirm and expand upon this claim in our clustering results section.

## Unsupervised Learning

### Principal Component Analysis

Despite already having a low number of features, we decided to use PCA to see if we could further reduce the dimensionality. After performing PCA on our scaled features, we plotted the principal components against cumulative explained variance and found that we would need 8 out of 9 of these components to explain 99% of the total variance. Since this reduction from our existing dataset is very minor, we believe that it is not valuable to reduce the dimensionality.

<img width="400" alt="pca-variance" src="https://user-images.githubusercontent.com/46691358/98429880-7a4d1080-2077-11eb-8ee9-461c53cd6c9e.png">

### Clustering
#### KMeans

The first clustering method we utilized was KMeans. Since this method relies on euclidean distance to create clusters, we scaled all features to the range (0, 1) so that clustering was not dominated by our larger features. After running KMeans on our scaled features, it appeared difficult to find meaningful clusters.  However, by using the elbow method and silhouette coefficients, we decided that we would be able to gain valuable information from 15 clusters.

<img width="400" alt="kmeans-elbow" src="https://user-images.githubusercontent.com/46691358/98430674-3ceb8180-207d-11eb-9277-53b621d6c4ce.png"><img width="400" alt="kmeans-sil" src="https://user-images.githubusercontent.com/46691358/98430682-496fda00-207d-11eb-8e8a-384eaa24f6ba.png">

#### KPrototype

Another method that we attempted to use was KPrototype clustering. Since we have a mix of numerical and categorical data, many traditional clustering algorithms are difficult to apply to our data.  In KPrototype clustering, similiarity is determined by comparing numerical and categorical data differently.  After specifying which features are categorical, the algorithm will cluster the data using, for example, euclidean distance for numerical features and hamming distance for categorical features. By using this method we were able to achieve slighly improved distortion values, but ultimately we found our KMeans clustering to be more useful for understanding our data and accomplishing our task.

<img width="400" alt="kproto-elbow" src="https://user-images.githubusercontent.com/46691358/98430691-58568c80-207d-11eb-9882-3af78e71128d.png">

### Results

As stated previously, even though it was difficult to find meaningful clusters, we were able to gain valuable information from using KMeans with 15 clusters that confirmed our earlier data exploration findings. When observing the average and standard deviation of the charges in each cluster, it is clear that some clusters primarily have much lower charges than others.  To investigate this phenomenon, we looked at the average and standard deviation of each feature for each cluster. Only one feature offered a clear and meaningful explanation for the difference of charges between clusters: smoking. Every cluster that had signficantly higher average charges consisted of only smokers. This evidence confirms the earlier claim made in the data exploration section that smoking is by far the most important feature in determing an individual's risk for high insurance costs. These results coupled with our earlier findings from data exploration lead us to believe that $15000 is the optimal split between high and low-to-medium cost insurance, as this represents a split between the charges for smokers and non-smokers.

<img width="400" alt="Screen Shot 2020-12-07 at 2 59 04 AM" src="https://user-images.githubusercontent.com/46691358/101324475-52002f80-3838-11eb-80ed-b18f594e1c1b.png"><img width="400" alt="Screen Shot 2020-12-07 at 2 59 39 AM" src="https://user-images.githubusercontent.com/46691358/101324539-680df000-3838-11eb-95c4-a54e66b5d7fc.png">

## Supervised Learning

To make the best possible predictions, we compared the performance of four different regression models on combined, high, and low-to-medium cost data individually. By also training each of our models with high and low-to-medium cost data only, we hoped to achieve more accurate predictions for individuals highly likely to have low-to-medium or high costs, which we now know to be smokers and potentially those very overweight.

After creating our high and low-to-medium cost datasets based on our $15000 threshold, we created a training and testing set for each, as well as for the combined dataset.  Each training set consisted of 80% of the data in a set, while each testing set consisted of the remaining 20%. All data in both training and testing sets was scaled so that evaluation metrics were interpretable, and the performance of scaled sets was compared against unscaled sets to ensure that performance was unaffected.

### Linear Regression

Linear Regression is among the most common approaches we have seen others use to predict insurance costs. Since this method is so common, we decided to use it as a starting point to compare to our subsequent models. This method did not require any hyperparameter tuning.

### Random Forest



Brief explanation of model and why this model was chosen. Hyperparameter tuning.  Brief discussion of results and why they were what they were.

### XGBoost

Brief explanation and why this model was chosen. Hyperparameter tuning.  Brief discussion of results and why they were what they were.

### Artificial Neural Network

The last model we chose to evaluate was ANN. After some research, we learned that ANN has been able to make accurate predictions for high cost insurance in the past, so we chose it to try to improve our own high cost predictions. As an implementation, we used Scikit-learn's MLPRegressor. Since our dataset is not very large, we used the lbfgs solver for weight optimization, as this solver is known to help smaller datasets converge faster and perform better. We also chose to use one hidden layer since we discovered that only one was necessary for most other problems of similar scale and complexity. To optimize the remaining parameters, we again made use of RandomizedSearch with 300 iterations. These parameters included the maximum number of iterations, size of our hidden layer, activation function, and alpha value.

The ranges for each of these parameters were set as follows:
- Maximum iterations: 500 - 100 (increments of 100)
  - Values in this range allowed the model to converge most of the time
- Size of hidden layer: 2 - 7 (increments of 1)
  - Size of the hidden layer should be between the size of the input and output layer
- Activation function: tanh or relu
- Alpha: 0.0001 - 0.0009 (increments of 0.0001)

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
