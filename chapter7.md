---
title_meta  : Case study 10
title       : Case study 10 - Sensitivity analysis on queueing models
description : "In this case study we will first develop a basic queueing model
that models how customers arrive at an ATM and perform transactions.
Thereafter we will test the sensitivity of our model to model parameters."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

**Read the below instructions carefully.**

In this case study we will model a simple queuing system that represent customers using an ATM.
Customers arrive at an ATM and perform a number of ATM transactions.
After completing their transactions, the customer takes their card and leaves.
The next customer can then use the ATM.
When the ATM is in use, the next customers have to wait in-line until it is their time to use the ATM.

A key measurement of such a queueing system is how long customers wait in-line, before it is their turn to use the ATM.
If customers wait too long, the system should be improved. Either be reducing the time that customers spend completing transactions or by increasing the number ATMs.

Is this chapter we will develop a simulation model for this simply process and measure the average waiting time of customers.
When developing the model, we need to make certain assumptions, such as the arrival time and time required by customers to complete the transactions.

Critical to model development is to test the impact of the assumptions on the model output and to see how sensitive the model is to a change in any of the assumptions.
After developing the queueing model we will conduct informal sensitivity analysis.
We will sequentially change the model input parameters and see how it influences the customer queuing times.
By doing so we can see which input parameters are critical for our analysis, and spend extra effort to ensure that they are accurate, such as obtaining more data for the random variable.

In this lab we do the following:

1. Develop a simulation model of queueing model.
2. Run the simulation model, measure and analyse the average customer waiting time.
3. Perform sensitivity to determine which model parameters, if changed, will have the biggest influence on customer waiting time.

The specific R functions that are applicable to this chapter are:

```
rnorm(), rexp(), cumsum(), function(), rep(), for,
```

To find out more about the functions, type `?` followed by the R-function in the R console.

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

--- type:MultipleChoiceExercise lang:r xp:10 skills:1 key:1a10d56107
## Constant arrival and transaction times

Assume that customers arrive at the ATM exactly 1 minute apart and that only one customer arrives at a time.
Further assume that a customer takes exactly 0.9 minutes to complete his transactions.
Based on these assumptions, what is the mean waiting time of 100 customers over the course of a single day?

*** =instructions

* 0 minutes;
* between 1 and 5 minutes;
* between 5 and 10 minutes;
* more than 10 minutes;
* not enough information is available to answer the question.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_bad <- "Incorrect. Carefully read the description of queuing system and the given information of the question."
msg_success <- "Correct. Customers arrive exactly 1 minute apart. Meaning, a new customer will arrive exactly 60 seconds after the previous one. A customer arriving at the ATM will only spend 0.9 minutes at the ATM and then leave. Each customer will therefore leave 0.1 minutes before the next customer arrives. The ATM will be idle for 0.1 minutes, and no customer has to wait in-line since the ATM will always be idle by the time they arrive."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:592f5b5fae
## Calculating arrival-time, waiting time and completing time of two customers

Considering the following case.
A customer arrives at the ATM at 14:01 and takes 1.3 minutes to complete his transactions.
A second customer arrives at time 14:02 and takes 0.5 minutes to complete his transactions.
Based on this information, answer the following:

*** =instructions

1. At what time will the first customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_1`. For example, if the customer is done with the ATM at 14:05, then `leave_1 <- 5`. For minute fractions, simply give your answer as a decimal. For example, if the customer is done at 14:05:30, then `leave_1 <- 5.5`.
2. How long will the second customer wait before he can use the ATM? Give your answer in minutes after 14:00 and assign your answer to `wait_2`.
3. At what time will the second customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_2`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# 1. At what time will the first customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_1`.



# 2. How long will the second customer wait before he can use the ATM? Give your answer in minutes after 14:00 and assign your answer to `wait_2`.



# 3. At what time will the second customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_2`.



```

*** =solution
```{r}
# 1. At what time will the first customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_1`.

leave_1 <- 1 + 1.3

# 2. How long will the second customer wait before he can use the ATM? Give your answer in minutes after 14:00 and assign your answer to `wait_2`.

wait_2 <- leave_1 - 2

