--- 
title_meta  : Case study 5
title       : Case study 5 - Yacht production planning
description : In this case study we will try and predict the production time of luxury yachts using data on previous productions. Our objective is to develop a predictive model to be used for production planning. To develop the model we will analyse different variables from previous productions to identify the best predictor, and thereafter use linear-regression to fit a least-squared regression line, to be used for predicting the manufacturing time of our yachts.

--- type:NormalExercise lang:r xp:0 skills:1 key:ec139ff825
## Background

In this case study we will try and predict the production time of luxury yachts using data on previous productions. 
Our objective is to develop a predictive model to be used for production planning. 
To develop the model we will analyse different variables from previous productions to identify the best predictor, and thereafter use linear-regression to fit a least-squared regression line, to be used for predicting the manufacturing time of our yachts.

A critical component of manufacturing is production planning, which includes, amongst others:

1. Determine the resource and material requirements for production.
2. Scheduling the work to be conducted in the facility, including the purchase and delivery of materials.
3. Setting up and delivering production orders to clients.

We are responsible for production planning at luxury yachts manufacturing company. 
Given the uniques of our products, our production operates on the produce-to-order principle, whereby manufacturing only starts once an order has been placed for a yacht. 
Clients are also in full control of the yacht specifications, and can decide on the size of the yacht, its interior and a bunch of other customisations.
As a result, each order is unique and has its own materials and manufacturing requirements.
To manufacture the different yachts, we have a general purpose yacht-building facility, allowing us to build any type of yacht.
The only-limitation that we have is that we can only manufacture one yacht at a time.

A key input into production planning is the expected time that an order will take to complete.
This information is critical to communicate to the customer.
It is also used to predict when the next built will start, and to schedule its material delivery accordingly.

In this case study we will develop a simple predictive model, in the form of a linear-regression model, to predict how long a yacht will take to complete based on some of its key specifications.

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read the instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

Before continuing, have a quick look at the following tutorial as you may need to refer to it during the course of the chapter: 

