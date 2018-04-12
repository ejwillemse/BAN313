---
title_meta  : Case study 7b
title       : Case study 7b - Estimating distribution parameters using bootstrapping
description : "In this case study, we will revisit the examples from Case study 7, where we analysed data for different processes. We will use boot-strapping methods to estimate model parameters."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In this case study we will revisit the examples of [Case study 7 - Fitting probability distributions using chi-squared goodness-of-fit test](https://campus.datacamp.com/courses/industrial-analysis-using-r/) and use bootstrapping techniques to estimate model parameters.

**Before continuing with this chapter, please do the following:**

1. Complete all previous chapters in this datacamp course.
2. Make sure you understand how the `sample` function works in R. Review the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) datacamp chapter if necessary.
3. Make sure you understand how the `for` function works in R, and how it is used to store simulation run outputs in a vector. Review the [Foundations for inference: Sampling distributions](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-3a-foundations-for-inference-sampling-distributions?ex=1) datacamp chapter if necessary.
4. Go through _Section 4.5: Bootstrapping for the standard deviation (p.195)_ of the prescribed textbook by Diez et al (2014).

The prescribed textbook is:

* Diez, D. M., Barr, C. D., and Çetinkaya Rundel, M. (2014). _Introductory Statistics With Randomization and Simulation_. CreateSpace Independent Publishing Platform. Available at [this link](https://www.openintro.org/stat/textbook.php?stat_book=isrs), 1st edition.

The following video on bootstrapping may also be useful, especially the first five minutes:

* [https://youtu.be/_nhgHjdLE-I](https://youtu.be/_nhgHjdLE-I)

**After completing the above, read the below instructions carefully.**

In this lab we will look at the same examples from Case study 6, and do the following:

1. Estimate confidence intervals for key distribution parameters using the bootstrapping method.
2. Use the R functions of the distribution to calculate percentiles and probabilities as required.

Some of R functions that are applicable to this chapter are:

```
dnorm(), pnorm(), qnorm(), dpois(), ppois(), qpois(), dexp(), pexp(), qexp(), dunif(), punif(), qunif(), dt(), pt(), qt()
```

To find out more about the functions, type `?function` in the R console. The basic usage of the functions is described in [this page](http://www.cyclismo.org/tutorial/R/probability.html).

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip each questions' background information. The question backgrounds contain valuable information to assist you in completing the questions.

To continue with this chapter confirm the following:

*** =instructions

* I have not completed all the prescribed preparation material, as listed in the 7 tasks above, or have not read all the instructions on this page.

* I confirm that I have completed the prescribed preparation material, as listed in the 7 tasks above, and have read **ALL** the instructions on this page carefully.

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
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a44acfd135
## Estimating key parameters for Drill-hole size distribution

We wish to replace our current very old drilling machine.
The machine is required to drill a hole with a diameter of 10cm within a 0.5cm tolerance.
If the hole is larger than 10.5cm, the part has to be scrapped.
Similarly, if the hole is smaller than 9.5cm the part will also be scrapped.

We also want to use the machine to drill holes for other products that have different tolerances.
One product may have a 0.25cm tolerance, another 1cm.

In the chapter we formally tested the goodness-of-fit for the normal distribution, and found that due to the large $p$-value of the $\chi^2$ test statistic we cannot reject $H\_0$, which is that hole-size follows a normal distribution with a mean and standard deviation equal to that of the sample data. If we were to repeat the question with a new sample, we would get a different sample mean and standard deviation, even though the process output follows the same distribution.

Ideally we would like to calculate confidence intervals for the distribution parameters, and thereby make our analyses of the process output more robust. In this question we will do exactly that by using bootstrapping to calculate key distribution parameters.

The basic component of bootstrapping is as follows. Using the sample data we will do the following:

1. Sample from the sample data with replacement. The number of samples from the sample will be equal to the original sample size.
2. Calculate the parameters of interest on the sample of samples.
3. Repeat steps 1 and 2 a large number of times and capture the key parameters of interest each time. Each sample from the sample can be considered a simulation, or a bootstrap.
4. Statistically analyse the distribution of the parameters captured over the multiple runs of step 3.

The following R functions will come in handy when doing bootstrapping:

```
rep(), for(), sample(), quantile()
```

When using `sample` we can set `x` to the original sample variable that we are analysing, from which we will now sample, and `n` to the number of samples in the original sample since we want to sample the same number of times. Importantly `replace = TRUE` so that sampling is done with replacement, otherwise we will just end up with an identical sample to the original. For more information on the functions, use the `?function` command.

For this question we will perform 1000 simulations (bootstraps) and analyse the distribution of the mean and standard deviation for the drill-hole size.

To complete the question, do the following:

*** =instructions

1. View the mean and standard deviation of `holeDiameter` by printing their values to the console. You do not need to assign the values to anything. Just print and view it.
2. Using the `sample` function take a random sample with replacement from the hole-size samples and assign the results to `holeDiameter_boot`.
3. View the mean and standard deviation of `holeDiameter_boot` by printing their values to the console. You do not need to assign the values to anything. Just print and view it and compare it to the mean and standard deviation of the original sample.
4. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `holeMeans`.
5. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `holeSD`.
6. Use a `for` loop and repeat the `sample` function to take a sample with replacement from the hole-size samples 10000 times. In each execution of the `for` loop you can assign the results to `holeDiameter_boot_test`. Calculate the mean and standard deviation of `holeDiameter_boot` and store the results in the appropriate `i` position of `holeMeans` and `holeSD`. Hint: if you are unsure how to do this, have a look at the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) and [Foundations for inference: Sampling distributions](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-3a-foundations-for-inference-sampling-distributions?ex=1) datacamp chapters.
7. Draw histograms of `holeMeans` and `holeSD`.
8. Use the `quantile` function to calculate 95% confidence intervals for `holeMeans` and `holeSD` using the `quantile` function and assign the results to `meanInter` and `sdInter`. Hint: for the mean hole-size the code will be `meansInter <- quantile(holeMeans, prob = c(0.025, 0.975))`
9. View `meanInter` and `sdInter` by printing their values to the console.
10. For a worst-case scenario we can use the `holeMeansInter` value that is the furthest away from the required drill hole size of 10cm, and the `sdInter` value that is the largest. Using the appropriate two values, calculate the probability of a hole falling *outside* a tolerance limit of 0.5cm. For the calculations, use `holeMeansInter` and `sdInter` directly by calling either `holeMeansInter[1]` or `holeMeansInter[2]`, depending on which can be considered as the worst-case option, and the same with `sdInter`. Assign your answer to `pDefective05`. Do not use the values displayed in the terminal. They are not accurate enough. Hint: use the `pnorm` function and remember that you will have to call it twice.
11. View the value of `pDefective05` by printing it to the console.

*** =hint

* Make sure you understand how the `sample` function works in R. Review the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) datacamp chapter if necessary.
* Make sure you understand how the `for` function works in R, and how it is used to store simulation run outputs in a vector. Review the [Foundations for inference: Sampling distributions](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-3a-foundations-for-inference-sampling-distributions?ex=1) datacamp chapter if necessary.
* Go through _Section 4.5: Bootstrapping for the standard deviation (p.195)_ of the prescribed textbook by Diez et al (2014).

*** =pre_exercise_code
```{r}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

#1. View the mean and standard deviation of `holeDiameter` by printing their values to the console. You do not need to assign the values to anything. Just print and view it.



#2. Using the `sample` function take a random sample with replacement from the hole-size samples and assign the results to `holeDiameter_boot_test`.



#3. View the mean and standard deviation of `holeDiameter_boot_test` by printing their values to the console. You do not need to assign the values to anything. Just print and view it and compare it to the mean and standard deviation of the original sample.



#4. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `holeMeans`.



#5. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `holeSD`.



#6. Use a `for` loop and repeat the `sample` function to take a sample with replacement from the hole-size samples 10000 times. In each execution of the `for` loop you can assign the results to `holeDiameter_boot`. Calculate the mean and standard deviation of `holeDiameter_boot` and store the results in the appropriate `i` position of `holeMeans` and `holeSD`. Hint: if you are unsure how to do this, have a look at the previous datacamp courses.

for(i in 1:10000)
{
  holeDiameter_boot <-

  holeMeans[i] <-

  holeSD[i] <-
}

#7. Draw histograms of `holeMeans` and `holeSD`.



#8. Use the `quantile` function to calculate 95% confidence intervals for `holeMeans` and `holeSD` using the `quantile` function and assign the results to `meanInter` and `sdInter`. Hint: for the mean hole-size the code will be `meansInter <- quantile(holeMeans, prob = c(0.025, 0.975))`



#9. View `meanInter` and `sdInter` by printing their values to the console.

meanInter
sdInter

#10. For a worst-case scenario we can use the `holeMeansInter` value that is the furthest away from the required drill hole size of 10cm, and the `sdInter` value that is the largest. Using the appropriate two values, calculate the probability of a hole falling *outside* a tolerance limit of 0.5cm. For the calculations, use `holeMeansInter` and `sdInter` directly by calling either `holeMeansInter[1]` or `holeMeansInter[2]`, depending on which can be considered as the worst-case option, and the same with `sdInter`. Assign your answer to `pDefective05`. Do not use the values displayed in the terminal. They are not accurate enough. Hint: use the `pnorm` function and remember that you will have to call it twice.



#11. View the value of `pDefective05` by printing it to the console.

pDefective05

```

*** =solution
```{r}
holeDiameter_boot_test <- sample(holeSize$holeDiameter_cm, size = nrow(holeSize), replace = TRUE)
holeMeans <- rep(NA, 10000)
holeSD <- rep(NA, 10000)
for (i in 1:10000)
{
  holeDiameter_boot <- sample(holeSize$holeDiameter_cm, size = nrow(holeSize), replace = TRUE)
  holeMeans[i] <- mean(holeDiameter_boot)
  holeSD[i] <- sd(holeDiameter_boot)
}
hist(holeMeans)
hist(holeSD)
meanInter <- quantile(holeMeans, prob = c(0.025, 0.975))
sdInter <- quantile(holeSD, prob = c(0.025, 0.975))

if(abs(meanInter[1] - 10 - 0.5) > abs(meanInter[2] - 10.0 + 0.5))
{
  worstMean <- meanInter[1]
}else{
  worstMean <- meanInter[2]
}
worstSD <- sdInter[2]
pDefective05 <- pnorm(10 - 0.5, worstMean, worstSD) + pnorm(10 + 0.5, worstMean, worstSD, lower.tail = FALSE)
pDefective05
```

*** =sct
```{r}
test_object("holeDiameter_boot_test", undefined_msg = "Make sure to define an object `holeDiameter_boot_test`.",
            incorrect_msg = "Make sure that you took a sample from `holeSize$holeDiameter` with replacement and assigned the result to `holeDiameter_boot_test`")       

test_object("holeMeans", undefined_msg = "Make sure to define an object `holeMeans`.",
            incorrect_msg = "Make sure that you stored the mean of each of 10000 simulations in `holeMeans`.")  

test_object("holeSD", undefined_msg = "Make sure to define an object `holeSD`.",
            incorrect_msg = "Make sure that you stored the standard deviation of each of 10000 simulations in `holeSD`.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of hole `holeMeans`.",
              incorrect_msg = "Draw a histogram of `holeMeans`.")

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of `holeSD`.",
              incorrect_msg = "Draw a histogram of `holeSD`.")

test_object("meanInter", undefined_msg = "Make sure to define a variable `meanInter`.",
            incorrect_msg = "Calculate the 95% confidence interval for `holeMeans` and assign the results to `meanInter`.")

test_object("sdInter", undefined_msg = "Make sure to define a variable `sdInter`.",
            incorrect_msg = "Calculate the 95% confidence interval for `holeSD` and assign the results to `sdInter`.")

test_object("pDefective05", undefined_msg = "Make sure to define a variable `pDefective05`.",
            incorrect_msg = "Calculate the a worst case defective probability based on the 0.5cm tolerance and assign the results to `pDefective05`. One option is to logically decided on the values from the 95% confidence intervals that will give the highest defective probability. Another option is to test different combinations of the confidence intervals to see which combination will give the highest defective probability. Note that you will have to call `pnorm` twice to calculate `pDefective05` and that the same mean and standard deviation should be use. Do not use a different mean and standard deviation for the left and right tails.")

success_msg("Correct! By using the bootstrap method we can calculate a confidence interval for any parameter, including the mean and the standard deviation. The confidence intervals can then be used to get a conservative estimate of the process output.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:26ce6147d6
## Gautrain arrival rate: parameter estimation using bootstrapping

In the previous chapter we analysed Guatrain arrival rate per minute. We formally tested the goodness-of-fit for the poisson distribution and found that we cannot reject $H_0$. We can therefore model the number passenger arrivals per minute, $X$, using a poisson distribution, such that:

$$X \sim \text{Poisson}(\lambda),$$

where $\lambda$ is the expected (mean) number of arrivals per minute. For the goodness-of-fit test we estimated $\lambda$  using the sample data, but if we got a new sample, we would get a different expected number of arrivals per minute, even though arrivals follow the same poisson distribution.

Ideally we would like to calculate confidence intervals for the arrival rate, and thereby make our analyses of the arrivals more robust. In this question we will do exactly that by using bootstrapping to calculate the arrival rate.

Approximately _three_ passengers can go through Gautrain access gate per minute. The Gautrain wants to know how many access-gates should be in operation. The main constraint is that the probability of the access-gates not being able to process all the passengers arriving during a minute should be less than 0.05. We need to determine the number of access gates to ensure that the probability of the access-gates being insufficient to deal with the arriving customers during a minute is less than 0.05.

To answer the question we will do the following:

1. Calculate 99% confidence interval for the expected arrival rate per minute, $\lambda$, using the bootstrap method.
2. Use $X \sim \text{Poisson}(\lambda)$ by setting $\lambda$ the upper-limit of the confidence interval to calculate the 95$^\text{th}$ percentile of the number of customers arriving per minute, refer to as $X\_{95}$. This means that 95\% of the time, less than $X\_{95}$ customers will arrive per minute, meaning there is a 5% chance that more than $X\_{95}$ customers will arrive per minute.
3. Lastly we will use $X\_{95}$ and the given information that three passengers can go through a gate per minute to determine the number of required gates.

The arrival rate per minute of passengers between 16:00 and 18:00PM for the previous 30 working days can be found in the `nArrivePerMin` vector.

The instructions for the question are as follow:

*** =instructions

1. View the mean number of arrivals of `nArrivePerMin` by printing the values to the console. You do not need to assign the value to anything. Just print and view it.
2. Using the `sample` function take a random sample with replacement from the nArrivePerMin samples and assign the results to `nArrivePerMin_boot_test`.
3. View the mean number of arrivals of `nArrivePerMin_boot_test` by printing the value to the console. You do not need to assign the value to anything. Just print and view it and compare it to the mean of the original sample.
4. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `meanArrivePerMin`.
5. Use a `for` loop and repeat the `sample` function to take a sample with replacement from the `nArrivePerMin` samples 10000 times. In each execution of the `for` loop you can assign the results to `nArrivePerMin_boot`. Calculate the mean of `nArrivePerMin_boot` and store the results in the appropriate `i` position of `meanArrivePerMin`. Hint: if you are unsure how to do this, have a look at the previous questions and deatacamp courses.
6. Draw histograms of `meanArrivePerMin`.
7. Use the `quantile` function to calculate 99% confidence interval for `meanArrivePerMin` using the `quantile` function and assign the results to `meanInter`. Hint: if you are unsure how to do this, have a look at the previous question.`
8. View `meanInter` by printing its value to the console.
9. Use the upper-value of `meanInter` for $\lambda$ and calculate the 95$^\text{th}$ percentile of the number of customers arriving per minute. Hint: use the `qpois(p=0.95, lambda)` function. Assign your answer to `nArrive95`.
10. Calculate the minimum number of gates required using the 95$^\text{th}$ percentile and the information that a single gate can process _three_ customers per minute. Remember to round your final answer-up. Assign your final answer to `nGates`.
11. View your final answer by printing `nGates` to the console.

*** =hint

Refer to the previous question on bootstrapping and the previous BAN313 chapter.

*** =pre_exercise_code
```{r}
set.seed(31)
nArrivePerMin <- rpois(30*120, 5)
```

*** =sample_code
```{r}
# The arrivals per minute are available in the `nArrivePerMin` vector.

#1. View the mean number of arrivals of `nArrivePerMin` by printing the values to the console. You do not need to assign the value to anything. Just print and view it.



#2. Using the `sample` function take a random sample with replacement from the nArrivePerMin samples and assign the results to `nArrivePerMin_boot_test`.



#3. View the mean number of arrivals of `nArrivePerMin_boot_test` by printing the value to the console. You do not need to assign the value to anything. Just print and view it and compare it to the mean of the original sample.



#4. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `meanArrivePerMin`.



#5. Use a `for` loop and repeat the `sample` function to take a sample with replacement from the `nArrivePerMin` samples 10000 times. In each execution of the `for` loop you can assign the results to `nArrivePerMin_boot`. Calculate the mean of `nArrivePerMin_boot` and store the results in the appropriate `i` position of `meanArrivePerMin`. Hint: if you are unsure how to do this, have a look at the previous questions and deatacamp courses.



#6. Draw histograms of `meanArrivePerMin`.



#7. Use the `quantile` function to calculate 99% confidence interval for `meanArrivePerMin` using the `quantile` function and assign the results to `meanInter`. Hint: if you are unsure how to do this, have a look at the previous question.`



#8. View `meanInter` by printing its value to the console.



#9. Use the upper-value of `meanInter` for $\lambda$ and calculate the 95th percentile of the number of customers arriving per minute. Hint: use the `qpois(p=0.95, lambda)` function. Assign your answer to `nArrive95`.



#10. Calculate the minimum number of gates required using the 95th percentile and the information that a single gate can process _three_ customers per minute. Remember to round your final answer-up. Assign your final answer to `nGates`.



#11. View your final answer by printing `nGates` to the console.



```

*** =solution
```{r}
mean(nArrivePerMin)
nArrivePerMin_boot_test <- sample(nArrivePerMin, size = length(nArrivePerMin), replace = TRUE)
mean(nArrivePerMin_boot_test)
meanArrivePerMin <- rep(NA, 10000)
for (i in 1:10000)
{
  nArrivePerMin_boot <- sample(nArrivePerMin, size = length(nArrivePerMin), replace = TRUE)
  meanArrivePerMin[i] <- mean(nArrivePerMin_boot)
}
hist(meanArrivePerMin)
meanInter <- quantile(meanArrivePerMin, c(0.005, 0.995))
meanInter

nArrive95 <- qpois(p=0.95, meanInter[2])
nGates <- ceiling(nArrive95/3)
nGates
```

*** =sct
```{r}
test_object("nArrivePerMin_boot_test", undefined_msg = "Make sure to define an object `nArrivePerMin_boot_test`.",
            incorrect_msg = "Make sure that you took a sample from `nArrivePerMin` with replacement and assigned the result to `nArrivePerMin_boot_test`")     

test_object("meanArrivePerMin", undefined_msg = "Make sure to define an object `meanArrivePerMin`.",
            incorrect_msg = "Make sure that you stored the mean number of arrivals for each the 10000 simulations in `meanArrivePerMin`.")     

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of hole `meanArrivePerMin`.",
              incorrect_msg = "Draw a histogram of `meanArrivePerMin`.")

test_object("meanInter", undefined_msg = "Make sure to define a variable `meanInter`.",
            incorrect_msg = "Calculate the 99% confidence interval for `meanArrivePerMin` and assign the results to `meanInter`.")

test_object("nArrive95", undefined_msg = "Make sure to define a variable `nArrive95`.",
            incorrect_msg = "Calculate the 95th percentile for mean number of arrivals using the upper-limit of `meanInter` and the `qpois` function.")

test_object("nGates", undefined_msg = "Make sure to define a variable `nGates`.",
            incorrect_msg = "Calculate the number of required gates using `nArrive95` and assign your answer to `nGates`.")

success_msg("Correct! By using the bootstrap method we can calculate a confidence interval for the arrival rate. The confidence intervals can then be used to get a conservative estimate of the number of arrivals which can then be used to determine the resource requirements.")
```
