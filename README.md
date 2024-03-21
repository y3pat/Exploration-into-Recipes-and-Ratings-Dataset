# Exploration into Recipes and Ratings Dataset
Project 4 DSC80
By Yash Patel

## Introduction
In today's world, data science has become a major contributor to the success of many. Due to the gradual incline of data, the importance of what data can tell us has become such a necessity for companies to be successful. In this project, I show one of the many ways we can use data science to identify relationships and predict certain values based on these relationships.

The dataset being used for this analysis is from food.com and contains a recipes dataset, which contains numerous recipes on food.com, and an interactions dataset, which contains reviews from other people on these recipes. The columns are as follows for the recipes dataset: recipe name, recipe ID, minutes to prepare, contributor ID, submission date, tags, nutrition information, number of steps, steps text, and description. The columns in the interactions dataset are as follows: user ID, recipe ID, date of interaction, rating, and review text. 

The recipes dataframe has 83782 rows while the interactions dataframe has 731927 rows. Although there is a large number of columns, the ones relevant for this analysis are the minutes, submission date, tags, nutrition information, number of steps, and rating. The minutes column is the number of minutes required to prepare the recipe. Submission date represents the date on which the recipe was submitted. Tags are a column of tags that food.com had for the recipe. Nutrition information will actually be split up, but it is a string of a list which is in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value”. The number of steps is indicative of how many steps the recipe takes. Finally, rating is the rating given to the recipe. Some of these columns will be used to perform numerous analyses, while others will be used to answer the question: "Do people give lower ratings, on average, to recipes with higher total fat contents?"

## Data Cleaning and EDA
Before performing any analyses, the datasets must be merged and cleaned.

Firstly, I merged the datasets by renaming the 'id' column in recipes to 'recipe_id' and performing a left merge that kept all keys in recipes. Then, I looked at the data types of the data frame and cleaned them such that the data would be easier to handle for any future analyses. In doing so, I noticed that the 'submitted', 'date', and 'nutrition' columns needed to be changed.

I first converted both the 'submitted' and 'date' columns to DateTime objects such that certain values such as years, months, or days could be extracted from them if necessary. I then applied a function to the nutrition column that split the string of a list up into 7 columns. Each column represented a nutritional fact and was named the exact same way each element in the list is named in the introduction.

After cleaning these columns, I also saw that the 'user_id' column had to be changed. After the merge, it had been converted into a float in scientific notation so I filled any na value with 0.0 (since na values cannot be converted to int) and then changed the type to int. This fixed the issue and I replaced 0 with np.nan after to show that there was a null value.

Finally, I grouped the values by their recipe id and found the mean rating for each unique recipe. This was then merged into the dataset so that each unique recipeid would have its appropriate mean rating.

The head of part of the cleaned and merged data frame is shown below:

|     id | name                                 |   minutes | submitted           |   n_steps |   n_ingredients |    mean_rating |
|-------:|:-------------------------------------|----------:|:--------------------|----------:|----------------:|---------------:|
| 333281 | 1 brownies in the world    best ever |        40 | 2008-10-27 00:00:00 |        10 |               9 |            4.0 |
| 453467 | 1 in canada chocolate chip cookies   |        45 | 2011-04-11 00:00:00 |        12 |              11 |            5.0 |
| 306168 | 412 broccoli casserole               |        40 | 2008-05-30 00:00:00 |         6 |               9 |            5.0 |
| 286009 | millionaire pound cake               |       120 | 2008-02-12 00:00:00 |         7 |               7 |            5.0 |
| 475785 | 2000 meatloaf                        |        90 | 2012-03-06 00:00:00 |        17 |              13 |            5.0 |

...continued...

