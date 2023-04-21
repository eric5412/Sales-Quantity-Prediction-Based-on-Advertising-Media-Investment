Sales Quantity Prediction Based on Advertising Media Investment
================


### Introduction:

Data was analyzed for a company that invested in television, radio, and
newspaper advertising. The scope of this project was to provide an
overview of the amounts that were spent in these different media and
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


### Loading the Packages:

``` r
library(tidyverse)
library(corrplot)
library(reshape2)
library(scales)
```


### Loading the Data:

``` r
# To load the data and to save it to a data frame called advertising_df:
advertising_df <- read_csv("Advertising.csv")
```


### Viewing the Data:

``` r
# To view the first six rows of the data frame:
head(advertising_df)
```

    ## # A tibble: 6 × 5
    ##    ...1    TV radio newspaper sales
    ##   <dbl> <dbl> <dbl>     <dbl> <dbl>
    ## 1     1 230.   37.8      69.2  22.1
    ## 2     2  44.5  39.3      45.1  10.4
    ## 3     3  17.2  45.9      69.3   9.3
    ## 4     4 152.   41.3      58.5  18.5
    ## 5     5 181.   10.8      58.4  12.9
    ## 6     6   8.7  48.9      75     7.2

The independent variables were TV, radio, and newspaper. The dependent
variable was sales.


The first column of the data frame was an index indicating the number
assigned to each specific market. This column was removed, because it
was not used in the analysis.

``` r
# To remove the first column of the data frame:
advertising_df <- advertising_df %>% select(TV, radio, newspaper, sales)
```


The “radio”, “newspaper”, and “sales” columns were then capitalized.

``` r
# To rename the "radio", "newspaper", and "sales" columns:
advertising_df <- advertising_df %>% 
  rename(Radio = radio, Newspaper = newspaper, Sales = sales)
```


The advertising expense values for television, radio, and newspaper were
provided in terms of thousands of dollars. In order to change the
formatting, the values in each column were multiplied by 1,000.

``` r
# To first create a copy of the original data frame:
advertising_df_v1 <- advertising_df

# To multiply the "TV" column by 1,000:
advertising_df_v1$TV <- advertising_df_v1$TV * 1000

# To multiply the "Radio" column by 1,000:
advertising_df_v1$Radio <- advertising_df_v1$Radio * 1000

# To multiply the "Newspaper" column by 1,000:
advertising_df_v1$Newspaper <- advertising_df_v1$Newspaper * 1000
```


The values for quantity of product sold were provided in terms of
thousands of units. In order to change the formatting, these values were
multiplied by 1,000.

``` r
# To multiply the "Sales" column by 1,000:
advertising_df_v1$Sales <- advertising_df_v1$Sales * 1000
```


### Viewing the Structure of the Data:

``` r
glimpse(advertising_df_v1)
```

    ## Rows: 200
    ## Columns: 4
    ## $ TV        <dbl> 230100, 44500, 17200, 151500, 180800, 8700, 57500, 120200, 8…
    ## $ Radio     <dbl> 37800, 39300, 45900, 41300, 10800, 48900, 32800, 19600, 2100…
    ## $ Newspaper <dbl> 69200, 45100, 69300, 58500, 58400, 75000, 23500, 11600, 1000…
    ## $ Sales     <dbl> 22100, 10400, 9300, 18500, 12900, 7200, 11800, 13200, 4800, …


### Summary Statistics:

``` r
summary(advertising_df_v1)
```

    ##        TV             Radio         Newspaper          Sales      
    ##  Min.   :   700   Min.   :    0   Min.   :   300   Min.   : 1600  
    ##  1st Qu.: 74375   1st Qu.: 9975   1st Qu.: 12750   1st Qu.:10375  
    ##  Median :149750   Median :22900   Median : 25750   Median :12900  
    ##  Mean   :147043   Mean   :23264   Mean   : 30554   Mean   :14022  
    ##  3rd Qu.:218825   3rd Qu.:36525   3rd Qu.: 45100   3rd Qu.:17400  
    ##  Max.   :296400   Max.   :49600   Max.   :114000   Max.   :27000

#### **Findings:**

- Television was the most expensive form of advertising for the company.
- Radio was the least expensive form of advertising for the company.


The data frame was examined to determine if there were any duplicate
rows.

