Requirements:
▪ Summary figure (one infographic prepared by your team that summarizes your project goal)

▪ Introduction/Background (Proposed): I would add a subheading about the dataset (the format, what each sample represents, your ground truth)

▪ Methods: is this a classification (binary/multiclass) or regression? Mention the data preprocessing and dataset combining techniques, unsupervised and supervised algorithms, and the evaluation metric you plan on using 

▪ Results (what results are you trying to achieve? ): mention what you expect from the unsupervised and supervised algorithms, what is the outcome for each touchpoint (eg. 90% accuracy to classify CVD patients)

▪ Discussion (best outcome, what it would mean, what is next.)

▪ References (list containing at least three references, preferably peer-reviewed)

### Summary Figure
![Summary Figure](https://user-images.githubusercontent.com/41976165/94748881-63b4ea80-0350-11eb-8ac1-789a9b24df41.png)

### Introduction/Background

Health care accessibility and affordability has been at the forefront of issues that
Americans face on a daily basis. Health care costs often prevent people from seeking
proper treatment, and medical bills can be a major financial burden on U.S. families,
especially those that are in lower-income households, do not have insurance, or have a
serious medical condition. We hope to determine which factors play a bigger role in
determining the final cost of insurance to provide patients with ways they can reduce
their overall costs by making changes to factors they can control.
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

For the supervised learning portion of our project, we will be attempting to obtain better performance than Linear Regression when predicting insurance costs since it is the most common approach.  The models we have chosen to utilize are Random Forest, XGBoost, and ANN.  While we believe that our chosen models will lead to better performance than Linear Regression for the total dataset, we will also be training our models with low-to-medium and high cost data separately.  The reason for this is we would like to see if we are able to make better predictions for specifically low-to-medium cost and specifically high cost insurance.  To compare the performance of our models, we will be using the mean squared error metric to evaluate each individually.

### Results
For our unsupervised portion, from running K-means on our dataset, we’re anticipating seeing some relationships between our features appear that could tell us more information about our data. We plan to have this portion done by touch-point 2 where we’ll go into more detail about how we cleaned our data, clustered our data and the results we found. By touch-point 3, we will have made headway with our supervised portion of the assignment. After training our supervised models with low-to-medium cost, high cost, and the combined set, we believe that Random Forest, XGBoost, or ANN to perform better than Linear Regression. We’re anticipating that one of the following will perform the best with an accuracy of greater than 90%.

### Discussion

The best outcome would be predicting medical insurance accurately each time by inputting their 
features. It would mean that medical insurance buyers would be able to predict their medical 
insurance and how the cost would increase or decrease for them as factors influencing insurance 
changes (i.e. age). Predicting insurance cost will allow the government and people decide what 
actions to take or policies to implement to help reduce medical insurance cost and increase 
coverage and how much to budget for medical insurance. The next step, or future research, 
involves predicting how medical insurance changes over time. 

### References


