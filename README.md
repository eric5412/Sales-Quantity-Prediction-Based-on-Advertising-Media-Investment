Sales Quantity Prediction Based on Advertising Media Investment
================


### Introduction:

Data was analyzed for a company that invested in television, radio, and
newspaper advertising. The scope of this project was to provide an
overview of the amounts that were spent on these different media and
also to use machine learning to predict the quantity sold of the
company’s product based on their advertising investment. The advertising
was done across 200 markets. The advertising expense was provided in
terms of thousands of dollars. The quantity of product sold was provided
in terms of thousands of units.

**Data Source:** Advertising dataset from [An Introduction to
Statistical
Learning](https://www.statlearning.com/resources-first-edition).

**Business Problem:** To analyze how advertising media affected the
quantity sold of a product across markets. The goal of the project was
to provide the company with insights on how investment in television,
radio, and newspaper advertising influenced sales of their product.

#### The project was done using R and it consisted of two parts:

- **Part 1:** An Exploratory Data Analysis was performed in order to
  gain an understanding of how television, radio, and newspaper
  advertising affected the sales of the company’s product across
  different markets. An overview of the advertising expense was
  presented to support the company with planning and business strategy.

- **Part 2:** The Multiple Linear Regression machine learning algorithm
  was used to build models to predict the company’s sales, in terms of
  quantity of product sold, based on the amounts that the company had
  invested in the advertising media. The models were then evaluated
  using the Adjusted R-squared metric and the Root Mean Square Error
  (RMSE) metric to select the best model to provide the sales
  prediction. The company could then use the model to estimate the
  quantity of product they could expect to sell given the amounts that
  they planned to invest in the advertising media.

<br>

![A box plot of the advertising expense by media](/images/1.png)

#### **Findings:**

- Television had the greatest range in the amount spent by the company
  for advertising across their markets.
- Advertising on television was considerably more expensive than the
  other two media.
- The newspaper advertising expense and radio advertising expense
  resulted in ranges that were similar to each other, with the exception
  of two newspaper outlier values.

<br>

![A bar chart of the total amount spent on each advertising media](/images/3.png)

The company invested most of its advertising budget in television,
followed by newspaper, and then radio.

<br>

![A pie chart of the budget allocation](/images/4.png)

Approximately three quarters of the advertising budget was allocated to
television. The percentage of the budget that was invested in newspaper
advertising was approximately equal to the percentage of the budget that
was invested in radio advertising.

<br>

![A scatter plot of the relationship between sales and the amount spent on television advertising](/images/6.png)

There was a strong positive correlation between the quantity of product
sold and the amount spent on television advertising for the product.

<br>

In machine learning, the independent variables are also called
“features” and the dependent variable is also called the “target
variable”. Multiple Linear Regression is a machine learning algorithm
that is used to analyze the relationship between features and the target
variable. The algorithm uses two or more features when predicting the
outcome of the target variable.

For this project, the Multiple Linear Regression machine learning
algorithm was used to build models to predict the company’s sales
quantity given the different amounts that the company had invested in
television, radio, and newspaper advertising.

The dataset was split into a training set and a testing set. The training set was used for model building and the testing set was used for model validation. Two Multiple Linear Regression machine learning models were built and
compared. The models were then evaluated by using the Adjusted R-squared
metric and the testing set Root Mean Square Error (RMSE) metric. The model with the
highest Adjusted R-squared and the lowest testing set RMSE was then selected to
provide the company with a sales quantity prediction that was based on
the amount invested in advertising media.

<br>

### Comparison of Multiple Linear Regression Models:

**Adjusted R-squared for the first model:** 89.39%

**Testing set RMSE for the first model:** 1,501

<br>

**Adjusted R-squared for the second model:** 89.45%

**Testing set RMSE for the second model:** 1,489

<br>

When comparing several Multiple Linear Regression models, a higher
Adjusted R-squared value is preferred. The Adjusted R-squared was higher
in the second model, indicating that the second model was able to
explain more of the variance in the target variable. The second model
was able to fit the data better than the first model.

When comparing several Multiple Linear Regression models, a lower
testing set RMSE value is preferred. The testing set RMSE was lower in
the second model, indicating that the second model was able to generate
more accurate predictions than the first model.

<br>

**Conclusion:** The second model was selected to provide the company
with predictions of the quantity of units sold based on advertising
investment, because the second model had a higher Adjusted R-squared and
a lower testing set RMSE than the first model.

<br>

### Key Takeaways:

- Television was the most expensive form of advertising for the company.

- Radio was the least expensive form of advertising for the company.

- Television had the greatest range in the amount spent by the company
  for advertising across their markets.

- Approximately three quarters of the advertising budget was allocated
  to television.

- The lowest quantity of units sold was 1,600 units and the highest
  quantity of units sold was 27,000 units. The average (mean) quantity
  of units sold was 14,022 units.

- The best performing Multiple Linear Regression machine learning model
  was selected to provide the company with predictions of the quantity
  of units sold based on advertising investment. The criteria for model
  selection was based on which model had the highest Adjusted R-squared
  value and the lowest testing set RMSE value.

<br>

### Recommendations:

- Since sales were very weakly correlated with newspaper advertising,
  and newspaper advertising was more expensive than radio advertising,
  it is recommended that the company focuses on using the television and
  radio media.

- The company can use the selected machine learning model to obtain an
  estimate of the expected quantity of units sold, based on the amount
  that was budgeted for the advertising media.

<br> <br> <br> <br> <br> <br>