``` r
# To examine the data frame for any duplicate rows:
nrow(advertising_df_v1[duplicated(advertising_df_v1), ])
```

    ## [1] 0

There were no duplicate rows found in the data frame.


The data frame was inspected for any missing values.

``` r
# To inspect the data frame for missing values:
sapply(advertising_df_v1,function(x) sum(is.na(x)))
```

    ##        TV     Radio Newspaper     Sales 
    ##         0         0         0         0

There were no missing values in the data frame.


## **Part 1:**

### Exploratory Data Analysis:

A box plot was created to analyze the amount that was spent on
advertising for each media:

``` r
# To remove the "Sales" column when creating the box plot:
box_plot_df <- advertising_df_v1 %>% select(-Sales)

# To reshape the box_plot_df data frame to the long format:
box_plot_df_long <- melt(box_plot_df)

# To create the box plot:
ggplot(box_plot_df_long, aes(variable, value)) + 
  geom_boxplot() +
  scale_y_continuous(labels=scales::dollar_format(), limits = c(0, 300000), 
                     breaks = seq(0, 300000, by = 50000)) +
  labs(title = "Advertising Expense by Media",
       x = "Media",
       y = "Advertising Expense") +
  theme(text = element_text(size = 12.50))
```

![a box plot of the amount spent on
advertising by media](/images/1.png)

#### **Findings:**

- Television had the greatest range in the amount spent by the company
  for advertising across their markets.
- Advertising on television was considerably more expensive than the
  other two media.
- The newspaper advertising expense and radio advertising expense
  resulted in ranges that were similar to each other, with the exception
  of two newspaper outlier values.


A numerical summary of the box plot was created.

``` r
# To create a numerical summary of the box plot:
summary(box_plot_df)
```

    ##        TV             Radio         Newspaper     
    ##  Min.   :   700   Min.   :    0   Min.   :   300  
    ##  1st Qu.: 74375   1st Qu.: 9975   1st Qu.: 12750  
    ##  Median :149750   Median :22900   Median : 25750  
    ##  Mean   :147043   Mean   :23264   Mean   : 30554  
    ##  3rd Qu.:218825   3rd Qu.:36525   3rd Qu.: 45100  
    ##  Max.   :296400   Max.   :49600   Max.   :114000

#### **Findings:**

- The highest advertising expense for the company was television.
- The lowest advertising expense for the company was radio.
- The median amount spent on **TV** advertising was \$149,750, followed
  by \$25,750 spent on **Newspaper** advertising, and \$22,900 spent on
  **Radio** advertising. The mean values and the third quartile values
  displayed the same trend.


The outlier values for newspaper advertising were examined.

``` r
# To examine the outlier values for newspaper advertising:
boxplot(advertising_df_v1$Newspaper, plot=FALSE)$out
```

    ## [1] 114000 100900

The newspaper advertising expense outlier values were \$114,000 and
\$100,900. These high values could have been due to a data entry error,
or it was just very expensive for the company to advertise in newspapers
in those two particular markets.


A box plot was created to check if the “Sales” column had any outliers.

``` r
# To create a box plot to check for outliers in the "Sales" column:
ggplot(aes(x = "", y = Sales), data = advertising_df_v1) + 
  geom_boxplot() +
  scale_y_continuous(labels = scales::comma, limits = c(0, 30000)) +
  labs(x = "",
       y = "Quantity of Product Sold",
       title = "Box Plot of Sales") +
  theme(text = element_text(size = 12.25))
```

![a box plot of the sales column](/images/2.png)

There were no outliers present in the “Sales” column.


A numerical summary was created for the “Sales” column box plot.

``` r
# To create a numerical summary of the box plot for the "Sales" column:
advertising_df_v1 %>% select(Sales) %>% summary()
```

    ##      Sales      
    ##  Min.   : 1600  
    ##  1st Qu.:10375  
    ##  Median :12900  
    ##  Mean   :14022  
    ##  3rd Qu.:17400  
    ##  Max.   :27000

The lowest quantity of units sold was 1,600 units and the highest
quantity of units sold was 27,000 units. The average (mean) quantity of
units sold was 14,022 units.


A bar chart was then created to analyze the total amount that the
company spent on each advertising media.

