<img src="https://user-images.githubusercontent.com/41976165/94978740-c2af6680-04ec-11eb-8e63-b98fcdf3384b.png" alt="drawing" width="400"/>

### Introduction/Background

The United States has seen an unsustainable increase in the cost of health care in recent
decades. The accessibility and affordability of health care has been at the forefront of issues that
Americans face on a daily basis. High costs often prevents people from seeking
proper treatment, and medical bills can be a major financial burden on U.S. families,
especially those that are in lower-income households. Our goal is to offer accurate
predictions of health insurance costs so that U.S. families are able to choose the right
insurance and make informed financial decisions.

#### Dataset
Our [data set](https://www.kaggle.com/mirichoi0218/insurance) is a CSV file from Kaggle with 1338 rows and contains a mix of text and numerical data. Specifically, it includes the following features:
- Age (integer)
- Sex (male/female)
- BMI (decimal)
- Number of children (integer)
- Smoker (yes/no)
- Region in the US (southeast/soutwest/northeast/northwest)
- Charges (decimal)


### Methods

Since our problem involves the prediction of a numerical or continuous value, we will be utilizing regression methods to solve it.  Before clustering our data and using it to make predictions, we will ensure that the data is clean and usable by removing any rows with invalid values and one-hot encoding certain features like sex to make them easier to work with.  After our data is clean and usable, we will construct a correlation matrix and explore the relationship between features that are highly correlated.

For the unsupervised learning portion of our project, we will be using K-means clustering to identify any unknown or unclear groups within our dataset.  To ensure that we choose the correct number of clusters to best represent the data, we will make use of the elbow method along with a silhouette analysis.  By clustering the data, we hope to find feature relationships that were not apparent during our statistical analysis of the dataset.

For the supervised learning portion of our project, we will be attempting to obtain better performance than Linear Regression when predicting insurance costs since it is the most common approach.  The models we have chosen to utilize are Random Forest, XGBoost, and ANN.  While we believe that our chosen models will lead to better performance than Linear Regression for the total dataset, we will also be training our models with low-to-medium and high cost data separately.  The reason for this is we would like to see if we are able to make better predictions for specifically low-to-medium cost and specifically high cost insurance.  To compare the performance of our models, we will be using the mean squared error (MSE) metric to evaluate each individually.

### Results
For our unsupervised portion, we are anticipating seeing some relationships between our features appear that could tell us more information about our data from running K-means on our dataset. From the unsupervised K-mean algorithm, we are tyring to achieve a silhouette coefficient >.50 and 90% clustering accuracy. We plan to have the unsupervised portion done by touch-point 2, where we will go into more detail about how we cleaned our data, clustered our data, and the results we found. By touch-point 3, we will have made headway with our supervised portion of the assignment. After training our supervised models with low-to-medium cost, high cost, and the combined set, we believe that Random Forest, XGBoost, or ANN to perform better than Linear Regression. From our supervised algorithms, we are trying to achieve a lower MSE from one of our Random Forest, XGBoost, and ANN algorithms in comparison to the Linear Regression algorithm. Furthermore, we want to get a lower MSE when using the separated datasets. We want to predict with an accuracy >90%.

### Discussion

The best outcome would be predicting medical insurance accurately each time by inputting their features. It would mean that medical insurance buyers would be able to predict their medical insurance and how the cost would increase or decrease for them as factors influencing insurance changes (i.e. age). Predicting insurance cost will allow the government and people decide what actions to take or policies to implement to help reduce medical insurance cost and increase coverage and know how much to budget for medical insurance. A potential next step, or future research, would be predicting how medical insurance changes over time. 

### References
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5977561/
- https://people.csail.mit.edu/gjw/papers/healthcare.pdf
- https://bmchealthservres.biomedcentral.com/articles/10.1186/s12913-019-4616-x

