---
title_meta  : Case study 6
title       : Case study 6 - Different applications of probability distributions
description : In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. The distribitions that we will look at are normal, poisson and exponential, power-law, and uniform. After eye-balling the applicable distribution we will find the appropraite distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.

--- type:NormalExercise lang:r xp:0 skills:1 key:ec139ff825
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