* Refer to [http://www.cyclismo.org/tutorial/R/linearLeastSquares.html](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html).

After going through the tutorial and to continue with this chapter, hit the 'Submit Answer' button.

*** =instructions

- Hit the 'Submit Answer' button when you're done reading the instructions.

*** =hint
Just hit the 'Submit Answer'.

```{r cars}
summary(cars)
```

*** =pre_exercise_code
```{r}
#none

```

*** =sample_code
```{r}

```

*** =solution
```{r}
#none
```

*** =sct
```{r}
success_msg("Let's try and use our knowledge on regression to assist our company.")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:c6773a7f96
## Statistical methods to apply

Based on the case study description, which do you think is the correct statistical method to apply?

*** =instructions
- Inference for a single proportion.
- Inference for the difference between two proportions.
- Goodness-of-fit test.
- Test for independence.
- Inference for a single mean.
- Inference for paired numerical data.
- Inference for the difference between two means.
- Regression analysis

*** =hint
Have a look at the chapters on regression in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}
#none
```

*** =sct
```{r}
msg_bad <- "That is not correct. Have a look at the chapters on regression in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook."

msg_success <- "Correct! We want to predict how long a yacht will take to manufacture, based on its features. To do so we need to identify appropriate explanatory variables that best predict our response variable, which in this case, is the time required to complete the production of a yacht. The explanatory variables are features of the yacht. Using data on previous productions we need to first identify which explanatory variables to use, and thereafter fit a linear-model using the existing data. The linear-model can thereafter be used to predict the time required to complete an order based on its key features."
test_mc(correct = 8, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_success))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:b2d2f1b742

## R functions to apply

Which built in R function can we use to apply the correct statistical method? Use the console screen on the right to investigate the different functions through the `?function` command.

*** =instructions
- `plot()`
- `mean()`
- `sd()`
- `nrow()`
- `prop.test()`
- `t.test()`
- `lm()`
- `cor()`
- `var()`
- `cov()`
- None of the above, we will have to write our own R code to apply the method.

*** =hint

Use the `?function` to find out more about each command.

*** =pre_exercise_code
```{r}
#none
```

*** =sct
```{r}
msg_bad <- "That is not correct. Remember that you can use the `?function` to find out more about each function."
msg_success <- "Correct! By assigning a explanatory variable to `x` and the response variable to `y`, based on data of previous orders, we can call the function `lm(y~x)` to fit the least-squared regression line to the data points. We can also use `summary(lm(y~x))` to view its outputs."
test_mc(correct = 7, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad, msg_bad, msg_bad, msg_bad))
```



--- type:NormalExercise lang:r xp:100 skills:1 key:59933cccde
## Analysing the data

Data on all the previous productions of the company can be found in the `boatManufacturing` dataframe. 

It consists of five variables:

1. The number of complications (`nComplications`), which is the total number of customisations for the yacht. 
2. Total yacht price in South African Rands (`price_ZAR`); 
3. Yacht length in meters (`length_m`).
4. Yacht width in meters (`width_m`). 
5. Top speed in knots (`speed_knots`).
6. Time in hours it took to manufacture the yacht (`time_hr`). 

First have a look at the data in the console using the `head(boatManufacturing)` command, thereafter answer the following:

*** =instructions
1. Calculate the number of yachts that we have manufactured and assign your answer to `nYachts`.
2. Calculate the mean manufacturing time of a yacht and assign your answer to `production_mean`.
3. Draw a histogram of the manufacturing time of yachts to analyse the distribution.
4. For each of the five possible descriptive variables, draw a scatter plot of the variable against manufacturing time.
5. Decide on which variable you think will be the _best predictor_ of manufacturing time and assign your answer to `bestPredictor`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.
6. Decide on which variable you think will be the _second best_ predictor of manufacturing time and assign your answer to `secondBestPredictor`. Simply assign the variable name, as a string, to `secondBestPredictor`.
7. Decide on which variable you think will be the _worst_ predictor of manufacturing time and assign your answer to `worstPredictor`. Simply assign the variable name, as a string, to `worstPredictor`.

*** =hint

Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help.

*** =pre_exercise_code
```{r}
nBoats <- floor(runif(1, min = 100, max = 200))
nComplications <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
length_m <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
width_m <- length_m/4 + rnorm(n = nBoats, mean = 15, sd = 5)
speed_knots <- round(runif(n = nBoats, min = 50, max = 100), 0)
price_ZAR <- nComplications*100000 + length_m*width_m*2500 + speed_knots*50000
time_hr <- length_m*10 + rnorm(n = nBoats, mean = 300, sd = 150)
boatManufacturing <- data.frame(nComplications, length_m, width_m, speed_knots, price_ZAR, time_hr)

rm(nBoats)
rm(nComplications)
rm(length_m)
rm(width_m)
rm(speed_knots)
rm(price_ZAR)
rm(time_hr)

```

*** =sample_code
```{r}
# The boatManufacturing dataframe is available in your workspace. 

# 1. Calculate the number of yachts that we have manufactured and assign your answer to `nYachts`.

nYachts <- 

# 2. Calculate the mean manufacture time of a yacht and assign your answer to `production_mean`.

production_mean <- 

# 3. Draw a histogram of the manufacture time of yachts to analyse the distribution.

hist()

# 4. For each of the five possible descriptive variables, draw a scatter plot of the variable against the manufacture time.

plot() # nComplications
plot() # length_m
plot() # width_m
plot() # speed_knots
plot() # price_ZAR

# 5. Decide on which variable you think will be the _best predictor_ of manufacturing time and assign your answer to `bestPredictor`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.
              
bestPredictor <- 

# 6. Decide on which variable you think will be the _second best_ predictor of manufacturing time and assign your answer to `secondBestPredictor`. Simply assign the variable name, as a string, to `secondBestPredictor`.

secondBestPredictor <- 

# 7. Decide on which variable you think will be the _worst_ predictor of manufacturing time and assign your answer to `worstPredictor`. Simply assign the variable name, as a string, to `worstPredictor`.

worstPredictor <- 

# Print the results here:

nYachts
production_mean
bestPredictor
secondBestPredictor
worstPredictor
```

*** =solution
```{r}
# The boatManufacturing dataframe is available in your workspace. 

# 1. Calculate the number of yachts that we have manufactured and assign your answer to `nYachts`.

