# Analysis of Recipes
University of Michigan EECS398 Portfolio Analysis

# Introduction
The data sets that I will be analyzing are the recipes and ratings data sets. These consist of two unique CSV files describing recipes and what certain users thought of the recipes (their reviews). The interactions CSV has 731,927 rows and the recipes CSV has 83,782 rows. The reasoning for such a vast difference is that one recipe can have many reviews from many different users. The appeal of my investigation into these two data sets is that I am seeking out to determine the relationship of what makes a recipe successful (highly rated). Thus, I will be investigating: What common factors result in a recipe being highly rated. There are many columns storing information to work with and throughout the investigation I will look into the ‘nutrition’ column which stores nutrition information, ‘minutes’ which stores the amount of time the recipe takes, ‘n_steps’ which houses the amount of steps in a respective recipe, ‘n_ingredients’ which holds the integer value of the amount of ingredients in a recipe, and of course ‘rating’ which holds the rating given to the recipe on a one to five scale (with five being the highest rating). With that information in mind, let's get right into it!

# Data Cleaning and Exploratory Data Analysis
Getting right too it, we first must clean our data. Starting with the data frame that contains the ratings: we first are going to want to eliminate duplicates in the case that they may occur. Next we are going to normalize our column names with the same naming convention and then finally verify that all ratings are given as an integer on a 1-5 scale (inclusive). Moving on to recipes, there will be much more work done here: first, we are going to want to address the empty values. The most frequent column that contained empty values turned out to be minutes. In order to fill the rows missing 'minutes' (time the recipe takes) we are going to fill in with the median time. The reason the medain was utilzied instead of the mean was due to the presence of many outliers with extremely large cooking times (over 4k minutes, WOAH). The mean gives these outliers much more influence than the median does, so we will work with the mean. For descriptions that are missing, we will fill in simply NA, as we can not draw any conclusions. Next, we will normalize the column names the same way we did for the previous data frames. Finally, we are going to take out recipes that take over two weeks (or exaclty 20160 minutes). This will get rid of some large outliers that are not crucial to addressing our major question, however becuase there are not many recipes with that large of cooking time, we will take out those few outlying recipes. Below, you can see the first few rows of each cleaned data frame!

Here is the Ratings DataFrame cleaned:
<iframe
src = "keys/cleaned_interact_table.html"
width = "900"
height = "700"
frameborder = "0"
></iframe>
This displays the first few rows, containing the recipe ID, the rating given, and other identifying details.

Here is the cleaned recipes DataFrame:
<iframe
src = "keys/recipes.html"
width = "900"
height = "700"
frameborder = "0"
></iframe>

### Univariate Analysis
Listed below is a univariate analysis plot that describes the total amount of each rating, as in the total amount of reviews for each level rating. What clearly stands out the most is that there are drastically more five star reviews than any other review. This can likely be explained how people would be more likely to take the time to actually review someting (in this case a recipe) in the event that they really enjoy it.
<iframe
 src= "keys/p2.html"
 width = "800"
 height = "600"
 frameborder = "0"
></iframe>

### Bivariate Analysis
Next up we have an intresting bivariate analysis. This plot describes the the average amount of sugar in each grouped recipe rating. So, for example, if we were to take all of the recipes that recieved a 5/5 rating and found the average grams of sugar (in the nutrition column) of all these recipes and put it on a graph, we would create that column all the way to the right. Do this for the other three ratings and you have the graph you see now! An intresting trend is that the lowest rated recipes (1/5) tend to have the highest average sugar. It is also important to address that all recipes with over 200 g of sugar have been discarded for this graphical representation, in order to eliminate the sway of recipes with copius amounts of sugar.
<iframe
src = "keys/p5.html"
width = "900"
height = "700"
frameborder = "0"
></iframe>

### Aggregate Analysis
Our third graph is actually an aggregate graph!
<iframe
src = "keys/p10.html"
width = "900"
height = "700"
frameborder = "0"
></iframe>
The significane of this graph is that it can greatly help us understand what it is that makes recipes great! First off, take a look at the point located in the top right corner. That point represents all the recipes that recieved 1/5 ratings. The avearge time of these recipes is very high, at just under 70 minutes, much higher than any of the other data points representing the other rated recipes averages. The 1/5 recipes also have a high calorie total on average, once again higher than all the others. Meanwhile, as you decrease almost linearly in time and total calories, we get the 2/5 and then the 3/5 and then the 4/5. The 5/5 recipes are right there with 4/5 averages so the relationship is not perfectly linear, but this plot heavily suggests that as time and total calories increase, users are more likely to leave a lower review. Another interpretation is that users enjoy recipes that take little time and are low calorie, which logically makes sense as convienet low cal recipes are enjoyable to many.

## Prediction Question
A question that I am attempting to predict would be what are the total amount of calories in a recipe? I would know things such as cooking time, number of ingredients, amount of sugar, carbs, and saturated fats. Now, for the information that is found in the nutrition column, it is important to note that it is given in PDV, so I will convert this into total sugar, total carbs, etc. This information is all known before we make our prediction and we can safely base our prediction on these factors.
This type of prediction would be a regression problem as I am directly attempting to predict a numeric value, the total amount of calories.

# Baseline Model
Since we roughly established that low calorie recipes are, in general, more popular than high calories ones being able to predict how many calories a recipe is going to have would be an extremely useful model. 
For a baseline model, three features were utilized from the orignal data frames. Minutes (quantitive), n_ingredients (# of ingredients; quantitive), and rating (ordinal, but still uses numbers). There was no encoding done, although rating is ordinal, becuase we are working with linear regression encoding was not explicity needed. With these three features, the model perfomed relatively poorly. The test set R^2 score was only 0.02 and the MSE was very high at 290,161.90. This suggests that the model does not map the variance effecively at all. The training sets and test sets both have a high MSE, so we are severly underfiting the test and training data alike. Much to be improved

# Final Model
Thankfully, the final model drastically improved compared to the baseline version. The model was improved by adding additional features: sugar, total_fat, protein, and carbohydrates, which directly relate to the calorie content of recipes, so these features are much more likely to relate to total calories. A GridSearchCV was used to tune hyperparameters for the Lasso regression model, exploring options for regularization strength (alpha), maximum iterations (max_iter), and tolerance (tol). As previously mentioned, Lasso Regression was utilized to shrink the lesser features coeffiecents to smaller numbers, allowing for the prevelent features to have more infulence and limit over fitting. The best hyperparameters identified were alpha=0.0001, max_iter=1000, and tol=0.001, resulting in the final model perfoming very well: Test Set MSE: 1836.07 and R²: 0.99. This is a substantial improvement over the baseline model, which had an MSE of 290,161.90 and an R² of 0.02. By adding the new, relevent features, we experienced drastic improvment in the prediction model. 







