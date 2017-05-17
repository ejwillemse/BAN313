---
title_meta  : Case study 11
title       : Case study 11 - Sensitivity analysis on queueing models (Part II)
description : "In this case study we will do more formal sensitivity analysis
the basic ATM queueing model."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

**Read the below instructions carefully.**

In this case study we will do formal sensitivity analysis on the ATM queueing
model of Case study 10.

Before continuing with the case study, first complete [Case study 10 - Sensitivity analysis on queueing models (Part I)](https://campus.datacamp.com/courses/industrial-analysis-using-r/11779?ex=1).

Recall that for the case customers arrive at an ATM and perform a number of ATM
transactions. After completing their transactions, the customer takes their card
and leaves. The next customer can then use the ATM. When the ATM is in use, the
next customers have to wait in-line until it is their time to use the ATM.

A key measurement of such a queueing system is how long customers wait in-line,
before it is their turn to use the ATM. If customers wait too long, the system
should be improved. Either be reducing the time that customers spend completing
transactions or by increasing the number ATMs.

Is this chapter we will perform sensitivity analysis on the model. The goal of
sensitivity analysis it to test how changes in model input affects changes in
model output. By doing so we can see which input parameters are critical for our
analysis, and spend extra effort to ensure that they are accurate, such as
obtaining more data for the random variable.

In this chapter we will first analyse process data and identify distributions to
model subprocesses and calculate their parameters. Thereafter we will test how
changing the distribution parameters affects the model outputs.

In this lab we do the following:

1. Analyse process data and identify distributions to model subprocesses and calculate distribution parameters.
2. Use bootstrapping to get parameter intervals.
3. Perform one-factor-at-a-time sensitivity analysis to see which parameters are most sensitive.
4. Test variable factor combinations to see which factors are the most sensitive.

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip the each questions' background information. The question backgrounds contain valuable information to assist you in completing the questions.

To continue with this chapter confirm the following:

*** =instructions

* I have read **ALL** the instructions on this page carefully.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_bad <- "Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."

msg_success <- "Let's get started with the Lab. Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."
test_mc(correct = 1, feedback_msgs = c(msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:592f5b5fae
## Analysing the data

For the first step in this case study we will analyse customer arrival and service data to see what distribution the data follows, formally test the distributions using the $\chi^2$ goodness of fit test. In the following question we will calculate the key distribution parameters.

The available data has already been loaded as the `serviceData` and `arrivalData` dataframes. To view the data simply type `serviceData` or `arrivalData` in the console.

To complete this question.

*** =instructions

1. View the `serviceData` and decide which distribution the variable follows. Thereafter conduct a $\chi^2$ goodness of fit test and report on the $p$-value of the test. Assign your answer to `p_val_service`. Hint: assign the $\chi^2$ goodness of fit test to a variable, for example `fitResults <- chisq.test(...)`. View the results of the test by typing `fitResults` in the console. The $p$-value can then be retrieved as `p_val_service <- fitResults$p.value`
2. For the arrival, calculate the inter-arrival time of customers, which is the variable that we are after. Use the `diff` command to calculate the inter-arrival times and assign your answer to `inter_times`.
3. View `inter_times` and decide which distribution the variable follows. Thereafter conduct a $\chi^2$ goodness of fit test and report on the $p$-value of the test. Assign your answer to `p_val_arrive`. Hint: the $p$-value can be obtained using the same method as Part 1.

*** =pre_exercise_code
```{r}
set.seed(31)
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)
```

*** =sample_code
```{r}
# Part 1



# Part 2



# Part 3



```

*** =solution
```{r}
h1 <- hist(serviceData$serviceTime_min)
null.prob <- diff(pnorm(h1$breaks, mean = mean(serviceData$serviceTime_min), sd = sd(serviceData$serviceTime_min)))
fitResults1 <- chisq.test(h1$counts, p = null.prob, rescale.p = TRUE, simulate.p.value = TRUE)
p_val_service <- fitResults1$p.value

inter_times <- diff(arrivalDataDataCa$arrivalTime_minFromMidn)
h2 <- hist(inter_times)
null.prob <- diff(pexp(h2$breaks, rate = 1/mean(inter_times)))
fitResults2 <- chisq.test(h2$counts, p = null.prob, rescale.p = TRUE, simulate.p.value = TRUE)
p_val_arrive <- fitResults2$p.value
```

*** =sct
```{r}
test_object("p_val_service", undefined_msg = "Make sure to define an object `p_val_service`.",
incorrect_msg = "Something went wrong in calculating `p_val_service`.
Make sure you called the `chisq.test` function correctly. Refer to the previous chapters on the goodness-of-fit test if necessary and see the question hints.")

test_object("inter_times", undefined_msg = "Make sure to define an object `inter_times`.",
incorrect_msg = "Something went wrong in calculating `inter_times`.
Make sure that you calculated the inter-arrival times of consecutive customers. It is quite easy to do so using the `diff` function.")

test_object("p_val_arrive", undefined_msg = "Make sure to define an object `p_val_arrive`.",
incorrect_msg = "Something went wrong in calculating `p_val_arrive`.
Make sure you called the `chisq.test` function correctly. Refer to the previous chapters on the goodness-of-fit test if necessary and see the question hints. Recall that for the exponential distribution we need to calculate the mean arrival rate for the `pexp` function, which is equal to $\frac{1}{\text{mean inter arrival time}}$")

success_msg("Correct!
The service time seems to follow a normal distribution whereas the inter-arrival time seems to follow an exponential distribution.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:aec6c4a986
## Calculating the distribution parameters

In the previous question we saw that the service time follows a normal distribution and the inter-arrival time follows an exponential distribution.

To model the two variables we need to calculate the key parameters of the distributions.
Although we have already done so for the goodness-of-fit test we will redo the calculates in this question.

*** =instructions

1. Using the available `serviceData` data.frame, calculate and view the sample mean and standard deviation of the service time and assign your answer to `serviceMean` and `serviceSD`.
2. Using the available `arrivalData` data.frame, calculate and view the mean arrival rate and assign your answer to `arrivalMean`. Remember to first calculate the inter-arrival times.

*** =pre_exercise_code
```{r}
set.seed(31)
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)
```

*** =sample_code
```{r}
# Part 1



# Part 2



```

*** =solution
```{r}
serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))
```

*** =sct
```{r}
test_object("serviceMean", undefined_msg = "Make sure to define an object `serviceMean`.",
incorrect_msg = "Something went wrong in calculating `serviceMean`.")