nYachts <- nrow(boatManufacturing)

# 2. Calculate the mean manufacture time of a yacht and assign your answer to `production_mean`.

production_mean <- mean(boatManufacturing$time_hr)

# 3. Draw a histogram of the manufacture time of yachts to analyse the distribution.

hist(boatManufacturing$time_hr)

# 4. For each of the five possible descriptive variables, draw a scatter plot of the variable against the manufacture time in the sequence specified below:

plot(boatManufacturing$nComplications, boatManufacturing$time_hr) # nComplications
plot(boatManufacturing$length_m, boatManufacturing$time_hr) # length_m
plot(boatManufacturing$width_m, boatManufacturing$time_hr) # width_m
plot(boatManufacturing$speed_knots, boatManufacturing$time_hr) # speed_knots
plot(boatManufacturing$price_ZAR, boatManufacturing$time_hr) # price_ZAR

# 5. Decide on which variable you think will be the _best predictor_ of manufacturing time and assign your answer to `bestPredictor`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.

discriptiveVariables <- c('nComplications', 'length_m', 'width_m', 'speed_knots', 'price_ZAR')

cors <- c(cor(boatManufacturing$nComplications, boatManufacturing$time_hr),
          cor(boatManufacturing$length_m, boatManufacturing$time_hr),
          cor(boatManufacturing$width_m, boatManufacturing$time_hr),
          cor(boatManufacturing$speed_knots, boatManufacturing$time_hr),
          cor(boatManufacturing$price_ZAR, boatManufacturing$time_hr))
              
bestPredictor <- discriptiveVariables[which.max(cors)]

# 6. Decide on which variable you think will be the _second best_ predictor of manufacturing time and assign your answer to `secondBestPredictor`. Simply assign the variable name, as a string, to `secondBestPredictor`.

corsTemp = cors
corsTemp[which.max(cors)] <- 0

secondBestPredictor <- discriptiveVariables[which.max(corsTemp)]

# 7. Decide on which variable you think will be the _worst_ predictor of manufacturing time and assign your answer to `worstPredictor`. Simply assign the variable name, as a string, to `worstPredictor`.

worstPredictor <- discriptiveVariables[which.min(cors)]

# Print the results here:

nYachts
production_mean
bestPredictor
secondBestPredictor
worstPredictor

```

*** =sct
```{r}
test_object("nYachts", undefined_msg = "Make sure to define a variable `nYachts`.",
            incorrect_msg = "Make sure that you calculated the number of yachts manufactured correctly and assigned your answer to `nYachts`.") 
    
test_object("production_mean", undefined_msg = "Make sure to define a variable `production_mean`.",
            incorrect_msg = "Make sure that you calculated the mean manufacturing time of the yachts correctly and assigned your answer to `production_mean`.") 
            
test_function('hist', args = "x", not_called_msg = "Draw a histogram of the mean manufacturing time for yachts.",
              incorrect_msg = "Your call to `hist()` is incorrect. Make sure you are drawing a histogram of the correct variable.")
              
test_function("plot", args = c("x", "y"), index = 1, not_called_msg = "Draw a scatter plot for all five variables against the yacht manufacturing time.",
                                                    incorrect_msg = "Make sure the your x (descriptive) and y (response) variables for the plot are correct.")
test_function("plot", args = c("x", "y"), index = 2, not_called_msg = "Draw a scatter plot for all five variables against the yacht manufacturing time.",
                                                    incorrect_msg = "Make sure the your x (descriptive) and y (response) variables for the plot are correct.")
test_function("plot", args = c("x", "y"), index = 3, not_called_msg = "Draw a scatter plot for all five variables against the yacht manufacturing time.",
                                                    incorrect_msg = "Make sure the your x (descriptive) and y (response) variables for the plot are correct.")
test_function("plot", args = c("x", "y"), index = 4, not_called_msg = "Draw a scatter plot for all five variables against the yacht manufacturing time.",
                                                    incorrect_msg = "Make sure the your x (descriptive) and y (response) variables for the plot are correct.")
