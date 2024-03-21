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

Once I had looked at individual distributions, I moved on to examining some bi-variate relationships. In the plot below, I investigate the relationship between 