| calories (#) | total fat (PDV) | sugar (PDV) | sodium (PDV) |   protein (PDV) |   saturated fat (PDV) |   carbohydrates (PDV) |
|-------------:|:----------------|------------:|:-------------|----------------:|----------------------:|----------------------:|
|        138.4 |            10.0 |        50.0 |          3.0 |             3.0 |                  19.0 |                   6.0 |
|        595.1 |            46.0 |       211.0 |         22.0 |            13.0 |                  51.0 |                  26.0 |
|        194.8 |            20.0 |         6.0 |         32.0 |            22.0 |                  36.0 |                   3.0 |
|        194.8 |            20.0 |         6.0 |         32.0 |            22.0 |                  36.0 |                   3.0 |
|        194.8 |            20.0 |         6.0 |         32.0 |            22.0 |                  36.0 |                   3.0 |


### Univariate Analysis

The next thing I did was to look at the distribution of the n_steps column. 

<iframe src="assets/n_steps_dist.html" width=800 height=600 frameBorder=0></iframe>

As seen in the plot, the distribution of the number of steps follows a normal distribution. The number of steps for a recipe is generally around seven steps as seen in the plot.

### Bivariate Analysis

Once I had looked at individual distributions, I moved on to examining some bi-variate relationships. In the 2d Histogram below, I investigate the relationship between mean ratings and the number of steps.

<iframe src="assets/n_steps_mean_rating_fig.html" width=800 height=600 frameBorder=0></iframe>

Before trying a 2d histogram, I had tried using a scatter plot to show this relationship. However, the scatter showed no pattern at all. When I investigated deeper and used a 2d histogram I found trends that could be found in each rating. If you look at the trends based on how many total recipes are in that bin of n_steps, we notice that there is generally a higher proportion of recipes that have a higher number of steps that have a higher rating (in the 4.9 to 5.099 range). Recipes with less than 40 steps do not appear in the 4.9 to 5.099 range but those with more than 60 steps have the largest majority of their ratings within this high range.

### Interesting Aggregates

In this aggregate analysis I look into the mean and median of protein (PDV) and total fat (PDV) based on the number of steps the recipe takes to prepare. The first few rows are shown below:

|   ('n_steps', '') |   ('mean', 'protein (PDV)') |         ('mean', 'total fat (PDV)') |   ('median', 'protein (PDV)') |  ('median', 'total fat (PDV)') |
|------------------:|----------------------------:|------------------------------------:|------------------------------:|-------------------------------:|
|                 1 |                    11.75    |                               19.71 |                           4.0 |                            6.0 |
|                 2 |                       15.45 |                               20.52 |                           6.0 |                            7.0 |
|                 3 |                       17.13 |                               20.44 |                           7.0 |                           10.0 |
|                 4 |                      22.98  |                               24.57 |                          11.0 |                           14.0 |
|                 5 |                      27.43  |                               26.15 |                          13.0 |                           17.0 |


This pivot table shows that both mean protein and mean total fat, along with median protein and median total fat, increase as the number of steps initially increases. If we look at the whole dataframe however, the trend does not stay this way.

## Assessment of Missingness

### NMAR Analysis

Assessing the missingness of data is important as it allows us some insight into why a column might have null values. When data is NMAR (not missing at random) the fact that the data is missing is completely dependent on the column itself. For example, the description column in our dataframe is most likely NMAR. This is because there is a possibility that the contributor of the recipe did not feel that it was necessary to give the recipe a description and instead left it blank. 

### Missingness Dependency

Now that we have gone over a possible NMAR column, we can start looking into showing if a column is MAR or NMAR based on statistics. For the purpose of this section I will be investigating the randomness of the missingness of ratings.

My first set of hypotheses related the minutes it takes to prepare the recipe and the missingness of the rating. I thought that the rating might be missing if the recipe takes too long to prepare as not many people would want to prepare that recipe. In order to test this I came up with the following permutation test which shuffles the data 1000 times:

Null hypothesis: The distribution of minutes when rating is missing is the same as the distribution of minutes when rating is not missing
Alternate Hypothesis: The distribution of minutes when rating is missing is not the same as the distribution of minutes when rating is not missing
Observed Statistic: Absolute Difference of Means

<iframe src="assets/minute_rating_plot.html" width=800 height=600 frameBorder=0></iframe>

The result of the permutation test is shown below:

<iframe src="assets/minutes_rating.html" width=800 height=600 frameBorder=0></iframe>

As we can see from the permutation testing done on minutes vs ratings, we calculate a p-value of 0.1. With a significance level of 0.05, we fail to reject the null which means that ratings is possibly NMAR in this case.


After finding one scenario where the missingness of rating is not MAR, I looked into if the missingness of rating does depend on any other column. When doing this I came up with the following permutation test which shuffles the data 1000 times:

Null hypothesis: The distribution of total fat (PDV) when rating is missing is the same as the distribution of total fat (PDV) when rating is not missing
Alternate Hypothesis: The distribution of total fat (PDV) when rating is missing is not the same as the distribution of total fat (PDV) when rating is not missing
Observed Statistic: Absolute Difference of Means

<iframe src="assets/fat_rating_plot.html" width=800 height=600 frameBorder=0></iframe>

The result of the permutation test is shown below:

<iframe src="assets/totalfat_rating.html" width=800 height=600 frameBorder=0></iframe>

As we can see from the permutation testing done on total fat (PDV) vs ratings, we calculate a p-value of 0.0. With a significance level of 0.05, we reject the null which means that the ratings column is most likely MAR and dependent on the total fat (PDV) column.

## Hypothesis Testing

After seeing all of these distributions and tables, I wanted to finally answer the question on if people give lower ratings on average to recipes with higher total fat contents. Before performing the permutation test to see if we can reject the null, I came up with the null hypothesis, alternate hypothesis, and observation statistic.

Null Hypothesis: People give recipes with a total fat greater than 100% of your daily value the same average ratings as those with total fat content less than or equal to 100%
Alternate Hypothesis: People give recipes with a total fat greater than 100% of your daily value lower average ratings than those with total fat content less than or equal to 100%
Observation Statistic: Difference of Means

First, I created a new column in a df that contained the ratings and total fat (PDV) columns that identified if a food was high in fat. This was done by putting True if the food had >100.0 PDV and False otherwise. This allowed me to then group the df so that the indices were True or False, and found the mean ratings for each. I then did (mean rating of False) - (mean rating of True) which calculates how much higher the rating of foods with low fat is than the rating of foods with high fat. 

After performing a permutation test using this as the statistic, I found a p-value of 0.095. This means that we fail to reject the null with a significance of 0.05. This also means that the test is statistically insignificant.

## Framing a Prediction Problem

Another amazing part of datascience is that we can use feature engineering to transform columns in order to create accurate predictions on other columns. I chose to perform feature engineering to predict the rating column of the dataset. The reason I chose rating is because I feel like rating would have the closest connection with other features in the column. For example, if the people reviewing would enjoy healthy foods, then they would give foods with low fat and high protein higher ratings. By transforming values in other columns through the sklearn modules we can attempt to accurately predict the rating column. The metric I will be using to evaluate the model is root mean-squared-error. This metric would be better compared to accuracy and other metrics since it will allow for us to see if the rating predictions are very off. Since the ratings only go from 0.0 to 5.00, the rmse will allow us to know if our model tends to overestimate or underestimate. If we were to use something such as accuracy we would only know if our model generally predicted correctly, but not how distant the predictions are from the actual values.

## Baseline Model

My regression model uses a column transformer and a pipeline to change the 'submitted' and 'protein (PDV)' columns. The model first alters the submitted column so that it changes from an ordinal variable to a quantitative variable that it can work with by converting the values in the column to its year. It then converts the protein (PDV) column, which is a continuous variable, by using a quantiletransformer that replaces the protein value with whatever quantile it lies within, which is discrete since it can only take on a value between 0 and 100. Finally, it uses linear regression to predict the values in the rating column. 

The model ended up with an rmse of 0.7102. The model is okay at predicting the rating but it could still get much better. This rmse can be considered mediocre and means that the model gets relatively close to the actual value when predicting but is not able to accurately predict it.

## Final Model



