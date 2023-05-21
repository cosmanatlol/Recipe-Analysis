<iframe src="assets/count_steps_recipes.html" width=800 height=600 frameBorder=0></iframe>
# Recipe-Analysis

## Analysis
This project will be dealing with a Recipe CSV file and Rating CSV file which can both be found on [food.com](food.com). These files have information about the rating of all recipes by individuals, nutritional information about recipes, and much more. This project deals with the following question: how does the number of steps in a recipe affect the nutrition of the recipe. In general the number of steps in a recipe generally correlates to the amount of actions done to ingredients (like boiling or deep frying) so this question can help answer how exactly the number of steps affects nutrition and whether or not cooking removes or adds nutrition to foods. In particular we will be analyzing carbohydrates, calories, sugar, total fat, saturated fat, sodium, and protein in which all but calories deal with percentage recommended daily intake. We will be dealing the n_steps (number of steps in a recipe) and nutrition as our column and there are totally 83,623 recipes to analyze.

## Cleaning
The Recipe CSV file contain all information about nutrition of recipes, number of steps, and ingredients and the Rating csv file has a bunch of review and a recipe id to connect each review with a recipe. We can merge these two csv files by the recipe id in which now each review also has more information about things like nutrition and ingredients. People must give ratings between 1 and 5 inclusive, however when we look at data frame we observe that there are reviews with rating 0, hence it is reasonable to assume that these are people that didint give a rating hence we can replace all of the 0s with Null value as that would represent no response and has no weight when dealing with things like average rating of recipes. And finally we have the nutrition column, in the initial file given all of the nutrition is given as a list so we must seperate it into each individual nutrition and add seperate columns for each of them. After dropping unnecessary columns we get the following dataframe:
<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th></th>      <th>n_steps</th>      <th>avg_rating</th>      <th>calories</th>      <th>total fat</th>      <th>sugar</th>      <th>sodium</th>      <th>protein</th>      <th>saturated fat</th>      <th>carbohydrates</th>      <th>n_ingredients</th>    </tr>  </thead>  <tbody>    <tr>      <th>0</th>      <td>15</td>      <td>4.750000</td>      <td>384.7</td>      <td>0.0</td>      <td>0.0</td>      <td>70.0</td>      <td>159.0</td>      <td>0.0</td>      <td>6.0</td>      <td>3</td>    </tr>    <tr>      <th>1</th>      <td>5</td>      <td>5.000000</td>      <td>304.1</td>      <td>13.0</td>      <td>121.0</td>      <td>3.0</td>      <td>13.0</td>      <td>9.0</td>      <td>19.0</td>      <td>3</td>    </tr>    <tr>      <th>2</th>      <td>5</td>      <td>4.777778</td>      <td>26.8</td>      <td>0.0</td>      <td>11.0</td>      <td>5.0</td>      <td>2.0</td>      <td>0.0</td>      <td>1.0</td>      <td>14</td>    </tr>    <tr>      <th>3</th>      <td>2</td>      <td>5.000000</td>      <td>40.7</td>      <td>0.0</td>      <td>17.0</td>      <td>2.0</td>      <td>4.0</td>      <td>0.0</td>      <td>2.0</td>      <td>11</td>    </tr>    <tr>      <th>4</th>      <td>4</td>      <td>5.000000</td>      <td>146.5</td>      <td>0.0</td>      <td>0.0</td>      <td>0.0</td>      <td>0.0</td>      <td>0.0</td>      <td>0.0</td>      <td>4</td>    </tr>  </tbody></table>

## Exploratory Data Analysis
To start, lets look at how exactly the distribution of steps looks like:


As we see it looks like the distribution of steps is centered around 9 and its appears to follow a normal distribution which is what we would expect with such a large sample size. Hence we will be able to conduct a hypothesis test centered around 9 to see nutritional affect in the future. Lets also look at the distribution of a specific nutrient type (fat):
<iframe src="/assets/fatper_recipe.html" width=800 height=600 frameBorder=0></iframe>
As we can see the fat appears to have a ton of 0 values, this would mean that not all recipes have fat which might skew our distribution when we conduct a hypothesis test. Now lets see the distribution of number of steps vs carbohydrates.

<iframe src="/assets/steps_vs_carbs.html" width=800 height=600 frameBorder=0></iframe>

This graph appears to follow some linear distribution with a higher amount of steps correlating to a higher amount of carbohydrates. But another question that can be asked is what if steps is not the true reason for higher nutrition, but instead a higher amount of steps results in higher amount of ingredient which instead is the cause for higher nutrition. We can see whether or not his is true by creating a pivot table with index as the steps, ingredients as the columns and values as calories. 

<table border="1" class="dataframe">  <thead>    <tr style="text-align: right;">      <th>n_ingredients</th>      <th>1</th>      <th>2</th>      <th>3</th>      <th>4</th>      <th>5</th>      <th>6</th>      <th>7</th>      <th>8</th>    </tr>    <tr>      <th>n_steps</th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>      <th></th>    </tr>  </thead>  <tbody>    <tr>      <th>1</th>      <td>NaN</td>      <td>213.295082</td>      <td>246.122078</td>      <td>226.167308</td>      <td>324.183992</td>      <td>223.804375</td>      <td>308.724324</td>      <td>313.586420</td>    </tr>    <tr>      <th>2</th>      <td>1851.75</td>      <td>224.256471</td>      <td>236.727032</td>      <td>247.509088</td>      <td>266.345312</td>      <td>287.740701</td>      <td>317.015464</td>      <td>318.145852</td>    </tr>    <tr>      <th>3</th>      <td>201.20</td>      <td>266.496460</td>      <td>251.027123</td>      <td>270.738571</td>      <td>277.039463</td>      <td>299.443945</td>      <td>320.241935</td>      <td>324.131434</td>    </tr>    <tr>      <th>4</th>      <td>284.80</td>      <td>391.448039</td>      <td>237.642314</td>      <td>385.776799</td>      <td>329.568616</td>      <td>312.687052</td>      <td>321.440529</td>      <td>340.693716</td>    </tr>    <tr>      <th>5</th>      <td>199.50</td>      <td>303.427848</td>      <td>359.144558</td>      <td>314.562428</td>      <td>346.674227</td>      <td>326.139122</td>      <td>360.033325</td>      <td>334.946556</td>    </tr>    <tr>      <th>6</th>      <td>NaN</td>      <td>356.192857</td>      <td>424.277632</td>      <td>365.591860</td>      <td>376.002450</td>      <td>374.634507</td>      <td>397.015855</td>      <td>361.519975</td>    </tr>    <tr>      <th>7</th>      <td>3116.20</td>      <td>407.926190</td>      <td>334.634911</td>      <td>320.568382</td>      <td>337.915513</td>      <td>359.516051</td>      <td>389.614938</td>      <td>391.944647</td>    </tr>    <tr>      <th>8</th>      <td>NaN</td>      <td>280.927907</td>      <td>317.607947</td>      <td>300.453793</td>      <td>333.636905</td>      <td>385.448466</td>      <td>389.633145</td>      <td>380.285597</td>    </tr>  </tbody></table>
It appears as we move right on the table, the calories dont necessarily follow any patter in any of the rows hence this idea that maybe ingredients is the cause for the correlation is likely not correct. Hence we can continue to investigate just how the number of steps affects nutrition.

## Missing Values