test_function("plot", args = c("x", "y"), index = 5, not_called_msg = "Draw a scatter plot for all five variables against the yacht manufacturing time.",
                                                    incorrect_msg = "Make sure the your x (descriptive) and y (response) variables for the plot are correct.")
                                                    
test_object("bestPredictor", undefined_msg = "Make sure to define a variable `bestPredictor`.",
            incorrect_msg = "Use the scatter plots to identify the _best_ predictor for manufacturing time and assign your answer to `production_mean`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.") 
            
test_object("secondBestPredictor", undefined_msg = "Make sure to define a variable `secondBestPredictor`.",
            incorrect_msg = "Use the scatter plots to identify the _second best_ predictor for manufacturing time and assign your answer to `secondBestPredictor`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.") 
            
test_object("worstPredictor", undefined_msg = "Make sure to define a variable `worstPredictor`.",
            incorrect_msg = "Use the scatter plots to identify the _worst predictor_ for manufacturing time and assign your answer to `worstPredictor`. Simply assign the variable name, as a string, to `bestPredictor`. For example, if you think it is `nComplications`, then `bestPredictor <- 'nComplications'`---remember the quotation marks.") 
            
success_msg("Good job! By looking at the scatter plots we can get a sense of how good certain variables are at predicting others. Ideally we want analyse this in a more quantitative way. In the next exercise we will calculate and compare the correlation coefficient between the variables to see exactly how strong the descriptive variable correlates to our response variable.")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:073be0f413
## Quantifying the relationship

Data on all the previous productions of the company can be found in the workspace as the `boatManufacturing` dataframe. 

In the previous exercise we eye-balled the best predictor for manufacturing time.
Better would be to quantify it, which is straightforward to do in R, using the `cor` function.
Type `?cor` in the console to find out more about the function.


Typically a correlation can be defined as positive, negative, weak, moderate and strong. In this exercise we are going to going to quantify the correlation between variables and manufacturing time, and thereafter classify the correlation as either positive or negative.

To complete the exercise, do the following:

*** =instructions
1. Calculate the correlation between `nComplications` and `time_hr` and assign your answer to `cor_nComplications`.
2. Calculate the correlation between `length_m` and `time_hr` and assign your answer to `cor_length`.
3. Calculate the correlation between `width_m` and `time_hr` and assign your answer to `cor_width`.
4. Calculate the correlation between `speed_knots` and `time_hr` and assign your answer to `cor_speed`.
5. Calculate the correlation between `price_ZAR` and `time_hr` and assign your answer to `cor_price`.
6. Print the values to the console and check if all the correlations are positive and assign your answer, which can either be `TRUE` or `FALSE` to `allPositive`

*** =hint

Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help.

*** =pre_exercise_code
```{r}
nBoats <- floor(runif(1, min = 100, max = 200))
nComplications <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
length_m <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
width_m <- length_m/4 + rnorm(n = nBoats, mean = 15, sd = 5)
speed_knots <- round(runif(n = nBoats, min = 50, max = 100), 0)
price_ZAR <- nComplications*100000 + length_m*width_m*2500 + speed_knots*50000
time_hr <- length_m*10 + rnorm(n = nBoats, mean = 300, sd = 150)
boatManufacturing <- data.frame(nComplications, length_m, width_m, speed_knots, price_ZAR, time_hr)

rm(nBoats)
rm(nComplications)
rm(length_m)
rm(width_m)
rm(speed_knots)
rm(price_ZAR)
rm(time_hr)

```

*** =sample_code
```{r}
# 1. Calculate the correlation between `nComplications` and `time_hr` and assign your answer to `cor_nComplications`.



# 2. Calculate the correlation between `length_m` and `time_hr` and assign your answer to `cor_length`.



# 3. Calculate the correlation between `width_m` and `time_hr` and assign your answer to `cor_width`.



# 4. Calculate the correlation between `speed_knots` and `time_hr` and assign your answer to `cor_speed`.



# 5. Calculate the correlation between `price_ZAR` and `time_hr` and assign your answer to `cor_price`.



# 6. Print the values to the console and check if all the correlations are positive and assign your answer, which can either be `TRUE` or `FALSE` to `allPositive`



```