test_object("serviceSD", undefined_msg = "Make sure to define an object `serviceSD`.",
incorrect_msg = "Something went wrong in calculating `serviceSD`.")

test_object("arrivalMean", undefined_msg = "Make sure to define an object `arrivalMean`.",
incorrect_msg = "Something went wrong in calculating `arrivalMean`. Make sure to first calculate the inter-arrival times of customers. Thereafter calculate the mean arrival rate which is equal to 1 divided by the mean inter-arrival times.")

success_msg("Correct!
We now have all the required parameter values to simulate the queueing model.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:aa396e8332
## First simulation

With the `serviceData` and `arrivalData` already available in the workspace, the parameter values for the service time and inter-arrival rate distributions can be calculated using the following code:

```
serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))
```

A function has been given that simulates the mean waiting time for a specified number of customers, depending on the above distribution parameters.

Use the above parameters and the provided simulation to conduct 1000 simulations of 100 customers arriving at an ATM and capture the following:

*** =instructions

1. For each simulation, assign the mean service time to `meanWaitingTimes`.
2. View a histogram of the `meanWaitingTimes`.
3. Calculate and view the mean of the mean waiting of the 1000 simulations and assign your answer to `meanWaitingSimulated`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# Ignore this code
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)

# ATM Simulation model, take note of its input parameters
atmSimulation <- function(arrivalRate, serviceTimeMean, serviceTimeSD, nCustomers)
{
  arrivalTime <- cumsum(rexp(nCustomers, arrivalRate))
  serviceTime <- rnorm(nCustomers, serviceTimeMean, serviceTimeSD)

  waitingTime = rep(NA, nCustomers) # time that customer waited in-line for the ATM
  leaveTime = rep(NA, nCustomers) # time that customer is done at ATM

  # Service, arrival, and leaving time of first customer
  waitingTime[1] = 0
  leaveTime[1] = arrivalTime[1] + serviceTime[1]

  for (i in 2:nCustomers)
  {
    if (arrivalTime[i] >= leaveTime[i - 1])
    {
      waitingTime[i] = 0
      leaveTime[i] = arrivalTime[i] + serviceTime[i]
    }else{
      waitingTime[i] = leaveTime[i - 1] - arrivalTime[i]
      leaveTime[i] = leaveTime[i - 1] + serviceTime[i]
    }
  }
  return(mean(waitingTime))
}

