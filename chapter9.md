---
title_meta: 'Case study 11'
title: 'Case study 11 - Sensitivity analysis on queueing models (Part II)'
description: 'In this case study we will do more formal sensitivity analysis on the basic ATM queueing model.'
---

## Background

```yaml
type: MultipleChoiceExercise
key: 8eaf7b9ebd6
lang: r
xp: 50
skills: 1
```

**Read the below instructions carefully.**

In this case study we will do formal sensitivity analysis on the ATM queueing
model of Case study 10.

**Before continuing with this chapter, complete all previous chapters in this datacamp course.**

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

`@possible_answers`
* I have read **ALL** the instructions on this page carefully.

`@hint`


`@pre_exercise_code`
```{r}

```

`@sct`
```{r}
msg_bad <- "Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."

msg_success <- "Let's get started with the Lab. Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."
test_mc(correct = 1, feedback_msgs = c(msg_success))
```

---

## Analysing the data

```yaml
type: NormalExercise
key: 592f5b5fae
lang: r
xp: 100
skills: 1
```

For the first step in this case study we will analyse customer arrival and service data to see what distribution the data follows, formally test the distributions using the $\chi^2$ goodness of fit test. In the following question we will calculate the key distribution parameters.

The available data has already been loaded as the `serviceData` and `arrivalData` dataframes. To view the data simply type `serviceData` or `arrivalData` in the console.

To complete this question.

`@instructions`
- View the `serviceData` and decide which distribution the variable follows. Thereafter conduct a $\chi^2$ goodness of fit test and report on the $p$-value of the test. Assign your answer to `p_val_service`. Hint: assign the $\chi^2$ goodness of fit test to a variable, for example `fitResults <- chisq.test(...)`. View the results of the test by typing `fitResults` in the console. The $p$-value can then be retrieved as `p_val_service <- fitResults$p.value`
- For the arrival, calculate the inter-arrival time of customers, which is the variable that we are after. Use the `diff` command to calculate the inter-arrival times and assign your answer to `inter_times`.
- View `inter_times` and decide which distribution the variable follows. Thereafter conduct a $\chi^2$ goodness of fit test and report on the $p$-value of the test. Assign your answer to `p_val_arrive`. Hint: the $p$-value can be obtained using the same method as Part 1.

`@hint`


`@pre_exercise_code`
```{r}
set.seed(31)
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)
```

`@sample_code`
```{r}
# Part 1



# Part 2



# Part 3



```

`@solution`
```{r}
h1 <- hist(serviceData$serviceTime_min)
null.prob <- diff(pnorm(h1$breaks, mean = mean(serviceData$serviceTime_min), sd = sd(serviceData$serviceTime_min)))
fitResults1 <- chisq.test(h1$counts, p = null.prob, rescale.p = TRUE, simulate.p.value = TRUE)
p_val_service <- fitResults1$p.value

inter_times <- diff(arrivalData$arrivalTime_minFromMidn)
h2 <- hist(inter_times)
null.prob <- diff(pexp(h2$breaks, rate = 1/mean(inter_times)))
fitResults2 <- chisq.test(h2$counts, p = null.prob, rescale.p = TRUE, simulate.p.value = TRUE)
p_val_arrive <- fitResults2$p.value
```

`@sct`
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

---

## Calculating the distribution parameters

```yaml
type: NormalExercise
key: aec6c4a986
lang: r
xp: 100
skills: 1
```

In the previous question we saw that the service time follows a normal distribution and the inter-arrival time follows an exponential distribution.

To model the two variables we need to calculate the key parameters of the distributions.
Although we have already done so for the goodness-of-fit test we will redo the calculates in this question.

`@instructions`
- Using the available `serviceData` data.frame, calculate and view the sample mean and standard deviation of the service time and assign your answer to `serviceMean` and `serviceSD`.
- Using the available `arrivalData` data.frame, calculate and view the mean arrival rate and assign your answer to `arrivalMean`. Remember to first calculate the inter-arrival times.

`@hint`


`@pre_exercise_code`
```{r}
set.seed(31)
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)
```

`@sample_code`
```{r}
# Part 1



# Part 2



```

`@solution`
```{r}
serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))
```

`@sct`
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

---

## First simulation

```yaml
type: NormalExercise
key: aa396e8332
lang: r
xp: 100
skills: 1
```

With the `serviceData` and `arrivalData` already available in the workspace, the parameter values for the service time and inter-arrival rate distributions can be calculated using the following code:

```
serviceMean <- mean(serviceData$serviceTime_min)
serviceSD <- sd(serviceData$serviceTime_min)
arrivalMean <- 1/mean(diff(arrivalData$arrivalTime_minFromMidn))
```

A function has been given that simulates the mean waiting time for a specified number of customers, depending on the above distribution parameters.

Use the above parameters and the provided simulation to conduct 1000 simulations of 100 customers arriving at an ATM and capture the following:

`@instructions`
- For each simulation, assign the mean service time to `meanWaitingTimes`.
- View a histogram of the `meanWaitingTimes`.
- Calculate and view the mean of the mean waiting of the 1000 simulations and assign your answer to `meanWaitingSimulated`.

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
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

`@solution`
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
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = arrivalMean, serviceTimeMean = serviceMean, serviceTimeSD = serviceSD, nCustomers = 100)
}
hist(meanWaitingTimes)
meanWaitingSimulated <- mean(meanWaitingTimes)
```

`@sct`
```{r}
test_object("meanWaitingTimes", undefined_msg = "Make sure to define an object `meanWaitingTimes`.",
incorrect_msg = "Something went wrong in calculating `meanWaitingTimes`. Make sure you assigned the correct parameter values to the `atmSimulation` function. Assign the calculated values directly to the function. To not copy them from the console screen.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of of the simulation output.",
incorrect_msg = "Draw a histogram of the simulation output.")

test_object("meanWaitingSimulated", undefined_msg = "Make sure to define an object `meanWaitingSimulated`.",
incorrect_msg = "Calculate the mean of the mean waiting times.")