*** =solution
```{r}
# 1. Calculate the correlation between `nComplications` and `time_hr` and assign your answer to `cor_nComplications`.

cor_nComplications <- cor(boatManufacturing$nComplications, boatManufacturing$time_hr)

# 2. Calculate the correlation between `length_m` and `time_hr` and assign your answer to `cor_length`.

cor_length <- cor(boatManufacturing$length_m, boatManufacturing$time_hr)

# 3. Calculate the correlation between `width_m` and `time_hr` and assign your answer to `cor_width`.

cor_width <- cor(boatManufacturing$width_m, boatManufacturing$time_hr)

# 4. Calculate the correlation between `speed_knots` and `time_hr` and assign your answer to `cor_speed`.

cor_speed <- cor(boatManufacturing$speed_knots, boatManufacturing$time_hr)

# 5. Calculate the correlation between `price_ZAR` and `time_hr` and assign your answer to `cor_price`.

cor_price <- cor(boatManufacturing$price_ZAR, boatManufacturing$time_hr)

# 6. Print the values to the console and check if all the correlations are positive and assign your answer, which can either be `TRUE` or `FALSE` to `allPositive`

if (min(cor_nComplications, cor_length, cor_width, cor_speed, cor_price) < 0){allPositive <- FALSE}else{allPositive <- TRUE}
```

*** =sct
```{r}
test_object("cor_nComplications", undefined_msg = "Make sure to define a variable `cor_nComplications`.",
            incorrect_msg = "Make sure that you calculated the correlation between `nComplications` and `time_hr` correctly.") 
    
test_object("cor_length", undefined_msg = "Make sure to define a variable `cor_length`.",
            incorrect_msg = "Make sure that you calculated the correlation between `length_m` and `time_hr` correctly.") 
            
test_object("cor_width", undefined_msg = "Make sure to define a variable `cor_width`.",
            incorrect_msg = "Make sure that you calculated the correlation between `width_m` and `time_hr` correctly.") 
test_object("cor_speed", undefined_msg = "Make sure to define a variable `cor_speed`.",
            incorrect_msg = "Make sure that you calculated the correlation between `speed_knots` and `time_hr` correctly.")

test_object("cor_price", undefined_msg = "Make sure to define a variable `cor_price`.",
            incorrect_msg = "Make sure that you calculated the correlation between `price_ZART` and `time_hr` correctly.")

test_object("allPositive", undefined_msg = "Make sure to define a variable `allPositive`.",
            incorrect_msg = "Check the calculate correlations to see if any are negative. If so, set `allPositive <- FALSE`, else `allPositive <- TRUE`.")

success_msg("Correct! The correlation coefficient allows us to quantify the strength of a relationship between two variables. Thereby we can easily identify the descriptive variable with the strongest correlation to the response variable, and use that to fit a linear regression line, which is what we will do next.")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:d275dff6e3
## Fitting a linear regression line

Data on all the previous productions of the company can be found in the workspace as the `boatManufacturing` dataframe.

Now that we know that `length_m` has the strongest relationship with `time_hr` we can fit and plot a linear regression line on our data and use the coefficients of the line to predict how long a boat will take to manufacture, based on its length. To fit a model we will use the `lm` command. Type `?lm` in the console to see how the function works, and thereafter complete the following:

*** =instructions

1. Fit a linear regression line to the `length_m` and `time_hr` variables using the `lm` function, and assign the results to `fit`. To do so, use `fit <- lm(...)`.
2. View the output of the model in the console.
3. Generate a scatter plot of the `length_m` and `time_hr` variables and add the linear-regression line to the plot using the `abline()` plot function. Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help on doing so.

*** =hint

Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help.

*** =pre_exercise_code
```{r}
nBoats <- floor(runif(1, min = 100, max = 200))
nComplications <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
length_m <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
width_m <- length_m/4 + rnorm(n = nBoats, mean = 15, sd = 5)
speed_knots <- round(runif(n = nBoats, min = 50, max = 100), 0)
price_ZAR <- nComplications*100000 + length_m*width_m*2500 + speed_knots*50000
time_hr <- length_m*10 + rnorm(n = nBoats, mean = 300, sd = 150)
boatManufacturing <- data.frame(nComplications, length_m, width_m, speed_knots, price_ZAR, time_hr)