# Part 1

serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  ...
}


# Part 2



# Part 3


```

*** =solution
```{r}
serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = arrivalMean, serviceTimeMean = serviceMean, serviceTimeSD = serviceSD, nCustomers = 100)
}
hist(meanWaitingTimes)
meanWaitingSimulated <- mean(meanWaitingTimes)
```

*** =sct
```{r}
test_object("meanWaitingTimes", undefined_msg = "Make sure to define an object `meanWaitingTimes`.",
incorrect_msg = "Something went wrong in calculating `meanWaitingTimes`. Make sure you assigned the correct parameter values to the `atmSimulation` function. Assign the calculated values directly to the function. To not copy them from the console screen.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of of the simulation output.",
incorrect_msg = "Draw a histogram of the simulation output.")

test_object("meanWaitingSimulated", undefined_msg = "Make sure to define an object `meanWaitingSimulated`.",
incorrect_msg = "Calculate the mean of the mean waiting times.")

success_msg("Correct!
By using the estimated distribution parameters we can simulate the queueing process and calculate the expected mean queuing time of 100 customers. The only problem is that we used the estimated distribution parameters, since the actual distribution parameters are unknown. To see what impact that may have, re-run the model as-is a few times and view `meanWaitingSimulated`. To do so simply click on the `submit answers` button again. With the re-run a new sample is loaded, which still comes from the same process. Note how `meanWaitingSimulated` changes each time we run the model. Even if we increased the number of simulation to 100000, the `meanWaitingSimulated` will still change since our sample data changes, and when the sample data changes our distribution parameter estimates change. Given that the distribution parameter estimates are most likely wrong, since they are based on samples, how can we be confident in our model results?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c91d7c054c
## Simulation using more accurate data

One way to improve our model accuracy is by getting better data.
For this exercise we will run the simulation but calculate the distribution parameters using 10000 samples, instead of 100.
You don't have to code anything. Simply run the model a few times and note by how much the simulation results change between runs.

*** =instructions

1. Run the model a few times and note how the simulation results change.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# Ignore this code
serviceTime_min <- rnorm(10000, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(10000, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)

# ATM Simulation model, take note of its input parameters
atmSimulation <- function(arrivalRate, serviceTimeMean, serviceTimeSD, nCustomers)
{
  arrivalTime <- cumsum(rexp(nCustomers, arrivalRate))
  serviceTime <- rnorm(nCustomers, serviceTimeMean, serviceTimeSD)

  waitingTime = rep(NA, nCustomers) # time that customer waited in-line for the ATM
  leaveTime = rep(NA, nCustomers) # time that customer is done at ATM

  # Service, arrival, and leaving time of first customer
  waitingTime[1] = 0
  leaveTime[1] = arrivalTime[1] + serviceTime[1]

  for (i in 2:nCustomers)
  {
    if (arrivalTime[i] >= leaveTime[i - 1])
    {
      waitingTime[i] = 0
      leaveTime[i] = arrivalTime[i] + serviceTime[i]
    }else{
      waitingTime[i] = leaveTime[i - 1] - arrivalTime[i]
      leaveTime[i] = leaveTime[i - 1] + serviceTime[i]
    }
  }
  return(mean(waitingTime))
}

# Part 1

serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = arrivalMean, serviceTimeMean = serviceMean, serviceTimeSD = serviceSD, nCustomers = 100)
}
hist(meanWaitingTimes)
meanWaitingSimulated <- mean(meanWaitingTimes)
meanWaitingSimulated

