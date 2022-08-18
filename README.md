# MachineLearning
# What drives the price of a car?
**OVERVIEW**

In this application, you will explore a dataset from kaggle that contains information on 3 million used cars.  Your goal is to understand what factors make a car more or less expensive.  As a result of your analysis, you should provide clear recommendations to your client -- a used car dealership -- as to what consumers value in a used car.

### CRISP-DM Framework

<center>
    <img src = images/crisp.png width = 50%/>
</center>


To frame the task, throughout our practical applications we will refer back to a standard process in industry for data projects called CRISP-DM.  This process provides a framework for working through a data problem. 

### Business Understanding

From a business perspective, we are tasked with identifying key drivers for used car prices.  In the CRISP-DM overview, we are asked to convert this business framing to a data problem definition.  Using a few sentences, reframe the task as a data task with the appropriate technical vocabulary. 

- The goal of this project is to understand the history of sales for used cars and predict the pricing of cars.
- The data will need to be defined, manipulated and cleaned to start building the model.
- The model should deliver predict the future consumer pricing.

### Data Understanding

After considering the business understanding, we want to get familiar with our data.  Write down some steps that you would take to get to know the dataset and identify any quality issues within.  Take time to get to know the dataset and explore what information it contains and how this could be used to inform your business understanding.
![image](https://user-images.githubusercontent.com/36009738/185293330-ad0aab3d-2403-43c3-aab9-55905f8d60ab.png)

#### A few notes about the data
There aren't a lot of data for hybrid cars, new & fair conditions, 3 5 and 10 cylinder cars.
The title column won't tell us too much about the car since most of the data is in clean.
Most cars are in automatic transmission.

### Data Preparation

After our initial exploration and fine tuning of the business understanding, it is time to construct our final dataset prior to modeling.  Here, we want to make sure to handle any integrity issues and cleaning, the engineering of new features, any transformations that we believe should happen (scaling, logarithms, normalization, etc.), and general preparation for modeling with `sklearn`. 

### Notes about the data

- I will remove ID and VIN number from the dataset because it is not necessary for modeling. Also, most of the size column is missing so that will likely be dropped as well. I will also drop model column, there are too many unique values (29650) to make a concrete observation.
- Remove null values
- There are no duplicate entries
- There are currently 4 numeric features and I intend to change year and odometer to integers. 
- Remove outliers. As seen from the histograms the price, odometer and years have a very large range

Z-score is a method for removing outliers (also called standard score). This value helps to understand how far the data point from the mean. 

Z-score=(data_point - mean)/(Std. deviation)

However, in this case I want to use IQR (Inter Quartile Range). 
IQR= Quartile3-Quartile1

### Modeling

With your (almost?) final dataset in hand, it is now time to build some models.  Here, you should build a number of different regression models with the price as the target.  In building your models, you should explore different parameters and be sure to cross-validate your findings.
Exploring a number of different models, let's start with linear regression


### Model just the numerical data 
The purpose of this is to explore building models with numerical data only. 
Steps:
1. Set up a pipeline using the Pipeline object from sklearn.pipeline

Pipelines are used to automate a machine learning workflow. The pipeline can involve pre-processing, feature selection, classification/ regression and post-processing.

2. Perform a grid search for the best parameters using GridSearchCV()

Optimization tunes the model for the best performance. The success of any learning model rests on the selection of the best parameters that give the best possible results. 

3. Analyze the results and visualize
Scaler: For pre-processing data, i.e., transform the data to zero mean and unit variance using the StandardScaler().
![image](https://user-images.githubusercontent.com/36009738/185293669-bf8ff540-0785-4ac5-9bdc-8a7ca347b2a5.png)

Training score: how the model is fitted in the training data. 

Test score: The higher the score, the better the model is generalized. 

Very low training score and low test score is under-fitting. Which is expected here as we only use year and odometer to fit the data. This model was just generated for practice and representation.

As we can see from the graph it does not fit well.
### Now let's try categorical modeling
Target encoding replaces a categorical feature with average target value of all data points belonging to the category. 
To use a Pipeline in a GridSearchCV you want to preface the value in your parameter dictionary with an all lowercase version of the object. For example, to search over a ridge estimators alpha value we will create a pipeline with names scaler and ridge to use the StandardScaler followed by the Ridge regressor. To search over the ridge objects alpha paramater we write ridge__alpha
![image](https://user-images.githubusercontent.com/36009738/185293770-c906ae28-0f46-44a7-992c-afb4f11d276d.png)

### Model 3

    Let's try Target Encoding with Sequential Selection with Lasso, Scaling and ridge. We will also add scoring in Grid Search CV
![image](https://user-images.githubusercontent.com/36009738/185293822-f843d0b7-86b3-42e8-a1a7-852cd9d1b51f.png)

### Evaluation

With some modeling accomplished, we aim to reflect on what we identify as a high quality model and what we are able to learn from this.  We should review our business objective and explore how well we can provide meaningful insight on drivers of used car prices.  Your goal now is to distill your findings and determine whether the earlier phases need revisitation and adjustment or if you have information of value to bring back to your client.
I have built 3 models.

The first was just for practice purposes using numerical data. As expected, the model performance was extremely low. With the following information:

    Test MSE: 118805993.95177384
    Best Alpha: 100.0
    Training set score: 0.11296720989770215
    Test set score: 0.12188239664954037
    R2: 12.19%

The second performed the best out of all 3 models. 

    Test MSE: 72032734.99578267
    Best Alpha: 100
    Training set score: 0.46374065904117034
    Test set score: 0.4657737480390959
    R2: 46.58%

The last performed similar to the second model, although slightly worse.

    Test MSE: 72108690.87875488
    Best Alpha: 1000.0
    Training set score: -6213.154450031672
    Test set score: 0.4659167625838996
    R2: 46.52%
    
These scores could be better, initially I was aiming for an 80% R2 score. Given more time I would go back to the data exploration phase and clean the data further. I would also try another model.

### Deployment

Now that we've settled on our models and findings, it is time to deliver the information to the client.  You should organize your work as a basic report that details your primary findings.  Keep in mind that your audience is a group of used car dealers interested in fine tuning their inventory.

We can develop an application for the client where they can assess a price of the car. Given the larger error, they can record the difference between the model prediction and sales price and we can further improve the accuracy of the model. 





