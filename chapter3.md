---
title_meta  : Case study 6
title       : Case study 6 - Different applications of probability distributions
description : In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. The distribitions that we will look at are normal, poisson and exponential, power-law, and uniform. After eye-balling the applicable distribution we will find the appropraite distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.

--- type:NormalExercise lang:r xp:0 skills:1 key:4fdf3ba090
## Background

In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. 
The distribitions that we will look at are:

* normal, 
* poisson and exponential, 
* power-law, and 
* uniform. 

To find out more about all R's built-in distribution functions, type `help("distributions")` in the R console.

After eye-balling the applicable distribution we will find the appropraite distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.

We will follow the following crude steps to model a process as a specific distribution:

1. Draw a histogram of the sample data from our process.
2. See what distribution seems like a good fit for the process (eyeballing),
3. Estimate the distribution parameters using the sample data.
4. Use the R functions of the distribution to calculate percentiles and probabilities as required.

The specific R functions that we will be using are:

```
pnorm(), qnorm(), rnorm(), ppois(), qpois(), rpois(), pexp(), qexp(), rexp(), punif(), qunif(), runif()
```

To find out more about the functions, type `?function` in the R console. The basic usage of the functions is described in [this page](http://www.cyclismo.org/tutorial/R/probability.html).

Consult the accompanying lecture slides, `Lecture_Week6.pdf`, available on _clickUP_ under the `Theme 2: Simulation models/Week 6 Introduction to key statistical distributions: lecture material/`, or by clicking [on this link](https://clickup.up.ac.za/bbcswebdav/pid-1016180-dt-content-rid-11418452_1/courses/ban313_s1_2017/Lecture_Week6.pdf).

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read the instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

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
success_msg("Let's use probability distributions to answer some questions.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a44acfd135
## Drill-hole sizes

We wish to replace our current very old drilling machine. 
The machine is required to drill a hole with a diameter of 10cm within 0.5cm tollerance.
If the hole is larger than 10.5cm, the part has to be scrapped.
Similarly, if the hole is smaller than 9.5cm the hole will also be scrapped.

We also want to use the machine to drill holes for other products that have different tollerance.
One product may have a 0.25cm tollerance, another 1cm.

We have identified a possible replacement machine, and its supplier assured us it will perform according to specification.
As industrial engineers we insisted that the supplier provide us with data on its performance, so that we can make a more informed decision on whether it will meet our requirement.

The supplier has given us data on different holes drilled by the machine during its testing.
The data consist of a number of drill samples, with the hole diameter for each hole measured and captured.
The data is available in the `holeSize` dataframe.

For the first part of this question, analyse the distribution of hole sizes from the machine and identify the distribution that best describes the hole-sizes of the machine. 
In the following parts we will find the key parameters of the distribution and use the distribution function to answer some basic planning support questions.

*** =instructions

1. Analyse the distribution of the hole-sizes using the `hist()` function and by playing around with `breaks` parameter. 
2. How many samples fall outside the 0.5cm tollerance limit and are therefore defective: assign your answer to `nDefective05`.
3. How many samples fall outside the 0.25cm tollerance limit and are therefore defective: assign your answer to `nDefective025`.
4. How many samples fall outside the 1cm tollerance limit and are therefore defective: assign your answer to `nDefective1`.
5. View the values by printing them to the console output via the `script.R` file.

*** =hint

Use `hist(holeSize$holeDiameter_cm)` to analyse the distribution and see which of the four distributions

* normal, 
* poisson and exponential, 
* power-law, or 
* uniform,

it most closely represents.

Use the `nDefective <- nrow(subset(holeSize, holeDiameter_cm < lower_limit | holeDiameter_cm > upper_limit))` to find the number of deffective products for different tollerance limits.

*** =pre_exercise_code
```{r}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.
# 1. Analyse the distribution of the hole-sizes using the `hist()` function, and by playing around with `breaks` parameter. 



# 2. How many samples fall outside the 0.5cm tollerance limit and are therefore defective: assign your answer to `nDefective05`



# 3. How many samples fall outside the 0.25cm tollerance limit and are therefore defective: assign your answer to `nDefective025`



# 4. How many samples fall outside the 1cm tollerance limit and are therefore defective: assign your answer to `nDefective1`



# 5. View the values by printing them to the console output via the `script.R` file. Print the values here:



```

*** =solution
```{r}
hist(holeSize$holeDiameter_cm)
nDefective05 <- nrow(subset(holeSize, holeDiameter_cm > 10.5 | holeDiameter_cm < 9.5 ))
nDefective025 <- nrow(subset(holeSize, holeDiameter_cm > 10.25 | holeDiameter_cm < 9.75 ))
nDefective1 <- nrow(subset(holeSize, holeDiameter_cm < 9 | holeDiameter_cm > 11 ))
nDefective05
nDefective025
nDefective1
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the hole diameters of the samples to see what the distribution looks like.",
             incorrect_msg = "Make sure to draw the histogram for the hole diameters. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")
     
test_object("nDefective05", undefined_msg = "Make sure to define an object `nDefective05`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 0.5cm tollerace and assigned the result to `nDefective05`")     

test_object("nDefective025", undefined_msg = "Make sure to define an object `nDefective025`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 0.25cm tollerace and assigned the result to `nDefective025`") 

test_object("nDefective1", undefined_msg = "Make sure to define an object `nDefective1`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 1cm tollerace and assigned the result to `nDefective1`") 

test_output_contains("nDefective05", incorrect_msg = "You did not view the actual value `nDefective05`.")
test_output_contains("nDefective025", incorrect_msg = "You did not view the actual value `nDefective025`.")
test_output_contains("nDefective1", incorrect_msg = "You did not view the actual value `nDefective1`.")

success_msg("Correct! Now that we looked at the distribution of hole-sizes, and have an idea of the number of products that are not according to specificaion, we can decide which distribution best represent the hole-sizes resulting from the drilling machine.")
```



--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:8eaf7b9ebd
## Choosing the best distribution for the drill-hole sizes

Based on the histogram of the drill-hole sizes, which of the following distributions best represent the drill-hole size resulting from the drilling machine?

*** =instructions
* normal, 
* poisson and exponential, 
* power-law, or 
* uniform,

*** =hint
Have a look at the distribution that you generated in the previous exercise.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg_bad <- "Incorrect. You may need to redo the previous exercise. The data is randomly generated, so there may be chance that the distribution looks like something else by accident."
msg_success <- "That is correct! The distribution that best describes the resulting hole-size is the normal distribution. Next we need to find the key parameters for the distribution."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:f49344f14e
## Drill-hole size distribution parameters.

Which distribution parameters do we need to determine to model the hole-size as a normal distribution? 

*** =instructions

* min and max;
* mean or arrival rate; or
* mean and standard deviation;

*** =hint
Have a look at the class slides available from [this link](https://clickup.up.ac.za/bbcswebdav/pid-1016180-dt-content-rid-11418452_1/courses/ban313_s1_2017/Lecture_Week6.pdf).

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg_bad <- "Incorrect. That is not the necessary parameters for the normal distribution."
msg_success <- "That is correct! For a normal distribution we need the mean and standard deviation of the random variable"
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success))
```


--- type:NormalExercise lang:r xp:100 skills:1 key:54412012be
## Using the best distribution for the drill-hole sizes

To use the distribution, $X \sim \mathcal{N}(\mu, \sigma)$ we first need to estimate its key parameters, namely the mean $\mu$ and standard deviation $\sigma$ of the drill hole sizes. 
Thereafter we will emperically calculate the fraction of products that meet different specification limits using the `pnorm()` function.

The data that we will use to estimate the key parameters are available in the `holeSize` dataframe. Visit [this page](http://www.cyclismo.org/tutorial/R/probability.html) to find out more about using the `pnorm` function.

Hint, to assist in your answers, on a piece of paper draw a figure of the normal distribution of the hole-size and check where the lower and upper tollerance limits lie. Pay special attention that the tollerance limits may not neccisseraly be symmetrically around the mean, in which case you need to calculate the probability for each tollerance limit, and calculate the area between the upper and lowered limit. This is covered in Example~2.49 on page 93 in the [prescibed textbook](https://www.openintro.org/stat/textbook.php?stat_book=isrs).

*** =instructions

1. Estimate $\mu$ for the normal distribution and assign your answer to `meanHoleSize`. 
2. Estimate $\sigma$ for the normal distribution and assign your answer to `sdHoleSize`. 
3. Use the normal distribution propability function and determine the fraction of holes that will fall within a _0.5cm_ tollerance limit and assign your answer to `fWork05`.
4. Use the normal distribution propability function and determine the fraction of holes that will fall within a _0.75cm_ tollerance limit and assign your answer to `fWork075`.
5. View all the above values by printing them to the console output via the `script.R` file.

*** =hint

To calculate $\mu$ and $\sigma$, simply use `mean` and `sd` on `holeSize$holeDiameter_cm`. 

Visit [this page](http://www.cyclismo.org/tutorial/R/probability.html) to find out more about using the `pnorm` function. 

The `pnorm` function has three inputs, `q`, $\mu$ and $\sigma$. To calculate the fraction of products that fall within a lower and upper limit, you need to call `pnorm` twice: once by setting `q = lower_limit` and once by setting `q = upper_limit`. The function gives the probability of getting a value lower than `q`, so you have to then figure you which probability value to subtract from which to get the area between two limits. This is covered in Example~2.49 on page 93 in the [prescibed textbook](https://www.openintro.org/stat/textbook.php?stat_book=isrs).

*** =pre_exercise_code
```{r}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

# 1. Estimate mu for the normal distribution and assign your answer to `meanHoleSize`. 



# 2. Estimate sigma for the normal distribution and assign your answer to `sdHoleSize`. 



# 3. Use the normal distribution propability function and determine the fraction of holes that will fall within a 0.5cm tollerance limit and assign your answer to `fWork05`.



# 4. Use the normal distribution propability function and determine the fraction of holes that will fall within a 0.75cm tollerance limit and assign your answer to `fWork075`.



# 5. View all the above values by printing them to the console output via the `script.R` file.  Print the values here:

```

*** =solution
```{r}
meanHoleSize <- mean(holeSize$holeDiameter_cm)
sdHoleSize <- sd(holeSize$holeDiameter_cm)
fWork05 <- pnorm(10+0.5, mean = meanHoleSize, sd = sdHoleSize) - pnorm(10-0.5, mean = meanHoleSize, sd = sdHoleSize)
fWork075 <- pnorm(10+0.75, mean = meanHoleSize, sd = sdHoleSize) - pnorm(10-0.75, mean = meanHoleSize, sd = sdHoleSize)
meanHoleSize
sdHoleSize
fWork05
fWork075
```

*** =sct
```{r}
test_object("meanHoleSize", undefined_msg = "Make sure to define an object `meanHoleSize`.",
            incorrect_msg = "Make sure that you calculated the mean hole size correctly and assigned your answer to `meanHoleSize`")     

test_object("sdHoleSize", undefined_msg = "Make sure to define an object `sdHoleSize`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of hole size correctly and assigned your answer to `sdHoleSize`")     

test_object("fWork05", undefined_msg = "Make sure to define an object `fWork05`.",
            incorrect_msg = "Make sure that you calculated the fraction of holes that are _within_ the 0.5cm tollerace correctly and assigned the result to `fWork05`") 

test_object("fWork075", undefined_msg = "Make sure to define an object `fWork075`.",
            incorrect_msg = "Make sure that you calculated the fraction of holes that are _within_ the 0.75cm tollerace correctly and assigned the result to `fWork075`") 

test_output_contains("meanHoleSize", incorrect_msg = "You did not view the actual value `meanHoleSize`.")
test_output_contains("sdHoleSize", incorrect_msg = "You did not view the actual value `sdHoleSize`.")
test_output_contains("fWork05", incorrect_msg = "You did not view the actual value `fWork05`.")
test_output_contains("fWork075", incorrect_msg = "You did not view the actual value `fWork075`.")

success_msg("Correct! We have no successfully fitted a normal distribution to the hole size and used the distribution to emperically calculate the fraction of products that are within specification.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:acbbc34771
## Predicting softdrink orders for the next day

We are responsible for inventory management for a softdrink company.
Due to a strike, the company cannot produce softdrink. The company is afraid that it may not be able to fullfill customer orders for the next day.
Human resources believe that the strike will be this evening, and we therefore need to determine if we have enough inventory for tomorrow's orders.
Orders are only placed by our clients in the morning and delivered on the same day.
Our clients are unaware of the strike, and management has no intention of informing them of it.
We currently have 210 units of the softdrink in stock.

Management wishes to know what probabilty is of running out of stock during the next day.
Depending on the probability they can either import soft-drink from another manufacturing plant via expensive air-delivery, or decide to take their chances and wait for the strike to end and resume manufacturing.

The sales data for the previous 30 days' orders are available in the `clienOrders` dataframe, which consists of the day and the number of units ordered on that day by our clients.

For the first part of this question, analyse the distribution of the number orders per day and identify the distribution that best describes this value. 
In the following parts we will find the key parameters of the distribution and use the distribution function to answer some the above question.

*** =instructions

1. Analyse the distribution of the orders per day using the `hist()` function and by playing around with `breaks` parameter. 
2. How many times over the last 30 days did customers order _less_ than 210 units: assign your answer to `less210`.
3. View the values by printing them to the console output via the `script.R` file. Print the values here:

*** =hint

Use `hist(clienOrders$ordersPerDay)` to analyse the distribution and see which of the four distributions

* normal, 
* poisson and exponential, 
* power-law, or 
* uniform,

it most closely represents.

Use the `less210 <- nrow(subset(clienOrders, ordersPerDay < lower_limit))` to find the number of days that less than the specified number of products were ordered.

*** =pre_exercise_code
```{r}
clienOrders <- data.frame(day = 1:30, ordersPerDay <- round(runif(30, 150, 250), 0))
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.
# 1. Analyse the distribution of the orders per day using the `hist()` function and by playing around with `breaks` parameter. 



# 2. How many times over the last 30 days did customers order _less_ than 210 units: assign your answer to `less210`.



# 3. View the values by printing them to the console output via the `script.R` file. Print the values here:



```

*** =solution
```{r}
hist(clienOrders$ordersPerDay)
less210 <- nrow(subset(clienOrders, ordersPerDay < 210))
less210
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the number of orders per day to see what the distribution looks like.",
             incorrect_msg = "Make sure to draw the histogram for the number of orders per day. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")
     
test_object("less210", undefined_msg = "Make sure to define an object `less210`.",
            incorrect_msg = "Make sure that you calculated the number of times that clients ordered less than 210 units and assigned the result to `nDefective05`")     

test_output_contains("less210", incorrect_msg = "You did not view the actual value `less210`.")

success_msg("Correct! Now that we looked at the distribution of orders per day, and have an idea of the number of times that our clients ordered less units than what is in our inventory, we can decide which distribution best represent the number of units ordered and predict the probability of running out of stock tomorrow.")
```