``` r
# To create a bar chart to analyze the total amount that the company spent on 
# each advertising media:
box_plot_df_long %>%
  group_by(variable) %>%
  summarize(sum = sum(value, na.rm=TRUE)) %>%
  ungroup() %>%
  ggplot(aes(reorder(factor(variable), -sum), sum)) + 
  geom_col(width = 0.4) +
  geom_text(aes(label = dollar(sum),
                vjust = -0.7, hjust = 0.5)) +
  labs(title = "Total Expense by Advertising Media",
       x = "Advertising Media",
       y = "Total Expense") +
  theme(text = element_text(size = 12.50)) +
  scale_y_continuous(labels=scales::dollar_format(), limits = c(0, 32000000), 
                   breaks = seq(0, 32000000, by = 8000000))
```

![a bar chart of the total amount spent on each advertising media](/images/3.png)

The company invested most of its advertising budget in television,
followed by newspaper, and then radio.


A pie chart was created to analyze the budget allocation percentage for
the advertising media.

``` r
# To create a pie chart of the budget allocation:
box_plot_df_long %>%
  group_by(variable) %>%
  summarize(sum = sum(value, na.rm=TRUE)) %>%
  ungroup() %>% 
  mutate(prop = sum/sum(sum), percent = round((prop*100), 2)) %>%
  ggplot(aes(x = "", y = percent, fill = variable)) +
  geom_bar(stat="identity", width=1) +
  coord_polar("y", start=0) +
  geom_text(aes(label = paste0(percent, "%")), size = 7.0, vjust = -0.5, 
            position = position_stack(vjust = 0.8)) +
  labs(x = NULL, y = NULL, fill = "Media") +
  theme_classic() +
  theme(axis.line = element_blank(),
        axis.text = element_blank(),
        axis.ticks = element_blank()) +
  labs(title = "Percentage of Advertising Budget by Media",
       fill = "Media") +
theme(text = element_text(size = 20.00))
```

![a pie chart of the budget allocation](/images/4.png)

Approximately three quarters of the advertising budget was allocated to
television. The percentage of the budget invested in newspaper
advertising and radio advertising were approximately equal to each
other.


A correlation matrix was created to analyze the relationships between
the advertising media and the quantity of product sold.

``` r
# To create a correlation matrix to analyze the relationships between the 
# variables:
round(cor(advertising_df_v1), 2)
```

    ##             TV Radio Newspaper Sales
    ## TV        1.00  0.05      0.06  0.78
    ## Radio     0.05  1.00      0.35  0.58
    ## Newspaper 0.06  0.35      1.00  0.23
    ## Sales     0.78  0.58      0.23  1.00

``` r
corrplot(cor(advertising_df_v1))
```

![a correlation matrix to analyze the relationships between the variables](/images/5.png)

#### **Findings:**

- Sales had a strong positive correlation with television advertising.
- Sales had a moderate positive correlation with radio advertising.
- Sales had a very weak positive correlation with newspaper advertising.
- There was some multicollinearity present between radio advertising and
  newspaper advertising. Multicollinearity occurs when independent
  variables are correlated with each other, which can make it difficult
  to determine the effect of each independent variable on the dependent
  variable. To address this, a solution is to remove one of the
  correlated independent variables and to use the one that has the
  strongest correlation with the dependent variable.


A scatter plot was created to visualize the relationship between sales
and the amount spent on television advertising.

``` r
# Quantity of Product Sold vs. TV Advertising Expense:
ggplot(advertising_df_v1, aes(x = TV, y = Sales))+
  geom_point()+
  geom_smooth(method = "lm", se = FALSE)+
  labs(x = "TV Advertising Expense",
       y = "Quantity of Product Sold",
       title = "Quantity of Product Sold vs. TV Advertising Expense") +
  theme(text = element_text(size = 12.25)) +
  scale_y_continuous(labels = scales::comma, limits = c(0, 28000), 
                     breaks = seq(0, 28000, by = 7000)) + #divided by 4
  scale_x_continuous(labels=scales::dollar_format(), limits = c(0, 300000),
                     breaks = seq(0, 300000, by = 60000)) # divided by 5
```

![a scatter plot of the relationship between sales and the amount spent on television advertising](/images/6.png)

There was a strong positive correlation between the quantity of product
sold and the amount spent on television advertising for the product.


A scatter plot was created to visualize the relationship between sales
and the amount spent on radio advertising.