success_msg("Correct!
By using the estimated distribution parameters we can simulate the queueing process and calculate the expected mean queuing time of 100 customers. The only problem is that we used the estimated distribution parameters, since the actual distribution parameters are unknown. To see what impact that may have, copy and re-run the model in _RStudio_ as-is a few times and view `meanWaitingSimulated`. Note how `meanWaitingSimulated` changes each time we run the model. Even if we increased the number of simulation to 100000, the `meanWaitingSimulated` will still change since our sample data changes, and when the sample data changes our distribution parameter estimates change. Given that the distribution parameter estimates are most likely wrong, since they are based on samples, how can we be confident in our model results?")
```

---

## Simulation using more accurate data

```yaml
type: NormalExercise
key: c91d7c054c
lang: r
xp: 100
skills: 1
```

One way to improve our model accuracy is by getting better data.
For this exercise we will run the simulation but calculate the distribution parameters using 10000 samples, instead of 100.
You don't have to code anything. Simply run the model a few times and note by how much the simulation results change between runs.

`@instructions`
- Run the model a few times and note how the simulation results change.

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
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

`@solution`
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

`@sct`
```{r}
success_msg("Copy and run the model as-is in _RStudio_ a few times, the more the better, and see by how much the model output changes. You'll notice the model output is much more consistent. The lesson is that the more accurate our input data is, the more accurate the model results will be. The opposite also holds, if we give our model garbage data, the model will give us garbage output, aka garbage-in-garbage-out. Unfortunately we don't always have the luxury to get 100000 observations for each variable. Key to model development is to see where our model is weak, and invest effort in improving the data quality of the critical input parameters. We can use sensitivity analysis to do so.")
```

---

## Finding distribution parameter ranges

```yaml
type: NormalExercise
key: dec1c30a07
lang: r
xp: 100
skills: 1
```

In the previous chapter we informally tested our model's sensitivity towards its distribution input-parameters by varying each by a certain percentage.
For sensitivity analysis we want to get more accurate parameter ranges.
To do so we can use the bootstrapping.

For this exercise we will use the bootstrapping method to calculate 99% confidence intervals for the distribution parameters, as well as the expected values for the intervals. Similar to the previous exercises the sample data is available from the `arrivalData` and `serviceData` dataframes. Partial bootstrapping code has also been provided.

`@instructions`
- Use the bootstrapping method to calculate and view a 99% confidence interval and the expected value for the arrival rate of customers. Use 10000 bootstrap simulations and assign the interval to `arrivalInterval` and the expected value to `meanArrival`. Simply assign the results of the `quantile()` function to `arrivalInterval` as follows: `arrivalInterval <- quantile()`.
- Use the bootstrapping method to calculate and view 99% confidence intervals the expected values for the mean and standard deviation service times of customers. Use 10000 bootstrap simulations and assign the intervals to `meanServiceInterval` and `sdServiceInterval`, and the expected values to `meanMeanService` and `meanSdService`. Calculate both the mean and standard deviation using the bootstrapping samples.

`@hint`


`@pre_exercise_code`
```{r}
serviceTime_min <- rnorm(100, 1, 0.2)
serviceData <- data.frame(serviceTime_min)

arrivalTime_minFromMidn <- cumsum(rexp(100, 1.5)) + 60*8
arrivalData <- data.frame(arrivalTime_minFromMidn)
```

`@sample_code`
```{r}
# Incomplete bootstrapping code

inter_times <- diff(arrivalData$arrivalTime_minFromMidn)

arrivalMean <- rep(NA, 10000)
for (i in 1:10000)
{
    arrivalMean[i] <- 1/mean(sample(...))
}
arrivalInterval <- quantile(arrivalMean, p = c(0.005, 0.995))
meanArrival <- ...


meanService <- rep(NA, 10000)
meanSD <- rep(NA, 10000)
for (i in 1:10000)
{
  s <- sample(...)
  meanService[i] <- mean(s)
  meanSD[i] <- sd(s)
}
meanServiceInterval <- quantile(meanService, p = c(0.005, 0.995))
sdServiceInterval <- quantile(meanSD, p = c(0.005, 0.995))

meanMeanService <- ...
meanSdService <- ...

# View the results here

```

`@solution`
```{r}
inter_times <- diff(arrivalData$arrivalTime_minFromMidn)

arrivalMean <- rep(NA, 10000)
for (i in 1:10000)
{
  arrivalMean[i] <- 1/mean(sample(inter_times, size = length(inter_times), replace = TRUE))
}
arrivalInterval <- quantile(arrivalMean, p = c(0.005, 0.995))
meanArrival <- mean(arrivalMean)

meanService <- rep(NA, 10000)
meanSD <- rep(NA, 10000)
for (i in 1:10000)
{
  s <- sample(serviceData$serviceTime_min, size = length(serviceData$serviceTime_min), replace = TRUE)
  meanService[i] <- mean(s)
  meanSD[i] <- sd(s)
}
meanServiceInterval <- quantile(meanService, p = c(0.005, 0.995))
sdServiceInterval <- quantile(meanSD, p = c(0.005, 0.995))

meanMeanService <- mean(meanService)
meanSdService <- mean(meanSD)
```

`@sct`
```{r}
test_object("arrivalInterval", undefined_msg = "Make sure to define an object `arrivalInterval`.",
incorrect_msg = "Something went wrong in calculating `arrivalInterval`. Make sure that you conducted the bootstrapping correctly and calculated the mean arrival rate for each bootstrap simulation. Also make sure that you assigned `arrivalInterval` as per the instructions.")

test_object("meanArrival", undefined_msg = "Make sure to define an object `meanArrival`.",
incorrect_msg = "Something went wrong in calculating `meanArrival`. Make sure that you calculated the mean for over all the bootstrap simulation means, and _not_ on the interval.")

test_object("meanServiceInterval", undefined_msg = "Make sure to define an object `meanServiceInterval`.",
incorrect_msg = "Something went wrong in calculating `meanServiceInterval`. Make sure that you conducted the bootstrapping correctly. Also make sure that you assigned `meanServiceInterval` as per the instructions.")

test_object("sdServiceInterval", undefined_msg = "Make sure to define an object `sdServiceInterval`.",
incorrect_msg = "Something went wrong in calculating `sdServiceInterval`. Make sure that you conducted the bootstrapping correctly. Also make sure that you assigned `sdServiceInterval` as per the instructions.")

test_object("meanMeanService", undefined_msg = "Make sure to define an object `meanMeanService`.",
incorrect_msg = "Something went wrong in calculating `meanMeanService`. Make sure that you calculated the mean for over all the bootstrap simulation means, and _not_ on the interval.")

test_object("meanSdService", undefined_msg = "Make sure to define an object `meanSdService`.",
incorrect_msg = "Something went wrong in calculating `meanSdService`. Make sure that you calculated the mean for over all the bootstrap simulation means, and _not_ on the interval.")

success_msg("Correct!
By using the bootstrapping method we can calculate intervals for the distribution parameters.
The intervals give us a more realistic range over which to conduct the sensitivity analysis.
In the next question we will analyse the simulation model results over the minimum, expected and maximum values calculated using the bootstrapping methods.
")
```

---

## Single factor sensitivity analysis for the arrival rate

```yaml
type: NormalExercise
key: 53546d675a
lang: r
xp: 100
skills: 1
```

In the previous question we calculated parameter value ranges using the bootstrapping method. In this question we will consider the arrival rate and change its values to the minimum range value, the expected value and the maximum range value. We will then see by how much the mean waiting time changes between the values. All other parameter values will be set equal to their expected values from the bootstrapping method of the previous question.

Assume that the arrival rate interval and expected value are as follow:

* (1.17, 1.47, 1.91)

and that the expected service time and standard deviation are:

* (1.03, 0.21)

The simulation model to calculate the queueing model has been provided.

`@instructions`
- Use the available code and repeat the ATM simulation with an arrival rate of 1.17 customers per minute. Assign the mean of the mean waiting times to `meanSimWaiting_low`.
- Repeat the ATM simulation with an arrival rate of 1.47 customers per minute. Assign the mean of the mean waiting times to `meanSimWaiting_expected`.
- Repeat the ATM simulation with an arrival rate of 1.91 customers per minute. Assign the mean of the mean waiting times to `meanSimWaiting_high`.
- View and compare `meanSimWaiting_low`, `meanSimWaiting_expected` and `meanSimWaiting_high` and note by how much the mean waiting time increased as the arrival rate increased.

`@hint`


`@pre_exercise_code`
```{r}



```

`@sample_code`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.17, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
hist(meanWaitingTimes)
meanSimWaiting_low <- mean(meanWaitingTimes)
meanSimWaiting_low

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_expected <- mean(meanWaitingTimes)
meanSimWaiting_expected

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_high <- mean(meanWaitingTimes)
meanSimWaiting_high

```

`@solution`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.17, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_low <- mean(meanWaitingTimes)

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_expected <- mean(meanWaitingTimes)

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.91, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_high <- mean(meanWaitingTimes)
```

`@sct`
```{r}
test_object("meanSimWaiting_low", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_low`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_expected", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_expected`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_high", undefined_msg = "Make sure to define an object `meanSimWaiting_high`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_high`. Make sure to set the simulation input parameters to the correct value.")

success_msg("Correct! We have now tested the model's sensitivity over realistic ranges of the mean arrival rate. Note by how much the mean waiting time changed from the low to high values. The range of the waiting time is about 15 minutes. In the next question we are going to repeat the analysis for the mean service time, and thereafter for the standard deviation of the service time.")
```

---

## Single factor sensitivity analysis for the mean service time

```yaml
type: NormalExercise
key: cebe14976c
lang: r
xp: 100
skills: 1
```

In this question we will consider the mean service time and change its values to the minimum range value, the expected value and the maximum range value. We will then see by how much the mean waiting time changes between the values. All other parameter values will be set equal to their expected values from the bootstrapping method of the previous question.

Assume that the mean service time interval and expected value are as follow:

* (0.97, 1.03, 1.08)

and that the expected arrival rate and standard deviation of service time are:

* (1.47, 0.21)

The simulation model to calculate the queueing model has been provided.

`@instructions`
- Use the available code and repeat the ATM simulation with a mean service time of 0.97 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_low`.
- Repeat the ATM simulation with a mean service time of 1.03 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_expected`.
- Repeat the ATM simulation with a mean service time of 1.08 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_high`.
- View and compare `meanSimWaiting_low`, `meanSimWaiting_expected` and `meanSimWaiting_high` and note by how much the mean waiting time increased as the mean service time increased.

`@hint`


`@pre_exercise_code`
```{r}



```

`@sample_code`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 0.97, serviceTimeSD = 0.21, nCustomers = 100)
}
hist(meanWaitingTimes)
meanSimWaiting_low <- mean(meanWaitingTimes)
meanSimWaiting_low

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_expected <- mean(meanWaitingTimes)
meanSimWaiting_expected

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_high <- mean(meanWaitingTimes)
meanSimWaiting_high

```

`@solution`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 0.97, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_low <- mean(meanWaitingTimes)

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_expected <- mean(meanWaitingTimes)

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.08, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_high <- mean(meanWaitingTimes)
```

`@sct`
```{r}
test_object("meanSimWaiting_low", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_low`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_expected", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_expected`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_high", undefined_msg = "Make sure to define an object `meanSimWaiting_high`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_high`. Make sure to set the simulation input parameters to the correct value.")

success_msg("Correct! We have now tested the model's sensitivity over realistic ranges of the mean service time. Note by how much the mean waiting time changed from the low to high values. The range of the waiting time is about 6 minutes, whereas it was 15 minutes when we changed the arrival rate. This is already different from our analysis from the previous chapter where we used a much less vigoures approach in chosing our ranged. In the next question we are going to repeat the analysis for the standard deviation of the service time.")
```

---

## Single factor sensitivity analysis for the standard deviation of service time

```yaml
type: NormalExercise
key: e9409a1d22
lang: r
xp: 100
skills: 1
```

In this question we will consider the service time standard deviation and change its values to the minimum range value, the expected value and the maximum range value. We will then see by how much the mean waiting time changes between the values. All other parameter values will be set equal to their expected values from the bootstrapping method of the previous question.

Assume that the service time standard deviation interval and expected value are as follow:

* (0.17, 0.21, 0.24)

and that the expected arrival rate and mean service time are:

* (1.47, 1.03)

The simulation model to calculate the queueing model has been provided.

`@instructions`
- Use the available code and repeat the ATM simulation with service time standard deviation of 0.17 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_low`.
- Repeat the ATM simulation with service time standard deviation of 0.21 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_expected`.
- Repeat the ATM simulation with service time standard deviation of 0.24 minutes. Assign the mean of the mean waiting times to `meanSimWaiting_high`.
- View and compare `meanSimWaiting_low`, `meanSimWaiting_expected` and `meanSimWaiting_high` and note by how much the mean waiting time increased as the mean service time increased.

`@hint`


`@pre_exercise_code`
```{r}



```

`@sample_code`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.17, nCustomers = 100)
}
hist(meanWaitingTimes)
meanSimWaiting_low <- mean(meanWaitingTimes)
meanSimWaiting_low

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_expected <- mean(meanWaitingTimes)
meanSimWaiting_expected

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(...)
}
hist(meanWaitingTimes)
meanSimWaiting_high <- mean(meanWaitingTimes)
meanSimWaiting_high

```

`@solution`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.17, nCustomers = 100)
}
meanSimWaiting_low <- mean(meanWaitingTimes)

# Part 2

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.21, nCustomers = 100)
}
meanSimWaiting_expected <- mean(meanWaitingTimes)

# Part 3

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.24, nCustomers = 100)
}
meanSimWaiting_high <- mean(meanWaitingTimes)
```

`@sct`
```{r}
test_object("meanSimWaiting_low", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_low`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_expected", undefined_msg = "Make sure to define an object `meanSimWaiting_low`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_expected`. Make sure to set the simulation input parameters to the correct value.")

test_object("meanSimWaiting_high", undefined_msg = "Make sure to define an object `meanSimWaiting_high`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_high`. Make sure to set the simulation input parameters to the correct value.")

success_msg("Correct! We have now tested the model's sensitivity over realistic ranges of the service time standard deviation. Note by how much the mean waiting time changed from the low to high values. From the one-factor at a time analysis it would seem that the model is not that sensitive over the standard deviation ranges. The same cannot be said of the arrival rate and mean service time. One short coming of the analysis is that we only consider the case of one variable being under or over estimated. For example, what would the impact be if we under estimated both the arrival rate and mean service time? In the next question we will formally look into this scenario by testing different combination of variables.")
```

---

## Multiple-factor sensitivity analysis

```yaml
type: NormalExercise
key: a3cdc3e22e
lang: r
xp: 100
skills: 1
```

For the last question we are going to test the sensitivity of our model over different parameter combinations.
To simplify our analysis we will only consider the lower and upper value for each parameter's bootstrapping interval.

We will therefore have two parameters values to test for each of our three input parameters. To test all combinations of the values we will run the simulation model $2^3 = 8$ times.

The values that we will test are as follow:

```
arrivalRateFactors <- c(1.17, 1.91)
serviceTimeMeanFactors <- c(0.97, 1.08)
serviceTimeSdFactors <- c(0.17, 0.24)
```

To compare the results of the input factor combinations we first need to setup a base case, and then measure the difference in model output from the changed input parameters to the base case. This will give us a good indication of how sensitive our model is towards the parameter combinations. For the base case we will simply calculate the mean waiting time when all the input parameters are set to their expected values from the bootstrapping method. This part of the code is give.

Incomplete code has been provided to calculate the mean value and print the output to the console screen.
Note that in the code the for loops are called a bit differently.
Instead of iteration over the a number from 1 to n, the for code directly retrieves the value in from the arrival factor. To see how it works run the following in the console:

```
for (arrivalRate in arrivalRateFactors)
{
  print(arrivalRate)
}
```

For this question, do the following:

`@instructions`
- Complete the given code and run the model.
- Analyse the output and see if the model is still insensitive to the standard deviation.
- Analyse the output and determine if the model is sensitive towards the combination of the mean service time and arrival rate.
- At the extreme values, which parameter would you consider to be the most critical?

Print the output and answer the following before submitting your answer.

`@hint`


`@pre_exercise_code`
```{r}

```

`@sample_code`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.22, nCustomers = 100)
}
base_case_mean <- mean(meanWaitingTimes)
base_case_mean

arrivalRateFactors <- c(1.17, 1.91)
serviceTimeMeanFactors <- c(0.97, 1.08)
serviceTimeSdFactors <- c(0.17, 0.24)

baseCaseDifferences <- c()

for (arrivalRate in arrivalRateFactors)
{
  for (serviceMean in serviceTimeMeanFactors)
  {
    for (serviceSd in serviceTimeSdFactors)
    {
      meanWaitingTimes <- rep(NA, 1000)
      for(i in 1:1000)
      {
        meanWaitingTimes[i] <- atmSimulation(arrivalRate = ..., serviceTimeMean = ..., serviceTimeSD = ..., nCustomers = 100)
      }
      simulationMean <- mean(meanWaitingTimes)
      baseCaseDifference <- base_case_mean - simulationMean
      print(paste(arrivalRate, serviceMean, serviceSd, simulationMean, baseCaseDifference))
      baseCaseDifferences <- c(baseCaseDifferences, baseCaseDifference)
    }
  }
}
```

`@solution`
```{r}
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

meanWaitingTimes <- rep(NA, 1000)
for(i in 1:1000)
{
  meanWaitingTimes[i] <- atmSimulation(arrivalRate = 1.47, serviceTimeMean = 1.03, serviceTimeSD = 0.22, nCustomers = 100)
}
base_case_mean <- mean(meanWaitingTimes)
base_case_mean

arrivalRateFactors <- c(1.17, 1.91)
serviceTimeMeanFactors <- c(0.97, 1.08)
serviceTimeSdFactors <- c(0.17, 0.24)

baseCaseDifferences <- c() # do not edit this

for (arrivalRate in arrivalRateFactors)
{
  for (serviceMean in serviceTimeMeanFactors)
  {
    for (serviceSd in serviceTimeSdFactors)
    {
      meanWaitingTimes <- rep(NA, 1000)
      for(i in 1:1000)
      {
        meanWaitingTimes[i] <- atmSimulation(arrivalRate = arrivalRate, serviceTimeMean = serviceMean, serviceTimeSD = serviceSd, nCustomers = 100)
      }
      simulationMean <- mean(meanWaitingTimes)
      baseCaseDifference <- base_case_mean - simulationMean
      print(paste(arrivalRate, serviceMean, serviceSd, simulationMean, baseCaseDifference))
      baseCaseDifferences <- c(baseCaseDifferences, baseCaseDifference) # do not edit this
    }
  }
}
```

`@sct`
```{r}
test_object("baseCaseDifferences", undefined_msg = "Make sure to define an object `baseCaseDifferences`. You edited the code.",
incorrect_msg = "Something went wrong in running the different simulations. The `arrivalRate`, `serviceMean` and `serviceTimeSD` has to be set to a new level each time the simulation is run. You therefore need to use the variables in the `for` loops correctly, and assign the input parameters to the appropriate `for` loop variables. Have a look at the example given in the problem statemen.")

success_msg("Carefully go through the model output. Note by how much the difference from the base case changes at the different levels of the standard deviation. It differs by very little. Even at the extreme values, changing the standard deviation has little impact on the model output. Our model is therefore not as sensitive towards the standard deviation so we need not worry too much about its accuracy. The same does not hold for the service mean and arrival rate. The waiting time increases by about 5 minutes at the different service times means, whereas it changes by about 15 minutes at the different arrival rates. The combination are also critical, where the waiting time can change by about 20 minutes between different extremes. The changes are therefore quite drastic. This should be taken into account when analysing the model output in that our model output may be wrong by about 10 minutes in either direction.")
```