```

*** =solution
```{r}
# Ignore this code
serviceTime_min <- rnorm(10000, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(10000, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)

# ATM Simulation model, take note of its input parameters
atmSimulation <- function(arrivalRate, serviceTimeMean, serviceTimeSD, nCustomers)
{
  arrivalTime <- cumsum(rexp(nCustomers, arrivalRate))
  serviceTime <- rnorm(nCustomers, serviceTimeMean, serviceTimeSD)

  waitingTime = rep(NA, nCustomers) # time that customer waited in-line for the ATM
  leaveTime = rep(NA, nCustomers) # time that customer is done at ATM

  # Service, arrival, and leaving time of first customer
  waitingTime[1] = 0
  leaveTime[1] = arrivalTime[1] + serviceTime[1]

  for (i in 2:nCustomers)
  {
    if (arrivalTime[i] >= leaveTime[i - 1])
    {
      waitingTime[i] = 0
      leaveTime[i] = arrivalTime[i] + serviceTime[i]
    }else{
      waitingTime[i] = leaveTime[i - 1] - arrivalTime[i]
      leaveTime[i] = leaveTime[i - 1] + serviceTime[i]
    }
  }
  return(mean(waitingTime))
}

serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = arrivalMean, serviceTimeMean = serviceMean, serviceTimeSD = serviceSD, nCustomers = 100)
}
hist(meanWaitingTimes)
meanWaitingSimulated <- mean(meanWaitingTimes)
meanWaitingSimulated
```

*** =sct
```{r}
success_msg("Run the model a few times, the more the better, and see by how much the model output difference. You'll notice the model output is much more consistent. The lesson is that the more accurate our input data is, the more accurate the model results will be. The opposite also holds, if we give our model garbage data, the model will give is garbage output, aka garbage-in-garbage-out. Unfortunately we don't always have the luxury to get 100000 observations for each variable. Key to model development is to see where our model is weak, and invest effort in improving the data quality of the critical input parameters. We can use sensitivity analysis to do so.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:dec1c30a07
## Finding distribution parameter ranges

In the previous chapter we informally tested our model's sensitivity towards its distribution input-parameters by varying each by a certain percentage.
For sensitivity analysis we want to get more accurate parameter ranges.
To do so we can use the bootstrapping.

For this exercise we will use the bootstrapping method to calculate 99% confidence intervals for the distribution parameters, as well the expected values for the intervals. Similar the previous exercises the

*** =instructions

1. Use the bootstrapping method to calculate and view a 99% confidence interval and the expected value for the arrival rate of customers. Use 10000 bootstrap simulations and assign the interval to `arrivalInterval` and the expected value to `meanArrival`. Simply assign the results of the `quantile()` function to `arrivalInterval` as follows: `arrivalInterval <- quantile()`.
2. Use the bootstrapping method to calculate and view 99% confidence intervals the expected values for the mean and standard deviation service times of customers. Use 10000 bootstrap simulations and assign the intervals to `meanServiceInterval` and `sdServiceInterval`, and the expected values to `meanMeanService` and `meanSdService`. Calculate the both the mean and standard deviation using the bootstrapping samples.