# 3. At what time will the second customer be done with the ATM? Give your answer in minutes after 14:00 and assign your answer to `leave_2`.

leave_2 <- 2 + wait_2 + 0.5
```

*** =sct
```{r}
test_object("leave_1", undefined_msg = "Make sure to define an object `leave_1`.",
incorrect_msg = "Something went wrong in calculating `leave_1`.
Make sure to give your answer as the minutes after 14:00 that the customer 1 will done with the ATM.
Give your answer as a number, not a string.")

test_object("wait_2", undefined_msg = "Make sure to define an object `wait_2`.",
incorrect_msg = "Something went wrong in calculating `wait_2`.
The time that customer 2 has to wait is equal to the time from his arrival at the ATM until customer 1 is done with the ATM.")

test_object("leave_2", undefined_msg = "Make sure to define an object `leave_2`.",
incorrect_msg = "Something went wrong in calculating `leave_2`.
Make sure to give your answer as the minutes after 14:00 that customer 2 will done with the ATM.
Give your answer as a number, not a string.
The time that customer 2 is done will depend on the time that he arrived, his waiting time, and his transaction time.")

success_msg("Correct!
The time that customer 1 will leave the ATM is equal to his arrival time plus his transaction time.
Since customer 2 will arrive while customer 1 is still busy at the ATM, he first has to wait until customer 1 is done.
Thereafter he will use the ATM and leave after completing his transaction.
His waiting time is equal to the time since his arrival until customer 1 is finished.
His finishing time is equal to his arrival time, plus waiting time plus the time required to complete his transaction.
If we know the arrival and transaction time of each customer, we can easily determine if the customers will wait in line, and for how long the customers will wait.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:bb32493aff
## Determining how long customers will wait in-line

A customer's waiting time depends on his arrival time and the time that the previous customer will leave the ATM.
When determining if customer $i$ will wait, we always need to see when customer $i-1$ is done with his transactions.
Let $a\_i$ be the arrival time of customer $i$, $w\_i$ be his waiting time, $s\_i$ be his service time, and $l\_i$ the time that he leaves the ATM.

In the model, $a\_i$ and $s\_i$ will be simulated and $w\_i$ and $l\_i$ will be calculated.
To check if customer $i$ will wait at the ATM we need to compare $a\_i$ against $l\_{i-1}$.
If $a\_i > l\_{i-1}$, customer $i$ will arrive after customer $i-1$ has left the ATM, and $w\_i = 0$.
Else, customer $i$ will have to wait until customer $i-1$ has left the ATM.
The waiting time will then be $w\_i = l\_{i-1} - a\_i$.
The customer will then be done at $l\_i = a\_i + w\_i + s\_i$

An alternative way to calculate the waiting time is to use the following equation:

$$w\_i = \max(0, l\_{i-1} - a\_i).$$

If $l\_{i-1} < a\_i$, then $w\_i$ will take the value of 0, since $0 > l\_{i-1} - a\_i$.
If not, $a\_i$ will take the value of the waiting time.

The above equation can be used to calculate the waiting time for any customer.
Lastly, $l\_i$ can be calculated as follows:

$$l\_i = a\_i + w\_i + s\_i.$$

By manually setting $a_1 = 0$ for the first customer, we can use the above equations to calculate the waiting time for the next $n > 1$ customers.

For this question we will calculate the waiting times of 100 customers.
The arrival times of the customers, starting at time 0 is available in the `arrivalTime` vector, and their service time is available in the `serviceTime` vector.

*** =instructions

1. Initiate two vectors with 100 `NAs` for the waiting time and leaving time of customers. Assign the vectors to `waitingTime` and `leavingTime`.
2. Using the above equations and the provided `for`-loop code, calculate the waiting and leaving times of the 100 customers and assign the times to `waitingTime` and `leavingTime`. Remember that the times of the first customer has to be calculated outside the for loop, as shown.
3. Draw a barplot of the `waitingTime`.
4. Calculate and view the mean waiting time for the 100 customers and assign your answer to `meanWaitingTime`.

*** =pre_exercise_code
```{r}
arrivalTime <- cumsum(rexp(100, 1))
serviceTime <- rnorm(100, 0.9, 0.25)
```

*** =sample_code
```{r}
# 1. Initiate two vectors with 100 `NAs` for the waiting time and leaving time of customers. Assign the vectors to `waitingTime` and `leavingTime`.



# 2. Using the above equations and the provided `for`-loop code, calculate the waiting and leaving of the 100 customers and assign the times to `waitingTime` and `leavingTime`. Remember that the times of the first customer has to be calculated outside the for loop, as shown.

waitingTime[1] <- 0
leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

for(i in 2:100)
{
  waitingTime[i] <-
  leavingTime[i] <-
}

# 3. Draw a barplot of the `waitingTime`.



# 4. Calculate and view the mean waiting time for the 100 customers and assign your answer to `meanWaitingTime`.



```

*** =solution
```{r}
waitingTime <- rep(NA, 100)
leavingTime <- rep(NA, 100)

waitingTime[1] <- 0
leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

for (i in 2:100)
{
  waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
  leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
}
barplot(waitingTime)
meanWaitingTime <- mean(waitingTime)
```

*** =sct
```{r}
test_object("waitingTime", undefined_msg = "Make sure to define an object `waitingTime`.",
incorrect_msg = "Something went wrong in calculating the waiting times of the 100 customers.
Use the given equations and make sure and make sure to assign the waiting times to the vector `waitingTime`.")

test_object("leavingTime", undefined_msg = "Make sure to define an object `leavingTime`.",
incorrect_msg = "Something went wrong in calculating the leaving times of the 100 customers.
Use the given equations and make sure and make sure to assign the waiting times to the vector `leavingTime`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `waitingTime`.",
incorrect_msg = "Draw a barplot of `waitingTime`.")

test_object("meanWaitingTime", undefined_msg = "Make sure to define an object `meanWaitingTime`.",
incorrect_msg = "Remember to calculate the mean waiting time of the 100 customers and assign your answer to `meanWaitingTime`.")

success_msg("Correct!
Taking the arrival and service times of 100 customers, the above code can calculate the waiting time of each customer.
All that's left for the simulation model is to randomly sample the arrival and waiting times, using specified distributions.
Thereafter we can repeatedly simulate the waiting times of 100 customers.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:7981afe651
## ATM waiting time simulation

In the previous exercise we wrote all the necessary code to calculate the waiting and leaving time of customers.
All that is left is to simulate customer arrivals and transaction times.

There are two methods to do so.
First, we can simulate it for each customer prior to calculating the waiting and leaving time of the customer.
Alternatively we can simulate it for all the customers and generate the `arrivalTime` and `serviceTime` vectors, and then use the previous code as-is.

In this question we will follow the second approach.

Assume that the inter-arrival time of customers follow an exponential distribution with a rate of 1 customer per minute.
Service time follows a normal distribution with a mean of 0.9 minutes and standard deviation of 0.25 minutes.

Using this information we can easily simulate the arrival times and service times of 100 customers.
Service times can be sampled directly from the normal distribution using the `rnorm(100, mean, sd)` function.

Arrival times are a bit more tricky.
Sampling from the exponential distribution will give us the time that a customer arrived after the previous customer.
If customer 1 arrived at 0.8 minutes, starting form time 0, and the inter-arrival time of customer 2 is randomly sampled as 0.5 minutes, it means that customer 2 will arrive at time `0.8 + 0.5 = 1.3` minutes.
The same calculation can be repeated for the arrival time of customer 3, up until customer 100.

To calculate the arrival times we can calculate it per customer, starting with customer two.
Or we can use the built-in function `cumsum`.
We simply need to sample the inter-arrival times of the 100 customers using the `rexp(100, rate)` function.
Thereafter `cumsum` will calculate the actual arrival-times.

To find out more about the function, type `?cumsum` in the terminal.

For this question we will complete the simulation model of the ATM process by randomly sampling the arrival and service times of 100 customers.
The waiting time calculations are performed in the provided function, populated with the code from the previous question.

*** =instructions

1. Randomly sample the arrival times of 100 customers using the `rexp`, `cumsum` and `rnorm` functions and assign your answers to `arrivalTime` and `serviceTime`.
2. Use the provided function to calculate the mean waiting time of the 100 customers. Assign your answer to `meanWaitingTime`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

# 1. Randomly sample the arrival times of 100 customers using the `rexp` and `cumsum` functions and assign your answers to `arrivalTime` and `serviceTime`.



# 2. Use the provided function to calculate the mean waiting time of the 100 customers. Assign your answer to `meanWaitingTime`.



```

*** =solution
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
}

arrivalTime <- cumsum(rexp(100, 1))
serviceTime <- rnorm(100, 0.9, 0.25)

meanWaitingTime <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
```

*** =sct
```{r}
test_object("arrivalTime", undefined_msg = "Make sure to define an object `arrivalTime`.",
incorrect_msg = "Something went wrong in sampling the arrival times of 100 customers. You need to first sample the inter-arrival times of 100 customers from the exponential distribution using the `rexp` function. Thereafter you need to calculate the arrival times using the `cumsum` function.")

test_object("serviceTime", undefined_msg = "Make sure to define an object `arrivalTime`.",
incorrect_msg = "Something went wrong in sampling the service times of 100 customers. You need to sample the service times of 100 customers from the normal distribution using the `rnorm` function.")

test_object("meanWaitingTime", undefined_msg = "Make sure to define an object `meanWaitingTime`.",
incorrect_msg = "Use the given function to calculate the mean waiting time of the 100 customers and assign your answer to `meanWaitingTime`.")

success_msg("Correct! The above code can be used to simulate the queueing process.
The last step is to execute the simulation a large number of times and analyse the distribution of the mean waiting time of 100 customers.
Note that the inter-arrival and service times need not follow an exponential and normal distribution.
Any distribution can be used with its parameters.
The distribution should however be fitted based on data on the actual inter-arrival and service times.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8c2a4ef510
## ATM waiting time analysis

For this question we will repeat the simulation 1000 times and analyse the distribution of the mean waiting time of 100 customers for each simulation.

*** =instructions

1. Initiate a vector with 1000 `NAs` for the mean waiting times of the simulations. Assign the vector to `simWaitingTimes` vector.
2. Using a `for` loop, repeat the simulation 1000 times and store the simulation results in the appropriate place in the `simWaitingTimes`. Remember that the arrival and service time sampling has to be done in-side the `for` loop, otherwise we will simply calculate the same mean waiting time 1000 times, instead of a new simulated waiting time.
3. Draw a histogram of `simWaitingTimes`.
4. Calculate and view the mean of the mean waiting times of the simulations, that is, calculate the mean of `simWaitingTimes`. Assign your answer to `meanSimWaiting`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

# 1. Initiate a vector with 1000 `NAs` for the mean waiting times of the simulations. Assign the vector to `simWaitingTimes`.

simWaitingTimes <-

# 2. Using a `for` loop, repeat the simulation 1000 times and store the simulation results in the appropriate place in the `simWaitingTimes` vector. Remember that the arrival and service time sampling has to be done in-side the `for` loop, otherwise we will simply calculate the same mean waiting time 1000 times, instead of a new simulated waiting time.



# 3. Draw a histogram of `simWaitingTimes`.



# 4. Calculate and view the mean of the mean waiting times of the simulations, that is, calculate the mean of `simWaitingTimes`. Assign your answer to `meanSimWaiting`.



```

*** =solution
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}

hist(simWaitingTimes)
meanSimWaiting <- mean(simWaitingTimes)
```

*** =sct
```{r}
test_object("simWaitingTimes", undefined_msg = "Make sure to define an object `simWaitingTimes`.",
incorrect_msg = "Something went wrong in simulating the mean waiting time 1000 times. Use a `for`-loop to repeatedly call the arrival and service time sampling, and the mean waiting time calculation function. Assign each simulation's output to the appropriate place in `simWaitingTimes`.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of `simWaitingTimes`.",
incorrect_msg = "Draw a histogram of `simWaitingTimes`.")

test_object("meanSimWaiting", undefined_msg = "Make sure to define an object `meanSimWaiting`.",
incorrect_msg = "Calculate the mean of the mean waiting time over all 1000 simulation and assign your answer to `meanSimWaiting`.")

success_msg("Correct! We now have a fully functional simulation model of the queueing system.
All that's left is to test our assumptions and see how sensitive the model output is to changes in the model's input parameters.
To do so we will change an input parameter, redo the 1000 simulations and capture the final output for the parameter value.
Repeating this process will show by how much the simulation output changes with changes in the input parameters.
If the output changes by a lot, we know that the simulation is sensitive towards the specific input parameter, and we need to make sure that the parameters are accurate.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8c2a4ef510
## Testing the model sensitivity to the arrival rate

In the previous example we assumed that the customer arrival rate is 1 per minute, exponentially distributed, but what if this is not the case?
What of the actual customer arrival rate is 1.1 per minute?
Will that significantly increase or decrease customer waiting times.
Of, what if it is 0.9 per minute?

Since we have a simulation model available, we can easily change the arrival rate and see how the mean waiting time increases or decreases.

In this exercise we will test three levels for the arrival rate and measure the mean waiting time at the different levels.
By comparing the arrival rate we can get a sense of how sensitive our model is to changes in the arrival rate.

The arrival rate levels will be 0.5, 1 and 1.5, thereby representing the assumed parameter value, the value decreased by 50%, and the value increased by 50%.

*** =instructions

1. Use the available code and repeat the ATM simulation with an arrival rate of 0.5 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_05`.
2. Repeat the ATM simulation with an arrival rate of 1 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_1`.
3. Repeat the ATM simulation with an arrival rate of 1.5 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_15`.
4. View and compare `simWaitingTimes_05`, `simWaitingTimes_1` and `simWaitingTimes_15` and note by how much the mean waiting time increased as the arrival rate increased.

*** =pre_exercise_code
```{r}



```

*** =sample_code
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

# 1. Use the available code and repeat the ATM simulation with an arrival rate of 0.5 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_05`.

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, ...))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_05 <- mean(simWaitingTimes)

# 2. Repeat the ATM simulation with an arrival rate of 1 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_1`.



# 3. Repeat the ATM simulation with an arrival rate of 1.5 customer per minute. Assign the mean of the mean waiting times to `simWaitingTimes_15`.



# 4. View and compare `simWaitingTimes_05`, `simWaitingTimes_1` and `simWaitingTimes_15` and note by how much the mean waiting time increased as the arrival rate increased.



```

*** =solution
```{r}

atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 0.5))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_05 <- mean(simWaitingTimes)

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_1 <- mean(simWaitingTimes)

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1.5))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_15 <- mean(simWaitingTimes)