``` r
# Quantity of Product Sold vs. Radio Advertising Expense:
ggplot(advertising_df_v1, aes(x = Radio, y = Sales))+
  geom_point()+
  geom_smooth(method = "lm", se = FALSE)+
  labs(x = "Radio Advertising Expense",
       y = "Quantity of Product Sold",
       title = "Quantity of Product Sold vs. Radio Advertising Expense") +
  theme(text = element_text(size = 12.25)) +
  scale_y_continuous(labels = scales::comma, limits = c(0, 28000), 
                     breaks = seq(0, 28000, by = 7000)) +
  scale_x_continuous(labels=scales::dollar_format(), limits = c(0, 50000),
                     breaks = seq(0, 50000, by = 10000))
```

![a scatter plot of the relationship between sales and the amount spent on radio advertising](/images/7.png)

There was a moderate positive correlation between the quantity of
product sold and the amount spent on radio advertising for the product.


A scatter plot was created to visualize the relationship between sales
and the amount spent on newspaper advertising.

``` r
# Quantity of Product Sold vs. Newspaper Advertising Expense:
ggplot(advertising_df_v1, aes(x = Newspaper, y = Sales))+
  geom_point()+
  geom_smooth(method = "lm", se = FALSE)+
  labs(x = "Newspaper Advertising Expense",
       y = "Quantity of Product Sold",
       title = "Quantity of Product Sold vs. Newspaper Advertising Expense") +
  theme(text = element_text(size = 12.25)) +
  scale_y_continuous(labels = scales::comma, limits = c(0, 28000), 
                     breaks = seq(0, 28000, by = 7000)) +
  scale_x_continuous(labels=scales::dollar_format(), limits = c(0, 115000),
                     breaks = seq(0, 115000, by = 23000))
```

![a scatter plot of the relationship between sales and the amount spent on newspaper advertising](/images/8.png)

There was a very weak positive correlation between the quantity of
product sold and the amount spent on newspaper advertising for the
product.


## **Part 2:**

### Multiple Linear Regression Machine Learning Models:

In machine learning, the independent variables are also referred to as
“features” and the dependent variable is also called the “target
variable”.

Multiple Linear Regression is a machine learning algorithm that is used
to analyze the relationship between features and the target variable. It
uses several features to predict the outcome of the target variable.

For this project, the Multiple Linear Regression machine learning
algorithm was used to build models to predict the company’s product
sales quantity given the different amounts that the company had invested
in television, radio, and newspaper advertising.

Two Multiple Linear Regression machine learning models were built and
compared. The models were then evaluated by using the Adjusted R-squared
metric and the Root Mean Square Error (RMSE) metric. The model with the
highest Adjusted R-squared and the lowest RMSE was then selected to
provide the company with a sales quantity prediction that was based on
the amount invested in advertising media.


To begin the model building process, the data frame was first split into
a training set and a testing set.

``` r
# To split the data frame into a training set and a testing set:
library(caTools)

# To make the example reproducible:
set.seed(49)

split <- sample.split(advertising_df_v1$Sales, SplitRatio = 0.8)
training_data <- subset(advertising_df_v1, split == "TRUE")
testing_data <- subset(advertising_df_v1, split == "FALSE")
```

The training set was used for constructing the model. The testing set
was used for validating the model and for making predictions. Model
validation involves using a testing set to evaluate how well a model
fits data that was not used in training the model. For this project, 80%
of the data was used to generate the model and the remaining 20% of the
data was used to test the model.


From the Exploratory Data Analysis, it was observed that
multicollinearity was present between the Radio and Newspaper features.
The VIF (Variance Inflation Factor) metric was used to measure the
degree of multicollinearity between the features.

``` r
# To test for multicollinearity by using VIF (Variance Inflation Factor):
library(car)
set.seed(49)
vif_test <- lm(Sales~., data = training_data)
vif(vif_test)
```

    ##        TV     Radio Newspaper 
    ##  1.003941  1.197732  1.196328

#### **Findings:**

- VIF measures the increase in the variance of an estimated regression
  coefficient that is due to multicollinearity. Multicollinearity occurs
  when two features are highly correlated with each other. If the degree
  of correlation is high enough between them, it may cause problems when
  fitting and interpreting the regression model.

- A VIF value greater than five indicates possible severe correlation
  between one feature and another feature. In this project, the VIF
  values for the Radio and Newspaper features were above one, indicating
  that there was moderate multicollinearity present.