rm(nBoats)
rm(nComplications)
rm(length_m)
rm(width_m)
rm(speed_knots)
rm(price_ZAR)
rm(time_hr)
```

*** =sample_code
```{r}
# 1. Fit a linear regression line to the `length_m` and `time_hr` variables using the `lm` function, and assign the results to `fit`. To do so, use `fit <- lm(...)`.



#2. View the output of the model in the console.



#3. Generate a scatter plot of the `length_m` and `time_hr` variables and add the linear-regression line to the plot using the `abline()` plot function. Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help on doing so.


```

*** =solution
```{r}
# 1. Fit a linear regression line to the `length_m` and `time_hr` variables using the `lm` function, and assign the results to `fit`. To do so, use `fit <- lm(...)`.

fit <- lm(boatManufacturing$time_hr~boatManufacturing$length_m)

#2. View the output of the model in the console.

fit

#3. Generate a scatter plot of the `length_m` and `time_hr` variables and add the linear-regression line to the plot using the `abline()` plot function. Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help on doing so.

plot(boatManufacturing$length_m, boatManufacturing$time_hr)
abline(fit)

```

*** =sct
```{r}
test_object("fit", undefined_msg = "Make sure to define an object `fit`.",
            incorrect_msg = "Make sure that you called the `lm` function correctly. Use `?lm` for help on the function.")

test_output_contains("fit", incorrect_msg = "You did not view the output of the fitted regression line `fit`.")

test_function("plot", args = c("x", "y"), not_called_msg = "Draw a scatter plot of the descriptive and predictive variable.",
             incorrect_msg = "Make sure that your x (descriptive) and y (response) variables for the plot are correct.")
             
test_function("abline", not_called_msg = "Add the linear regression line to the plot.",
             incorrect_msg = "Make sure the you added the linear regression line using the `fit` object that you created.")
             
success_msg("Correct! By using the `lm()` function we now have the least-squared linear regression line for `length_m` and `time_hr` which we can now use to predict the time required to manufacture a yacht, based on its required length. However, before we can use the linear regression line we first need to check certain conditions.")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:4dfe0cdb7c
## Linear regression conditions

Data on all the previous productions of the company can be found in the workspace as the `boatManufacturing` dataframe.

For the linear regression line to be useful for predictive purposes, certain conditions must hold:

1. The samples which we used to fit the line must be independent.
2. The relationship between the descriptive and response variables must be linear.
3. The residuals from the regression line must be nearly normal.
4. The variability of our observations around our regression line must be constant.

Recall that the residual is calculated for each observation as the the actual observation's y value minus its predicted y value using the linear regression line. The linear regression line is of the form 
`y_hat <- beta_0 + beta_1*x`

Both `beta_0` and `beta_1` were determined using the `lm` model so as to minimise the sum of the residuals squared of all our observations.

In this exercise we will fist calculate the residual of the first observation, and thereafter view all the residuals to see if the conditions for linear regression has been met.

*** =instructions

1.  Calculate the residual of the _first_ observation in the dataset by comparing its actual `time_hr` against its predicted `time_hr` based on its `length_m` value. To do so you will first need to fit a linear-regression line using the `lm` function, and then access the intercept (`beta_0`) and slope of the fitted line (`beta_1`) and convert it into the `time_hr_predict <- beta_0 + beta_1*length_m` format. Thereafter you can calculate the residuals. Assign the final residual value to `res_obs1`.
2. Use the `residuals()` function to calculate all the residuals for the observations and assign the results to `res <- residuals`. Use `?residuals` to find out more about the function.
3. View a plot of the residuals to see if variability is constant, meaning it doesn't seem to contract or expand.
4. View a histogram of the residuals to see if it's nearly normal.

*** =hint

Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help.

*** =pre_exercise_code
```{r}
nBoats <- floor(runif(1, min = 100, max = 200))
nComplications <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
length_m <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
width_m <- length_m/4 + rnorm(n = nBoats, mean = 15, sd = 5)
speed_knots <- round(runif(n = nBoats, min = 50, max = 100), 0)
price_ZAR <- nComplications*100000 + length_m*width_m*2500 + speed_knots*50000
time_hr <- length_m*10 + rnorm(n = nBoats, mean = 300, sd = 150)
boatManufacturing <- data.frame(nComplications, length_m, width_m, speed_knots, price_ZAR, time_hr)

rm(nBoats)
rm(nComplications)
rm(length_m)
rm(width_m)
rm(speed_knots)
rm(price_ZAR)
rm(time_hr)
```