```

*** =sct
```{r}
test_object("meanSimWaiting_05", undefined_msg = "Make sure to define an object `meanSimWaiting_05`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_05`. Make sure to set the arrival rate to the correct value.")

test_object("meanSimWaiting_1", undefined_msg = "Make sure to define an object `meanSimWaiting_1`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_1`. Make sure to set the arrival rate to the correct value.")

test_object("meanSimWaiting_15", undefined_msg = "Make sure to define an object `meanSimWaiting_15`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_15`. Make sure to set the arrival rate to the correct value.")

success_msg("Correct! As expected the waiting time increases as the arrival rate increases.
As more customers arrive per minute, the inter-arrival times between customers decrease.
It is then more likely that customers will arrive before the previous customers are done with their transactions.
If interest is the rate by which the waiting time increases.
When the arrival rate is 0.5, customers spend on average less than 0.5 minutes waiting.
When increased to 1.5, customers spend less than 2.7 minutes waiting.
But when it is increased by another 0.5, the customer waiting time jumps to over 12 minutes.
This indicates that the model output is non-linearly effected by the arrival rate.
At low arrival rates our models is insensitive to changes.
As the arrival rate increases, our model becomes more sensitive.
We therefore have to make sure that our arrival rate is indeed low, preferably by gathering additional data.
We also need to make sure that arrival rate does not vary during the day as the waiting time will be very long if there are peak periods with high arrival rates.
In the next and last question we are going to look at the mean service time.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8c2a4ef510
## Testing the model sensitivity to the mean service time

In this exercise we will test three levels for the mean service time of customers.
We will then make an early judgement on which parameter is more critical to model output, the arrival rate or mean service time.

The service time levels will be 0.45, 0.9 and 1.35, thereby representing the assumed parameter value, the value decreased by 50%, and the value increased by 50%.
All other parameters will remain at their default values.

*** =instructions

1. Use the available code and repeat the ATM simulation with a mean service time of 0.45 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_045`.
2. Use the available code and repeat the ATM simulation with a mean service time of 0.9 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_09`.
3. Use the available code and repeat the ATM simulation with a mean service time of 1.35 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_135`.
4. View and compare `simWaitingTimes_045`, `simWaitingTimes_09` and `simWaitingTimes_135` and note by how much the mean waiting time increased as the mean service time increased.

*** =pre_exercise_code
```{r}



```

*** =sample_code
```{r}
atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

# 1. Use the available code and repeat the ATM simulation with a mean service time of 0.45 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_045`.

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, ..., 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
simWaitingTimes_045 <- mean(simWaitingTimes)

# 2. Use the available code and repeat the ATM simulation with a mean service time of 0.9 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_09`.



# 3. Use the available code and repeat the ATM simulation with a mean service time of 1.35 minutes. Assign the mean of the mean waiting times to `simWaitingTimes_135`.



# 4. View and compare `simWaitingTimes_045`, `simWaitingTimes_09` and `simWaitingTimes_135` and note by how much the mean waiting time increased as the mean service time increased.



```

*** =solution
```{r}

atmMeanWaitingTimeSim <- function(arrivalTime, serviceTime)
{
  waitingTime <- rep(NA, 100)
  leavingTime <- rep(NA, 100)

  waitingTime[1] <- 0
  leavingTime[1] <- arrivalTime[1] + waitingTime[1] + serviceTime[1]

  for (i in 2:100)
  {
    waitingTime[i] <- max(0, leavingTime[i - 1] - arrivalTime[i])
    leavingTime[i] <- arrivalTime[i] + waitingTime[i] + serviceTime[i]
  }
  meanWaitingTime <- mean(waitingTime)
  return(meanWaitingTime)
}

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, 0.45, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_045 <- mean(simWaitingTimes)

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, 0.9, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_09 <- mean(simWaitingTimes)

simWaitingTimes <- rep(NA, 1000)
for (i in 1:1000)
{
  arrivalTime <- cumsum(rexp(100, 1))
  serviceTime <- rnorm(100, 1.35, 0.25)
  simWaitingTimes[i] <- atmMeanWaitingTimeSim(arrivalTime, serviceTime)
}
meanSimWaiting_135 <- mean(simWaitingTimes)

```

*** =sct
```{r}
test_object("meanSimWaiting_045", undefined_msg = "Make sure to define an object `meanSimWaiting_045`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_045`. Make sure to set the mean service time to the correct value.")

test_object("meanSimWaiting_09", undefined_msg = "Make sure to define an object `meanSimWaiting_09`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_09`. Make sure to set the mean service time to the correct value.")

test_object("meanSimWaiting_135", undefined_msg = "Make sure to define an object `meanSimWaiting_135`.",
incorrect_msg = "Something went wrong in calculate `meanSimWaiting_135`. Make sure to set the mean service time to the correct value.")

success_msg("Correct! As expected the waiting time increases as the mean service time increases.
As customers customer take longer to complete their transactions, is more likely that customers will arrive before the previous customers are done with their transactions.
Similar to the arrival rate, the increase in waiting time is non-linear.
Form 0.35 to 0.9 minute means service times, the mean waiting time increases from 0.23 to 2.6.
When the mean service time increases to 1.35 minutes, the mean waiting time jumps to 18 minutes.
A 50% increase in arrival rate resulted in the mean waiting time to increase to 12 minutes.
It would therefore seem that waiting time is more sensitive to the mean service time.
The above analysis can be repeated for the standard deviation of the service time, as well as the number of customers that we are simulating.
We can also repeat the analysis for different distributions, if there are other distributions that are reasonable for the random variables.
We can also look at variable combinations, for example, what would happen if both the mean service time and standard deviation changes.
In the next chapter we will look at more formal methods to conduct such analysis.")
```