- To address the multicollinearity, two Multiple Linear Regression
  models were built and compared. The first model used all the features.
  The second model did not use the Newspaper feature.

- The Newspaper feature was not used in the second model, because the
  Exploratory Data Analysis showed that the correlation between Sales
  and the Radio feature was stronger than the correlation between Sales
  and the Newspaper feature.


The first Multiple Linear Regression model was built by using all the
features.

``` r
# To make the example reproducible:
set.seed(49)
# To build the first model, using all the features:
model1 <- lm(Sales~. , data = training_data)
```


The summary of the first model was analyzed.

``` r
# To view the summary of the initial model:
summary(model1)
```

    ## 
    ## Call:
    ## lm(formula = Sales ~ ., data = training_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -8619.5  -861.1   273.1  1251.1  2786.9 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 2.906e+03  3.543e+02   8.201 8.22e-14 ***
    ## TV          4.612e-02  1.565e-03  29.476  < 2e-16 ***
    ## Radio       1.833e-01  1.010e-02  18.159  < 2e-16 ***
    ## Newspaper   2.497e-03  6.700e-03   0.373     0.71    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1733 on 156 degrees of freedom
    ## Multiple R-squared:  0.8959, Adjusted R-squared:  0.8939 
    ## F-statistic: 447.5 on 3 and 156 DF,  p-value: < 2.2e-16

#### Model Summary Interpretation:

##### **p-value:**

- A p-value was calculated for each feature that was used to build the
  model. In a Multiple Linear Regression model, if a feature has a
  p-value that is less than 0.05, this indicates that the feature has a
  statistically significant relationship with the target variable.

- The p-value for the TV feature was less than 0.05, indicating that
  there was a statistically significant relationship between the amount
  invested in television advertising and the quantity of product that
  was sold.

- The p-value for the Radio feature was less than 0.05, indicating that
  there was a statistically significant relationship between the amount
  invested in radio advertising and the quantity of product that was
  sold.

- The Newspaper feature did not have a statistically significant
  relationship with Sales, as its p-value was greater than 0.05. This
  supported the findings from the Exploratory Data Analysis, where the
  Quantity of Product Sold vs. Newspaper Advertising Expense scatter
  plot showed that there was a very weak positive correlation between
  the sales and the amount spent on newspaper advertising for the
  product.

##### **Multiple R-squared:**

- Multiple R-squared is the coefficient of determination for a Multiple
  Linear Regression model. It indicates how well the model fits the data
  by describing the amount of variance in the target variable that can
  be explained by the features.

- There is an important consideration with regards to Multiple
  R-squared. Adding more features to the model will increase Multiple
  R-squared, even if the additional features do not add to the
  predicting power of the model. For this reason, the Adjusted R-squared
  should be used instead when comparing Multiple Linear Regression
  models.

##### **Adjusted R-squared:**

- The Adjusted R-squared accounts for features that are not significant
  in the model. It indicates whether adding features improves a
  regression model or not. It only increases if the newly added features
  improve the model’s predicting power.

- The Adjusted R-squared also indicates how well the model fits the
  data. It describes the proportion of the variance in the target
  variable that can be explained by the features.

- In this model, the Adjusted R-squared value indicated that the
  features were able to explain 89.39% of the variance in the Sales
  target variable.

##### **F-statistic p-value:**

- The F-statistic tests the significance of the overall model. It
  indicates whether the model provides a better fit to the data than a
  model that contains no features. Specifically, the p-value for the
  F-statistic indicates if the overall model fit is statistically
  significant. If the p-value for the F-statistic is less than 0.05,
  this indicates that there is a statistically significant relationship
  between the overall model and the target variable.

- In this model, the F-statistic p-value was less than the 0.05
  significance level, indicating that the overall model fit was
  statistically significant. There was a statistically significant
  relationship between the overall model and Sales.


The Root Mean Square Error (RMSE) metric was then calculated by using
the model on the testing set. In Multiple Linear Regression, the RMSE is
used to determine how well a model fits the data. The RMSE is the
standard deviation of the residuals. Residuals are the differences
between the actual data values and the values that were predicted by the
model. As the RMSE value decreases, a model is able to better fit a
dataset. The RMSE was calculated on the testing set in order to evaluate
how well the model performed with data that was not used to train the
model.