*** =sample_code
```{r}
# 1. Calculate the residual of the _first_ observation in the dataset by comparing its actual `time_hr` against its predicted `time_hr` based on its `length_m` value. To do so you will first need to fit a linear-regression line using the `lm` function, and then access the intercept (`beta_0`) and slope of the fitted line (`beta_1`) and convert it into the `time_hr_predict <- beta_0 + beta_1*length_m` format. Thereafter you can calculate the residuals. Assign the final residual value to `res_obs1`.



# 2. Use the `residuals()` function to calculate all the residuals for the observations and assign the results to `res `. Use `?residuals` to find out more about the function.



# 3. View a plot of `length_m` and the residuals to see if variability is constant, meaning it doesn't seem to contract or expand.



# 4. View a histogram of the residuals to see if it's nearly normal.



```

*** =solution
```{r}
# 1. Calculate the residual of the _first_ observation by comparing its actual `time_hr` against its predicted `time_hr` using its `length_m` value. To do so you will first need to fit a linear-regression line using the `lm` function, and then access the intercept (`beta_0`) and slope of the fitted line (`beta_1`) and convert it into the `time_hr_predict <- beta_0 + beta_1*length_m` format. Thereafter you can calculate the residuals. Assign the residual value to `res_obs1`.

fit <- lm(boatManufacturing$time_hr~boatManufacturing$length_m)
time_hr_predict <- fit$coefficients[[1]] + fit$coefficients[[2]]*boatManufacturing$length_m[1]
res_obs1 <- boatManufacturing$time_hr[1] - time_hr_predict

# 2. Use the `residuals()` function to calculate all the residuals for the observations and assign the results to `res `. Use `?residuals` to find out more about the function.

res <- residuals(fit)

# 3. View a plot of `length_m` and the residuals to see if variability is constant, meaning it doesn't seem to contract or expand.

plot(boatManufacturing$length_m, res)

# 4. View a histogram of the residuals to see if it's nearly normal.

hist(res)
```

*** =sct
```{r}
test_object("res_obs1", undefined_msg = "Make sure to define an object `res_obs1`.",
            incorrect_msg = "Make sure that you called the `lm` function, then assigned its coefficients correctly to use the straight line formula, and then used the formula to calculate the residual of the first observation.")

test_object("res", undefined_msg = "Make sure to define an object `residuals`.",
            incorrect_msg = "Make sure that you called the `residuals` function correctly. Use `?residuals` for help on the function.")

test_function("plot", args = c("x", "y"), not_called_msg = "Draw plot of the descriptive variable and residual.",
             incorrect_msg = "Make sure that your x (descriptive) and y (residual) variables for the plot are correct.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the residuals.",
             incorrect_msg = "raw a histogram of the residuals.")

success_msg("Correct! We need to analyse the residuals to check if we can use the regression line for predictive purposes. Depending on the data you may notice the residuals may not have constant variability, but it should follow a normal distribution. In cases where all conditions are not met we can still use the regression line. We just have to interpret the results more carefully. We also did not check the independence assumption. Given the uniqueness of yachts and that only one can be manufactured at a time we can assume that each sample is independent.")
```



--- type:NormalExercise lang:r xp:100 skills:1 key:8f7127b622
## Making a prediction

Data on all the previous productions of the company can be found in the workspace as the `boatManufacturing` dataframe.

For the last question we are going to predict how long a 340m length yacht is going to take to manufacture. We will also check whether we will be extrapolating, meaning that we are going to make a production based on a descriptive variable that falls outside our observed values.

Similar to the previous questions we will do the following:

*** =instructions

1. Fit and visualise the linear regression line, similar to the previous questions. Assign the linear regression model to `fit`.
2. Convert the line into a regression line equation and use the equation to predict how long a 340m boat will take to manufacture. Assign your answer to `y_340m`.
3. Test whether the 340m boat falls outside our observed values. Assign your answer, which can either be `TRUE` or `FALSE` to `predictionExtrapolated`.

