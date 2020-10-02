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

We have found that the most common approach to predicting insurance cost is linear
regression. Some researchers have found that certain statistical methods such as linear
regression cause skewed distributions and spikes at zero in small to medium sample
sizes. In addition to this, it has been suggested that some model performances could be
improved by training high cost and low-to-medium cost data separately.

Before attempting to predict insurance cost, we will first use the K-means method to
identify any unknown groups within our data set. After completing this task, our
approach to this problem will be to train each of our supervised models using the total
data set, then only high cost data, and finally only low-to-medium cost data. Since linear
regression is so popular, we will be comparing this method to Random Forest, XGBoost, and ANN. 
We believe this approach will be successful since it is likely that certain
models will result in better performance for high cost and others for low-to-medium cost.
If this is not the case, we still believe that at least one of our 3 chosen models will have
better performance than linear regression for the total data set.

In order to evaluate how successful our algorithm is, we’ll probably want to take a look at
a few different evaluation metrics before deciding on one. Because different metrics can
tell different stories with regards to how well our model performs, it’s important to explore
different evaluation metrics to decide which best represents our algorithm. We definitely
want to be wary of metrics that could over exaggerate our performance or make our
algorithm’s performance out to be more optimistic than it really is. For now, we believe
that we’ll want to pay attention to our f-measure as this value will take into account both
precision and recall.

### Results

### Discussion

The best outcome would be predicting medical insurance accurately each time by inputting their 
features. It would mean that medical insurance buyers would be able to predict their medical 
insurance and how the cost would increase or decrease for them as factors influencing insurance 
changes (i.e. age). Predicting insurance cost will allow the government and people decide what 
actions to take or policies to implement to help reduce medical insurance cost and increase 
coverage and how much to budget for medical insurance. The next step, or future research, 
involves predicting how medical insurance changes over time. 

### References


