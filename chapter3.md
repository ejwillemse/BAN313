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
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_bad, msg_success))
```