*** =hint

Refer to [this tutorial](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html) for help and look at what we've done in the previous exercises.

*** =pre_exercise_code
```{r}
nBoats <- floor(runif(1, min = 100, max = 200))
nComplications <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
length_m <- round(rnorm(n = nBoats, mean = 150, sd = 50), 0)
width_m <- length_m/4 + rnorm(n = nBoats, mean = 15, sd = 5)
speed_knots <- round(runif(n = nBoats, min = 50, max = 100), 0)
price_ZAR <- nComplications*100000 + length_m*width_m*2500 + speed_knots*50000
time_hr <- length_m*10 + rnorm(n = nBoats, mean = 300, sd = 150)
boatManufacturing <- data.frame(nComplications, length_m, width_m, speed_knots, price_ZAR, time_hr)

rm(nBoats)
rm(nComplications)
rm(length_m)
rm(width_m)
rm(speed_knots)
rm(price_ZAR)
rm(time_hr)
```

*** =sample_code
```{r}

# 1. Fit and visualise the linear regression line, similar to the previous questions. Assign the linear regression model to `fit`.



# 2. Convert the line into a regression line equation and use the equation to predict how long a 340m boat will take to manufacture. Assign your answer to `y_340m` and print the result to the console.



# 3. Test whether the 340m boat falls outside our observed values. Assign your answer, which can either be `TRUE` or `FALSE` to `predictionExtrapolated` and print the result to the console.


```

*** =solution
```{r}

# 1. Fit and visualise the linear regression line, similar to the previous questions. Assign the linear regression model to `fit`.

fit <- lm(boatManufacturing$time_hr~boatManufacturing$length_m)
plot(boatManufacturing$length_m, boatManufacturing$time_hr)
abline(fit)

# 2. Convert the line into a regression line equation and use the equation to predict how long a 340m boat will take to manufacture. Assign your answer to `y_340m` and print the result to the console.

x <- 340
y_340m <- fit$coefficients[[1]] + fit$coefficients[[2]]*x
y_340m

# 3. Test whether the 340m boat falls outside our observed values. Assign your answer, which can either be `TRUE` or `FALSE` to `predictionExtrapolated` and print the result to the console.

if (x < min(boatManufacturing$length_m) | x > max(boatManufacturing$length_m)){predictionExtrapolated <- TRUE}else{predictionExtrapolated <- FALSE}
predictionExtrapolated

```

*** =sct
```{r}
test_object("fit", undefined_msg = "Make sure to define an object `fit`.",
            incorrect_msg = "Make sure that you fitted the linear-regression line correctly and assigned the result to `fit`")

test_function("plot", args = c("x", "y"), not_called_msg = "Draw a scatter plot of the descriptive and predictive variable.",
             incorrect_msg = "Make sure that your x (descriptive) and y (response) variables for the plot are correct.")
             
test_function("abline", not_called_msg = "Add the linear regression line to the plot.",
             incorrect_msg = "Make sure the you added the linear regression line using the `fit` object that you created.")

test_object("y_340m", undefined_msg = "Make sure to define an object `y_340m`.",
            incorrect_msg = "Make sure that created a linear regression line equation using the fitted line, calculate the predicted manufacturing time for a 340m yacht correctly, and assigned the result to `y_340m`.")

test_object("predictionExtrapolated", undefined_msg = "Make sure to define an object `predictionExtrapolated`.",
            incorrect_msg = "Check if the yacht length falls outside the observed yacht length. If it is then we are using the model to extrapolate and `predictionExtrapolated <- TRUE`. If not, we can be more reliant on the model results and `predictionExtrapolated <- FALSE`.")
            
success_msg("Correct! You have successfully developed and used a model that predicts how long a new boat will take to manufacture, based its required length. To do this, you first identified the variable that correlates the best with the response variable. You then used the variable and the response variable and fitted a linear regression line to available data. Next you used the regression line and analysed the residuals to check if conditions for regression were met. Lastly, you used the regression line to predict how long a 350m boat will take to manufacture.")
```