``` r
# To calculate the RMSE for the testing set:
sqrt(mean((testing_data$Sales - predict(model1, newdata = testing_data))^2))
```

    ## [1] 1500.6

The testing set RMSE for this model was 1,500.6 units sold. The RMSE was
rounded to 1,501 units sold.


A second Multiple Linear Regression model was built, using only the TV
and Radio features.

``` r
# To make the example reproducible:
set.seed(49)
# To build the second model:
model2 <- lm(Sales~ TV + Radio, data = training_data)
```


The summary of the second model was analyzed.

``` r
# To view the summary of the second model:
summary(model2)
```

    ## 
    ## Call:
    ## lm(formula = Sales ~ TV + Radio, data = training_data)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -8697.6  -898.0   270.6  1245.1  2781.8 
    ## 
    ## Coefficients:
    ##              Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept) 2.945e+03  3.370e+02   8.741 3.33e-15 ***
    ## TV          4.614e-02  1.560e-03  29.576  < 2e-16 ***
    ## Radio       1.848e-01  9.215e-03  20.059  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1728 on 157 degrees of freedom
    ## Multiple R-squared:  0.8958, Adjusted R-squared:  0.8945 
    ## F-statistic: 674.9 on 2 and 157 DF,  p-value: < 2.2e-16

#### Model Summary Interpretation:

##### **p-value:**

- The p-value for the TV feature was less than 0.05, indicating that
  there was a statistically significant relationship between the amount
  invested in television advertising and the quantity of product that
  was sold.

- The p-value for the Radio feature was less than 0.05, indicating that
  there was a statistically significant relationship between the amount
  invested in radio advertising and the quantity of product that was
  sold.

##### **Adjusted R-squared:**

- The Adjusted R-squared indicated that the features were able to
  explain 89.45% of the variance in the Sales target variable.
- Removing the Newspaper feature increased the Adjusted R-squared from
  89.39% to 89.45%, indicating that the second model was able to explain
  more of the variance in the target variable.

##### **F-statistic p-value:**

- The F-statistic p-value was less than the 0.05 significance level,
  indicating that the overall model fit was statistically significant.
- There was a statistically significant relationship between the overall
  model and Sales.


The RMSE was then calculated for the testing dataset:

``` r
# To calculate the RMSE for the testing dataset:
sqrt(mean((testing_data$Sales - predict(model2, newdata = testing_data))^2))
```

    ## [1] 1488.663

The testing set RMSE for this model was 1,488.663 units sold. The RMSE
was rounded to 1,489 units sold.


### Comparison of Multiple Linear Regression Models:

**Adjusted R-squared for the first model:** 89.39%

**Testing set RMSE for the first model:** 1,501


**Adjusted R-squared for the second model:** 89.45%

**Testing set RMSE for the second model:** 1,489


When comparing several Multiple Linear Regression models, a higher
Adjusted R-squared value is preferred. The Adjusted R-squared was higher
in the second model, indicating that the second model was able to
explain more of the variance in the target variable. The second model
was able to fit the data better than the first model.

When comparing several Multiple Linear Regression models, a lower
testing set RMSE value is preferred. The testing set RMSE was lower in
the second model, indicating that the second model was able to generate
more accurate predictions than the first model.


**Conclusion:** The second model was selected to provide the company
with predictions of the quantity of units sold based on advertising
investment, because the second model had a higher Adjusted R-squared and
a lower testing set RMSE than the first model.


### Using the Selected Model to Make Predictions with New Data:

New data was created in order to use the second model to generate a
prediction for the quantity of units sold. The investment in television
advertising was selected to be \$3,000 and the investment in radio
advertising was selected to be \$2,000.

``` r
# To use the second model to make a prediction on new data:
predict(model2, newdata = data.frame(TV = 3000, Radio = 2000))
```

    ##        1 
    ## 3453.449

If the company invested \$3,000 in television advertising and \$2,000 in
radio advertising, the model predicted that the company could expect to
sell an average of 3,453 units of their product.

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



### Recommendations:

- Since sales were very weakly correlated with newspaper advertising,
  and newspaper advertising was more expensive than radio advertising,
  it is recommended that the company focuses on using the television and
  radio media.

- The company can use the selected machine learning model to obtain an
  estimate of the expected quantity of units sold, based on the amount
  that was budgeted for the advertising media.

