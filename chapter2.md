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

To continue with this chapter, hit the 'Submit Answer' button.

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
success_msg("Let's try and use our knowledge on statistical inference to assist our company.")
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

Which built in R function can we use to apply the correct statistical method? Use the console screen on the right to investigate the different functions through `?function` command.

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
msg_success <- "Correct! By assigning a explanatory variable to `x` and the response variable to `y` from data on previous orders we can call the function `lm(y~x)` to fit the least-squared regression line to the data points. We can also use `summary(lm(y~x))` to view its outputs."
test_mc(correct = 7, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad, msg_bad, msg_bad, msg_bad))
```



--- type:NormalExercise lang:r xp:100 skills:1 key:59933cccde
## Analysing the data

Data on all the previous productions of the company can be found in the workspace as the `boatManufacturing` dataframe. 

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

# plot(nComplications, time_hr)
# plot(length_m, time_hr)
# plot(width_m, time_hr)
# plot(speed_knots, time_hr)
# plot(price_ZAR, time_hr)

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
            
success_msg("Good job! By looking at the scatter plots we can get a sense of how good certain variables are at predicting others. Ideally we want analyse this in a more quantitative way. In the next exercise we will calculate and compare the correlation coefficient between the variables to see exactly how strong the descriptive variable correlates to our response variable..")
```