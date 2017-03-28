---
title_meta  : Case study 7
title       : Case study 7 - Fitting probability distributions and estimating distribution parameters
description : "In this case study we revisit the examples from Case study 6, where we analysed data for different processes, and eyeballed the data to see if the processes can be modelled using specific probability distribution functions. In this case study we will use formal methods to test if a distribution is appropriate, using the chi-squared goodness-of-fit test. We will also use boot-strapping methods to estimate model parameters."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In this case study we will revisit the examples of [Case study 6: Different applications of probability distributions](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) and use formal methods to test whether the processes can be modelled using specific probability distribution functions. We will further use bootstrapping techniques to estimate model parameters.

**Before continuing with this chapter, please do the following:**

1. Complete [Case study 6: Different applications of probability distributions](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) on Datacamp.
2. Make sure you understand how the `sample` function works in R. Review the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) datacamp chapter if necessary.
3. Make sure you understand how the `for` function works in R, and how it is used to store simulation run outputs in a vector. Review the [Foundations for inference: Sampling distributions](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-3a-foundations-for-inference-sampling-distributions?ex=1) datacamp chapter if necessary.
4. Review _Section 3.3: Testing of goodness of fit using chi-square (p.134)_ of the prescribed textbook by Diez et al (2014).
5. Go through _Section 4.5: Bootstrapping for the standard deviation (p.195)_ of the prescribed textbook by Diez et al (2014).
6. Go through the goodness-of-fit using R tutorial, available from [this link](https://www.r-bloggers.com/goodness-of-fit-test-in-r/).
7. Review the lecture slides `Lecture_Week7.pdf`, available on _clickUP_ under `Theme 2: Simulation models/Week 7 Distribution fitting: lecture material`, or by clicking [on this link](https://clickup.up.ac.za/bbcswebdav/pid-1017449-dt-content-rid-11442908_1/courses/ban313_s1_2017/Lecture_Week7.pdf).

The prescribed textbook is:

* Diez, D. M., Barr, C. D., and Çetinkaya Rundel, M. (2014). _Introductory Statistics With Randomization and Simulation_. CreateSpace Independent Publishing Platform. Available at [this link](https://www.openintro.org/stat/textbook.php?stat_book=isrs), 1st edition.

**After completing the above, read the below instructions carefully.**

In this lab we will look at the same examples from Case study 6, and do the following:

1. Visualise the process data using histograms and guess what distribution can be used to model the process.
2. Use the statistical chi-square goodness-of-fit test to formally test if the distribution is appropriate.
3. Analyse the distribution of and estimate confidence interval for key distribution parameters using the bootstrapping method.
4. Use the R functions of the distribution to calculate percentiles and probabilities as required.


The specific R functions that are applicable to this chapter are:

```
dnorm(), pnorm(), qnorm(), dpois(), ppois(), qpois(), dexp(), pexp(), qexp(), dunif(), punif(), qunif(), dt(), pt(), qt()
```

To find out more about the functions, type `?function` in the R console. The basic usage of the functions is described in [this page](http://www.cyclismo.org/tutorial/R/probability.html).

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip the preceding question background. The question background contains valuable information to assist you in completing the question.

To continue with this chapter confirm the following:

*** =instructions

* I have not completed all the prescribed preparation material, as listed in the 7 tasks above, or have not read all the instructions on this page.

* I confirm that I have completed the prescribed preparation material, as listed in the 7 tasks above, and have read **ALL** the instructions on this page carefully.

*** =hist

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
test_mc(correct = 2, feedback_msgs = c(msg_success, msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a44acfd135
## Drill-hole sizes

We wish to replace our current very old drilling machine.
The machine is required to drill a hole with a diameter of 10cm within 0.5cm tolerance.
If the hole is larger than 10.5cm, the part has to be scrapped.
Similarly, if the hole is smaller than 9.5cm the part will also be scrapped.

We also want to use the machine to drill holes for other products that have different tolerances.
One product may have a 0.25cm tolerance, another 1cm.

We have identified a possible replacement machine, and its supplier assured us that it will perform according to specifications.
We insisted that the supplier provide us with data on its performance, so that we can make a more informed decision on whether it will meet our requirements.

The supplier has given us data on different holes drilled by the machine during its testing.
The data consist of a number of drill samples, with the hole diameter for each hole measured and captured.
The data is available in the `holeSize` dataframe.

For the first part of this question, analyse the distribution of hole sizes from the machine and identify the distribution that best describes the hole-sizes from the machine, find it's key parameters from the sample and using R's built-in function, use the _distribution_ (not the sample data) to find the probability of a hole drilled by the machine to fall outside a 0.30cm tolerance limit.

*** =instructions

1. Analyse the distribution of the hole-sizes using the `hist()` function and by playing around with the `breaks` parameter.
2. Determine the key parameters of the distribution (note that this part is not checked for correctness).
3. Using the distribution (not the sample data), calculate the **probability** of a hole drilled by the machine to fall **outside** a 0.30cm tolerance limit, and will therefore be defective. Assign your answer to `pDefective03`.
4. View the value of `pDefective03` by printing it to the console output via the `script.R` file.

*** =hint

Use `hist(holeSize$holeDiameter_cm)` to analyse the distribution and see which of the four distributions

* normal,
* poisson and exponential,
* power-law,
* uniform, or
* $t$-distribution,

it most closely represents.

Use the appropriate function from the following to calculate the probability of a hole falling outside the 0.30cm tolerance limit:

```
pnorm, ppois, pexp, punif, pt.
```
Remember that outside the tolerance means the hole is either smaller than 10 - 0.3cm **or** bigger than 10 + 0.3cm.

*** =pre_exercise_code
```{r}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

# 1. Analyse the distribution of the hole-sizes using the `hist()` function and by playing around with the `breaks` parameter.



# 2. Determine the key parameters of the distribution (note that this part is not checked for correctness).



# 3. Using the distribution (not the sample data), calculate the **probability** of a hole drilled by the machine to fall **outside** a 0.30cm tolerance limit, and will therefore be defective. Assign your answer to `pDefective03`.




# 4. View the value of `pDefective03` by printing it to the console output via the `script.R` file.Print the values here:



```

*** =solution
```{r}
hist(holeSize$holeDiameter_cm)
meanHole <- mean(holeSize$holeDiameter_cm)
sdHole <- sd(holeSize$holeDiameter_cm)
pDefective03 <- pnorm(10-0.3, meanHole, sdHole, lower.tail = TRUE) + pnorm(10+0.3, meanHole, sdHole, lower.tail = FALSE)
pDefective03
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the hole diameters of the samples to see what the distribution looks like.",
              incorrect_msg = "Make sure to draw the histogram for the hole diameters. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")

test_object("pDefective03", undefined_msg = "Make sure to define an object `pDefective03`.",
            incorrect_msg = "Make sure that you calculated the probability of a hole that is drilled by the machine falling outside the 0.3cm tolerance correctly, and assigned the result to `pDefective03`. You have to use the probability distribution function to calculate `pDefective03`. Do not use the sample data. To use the probability distribution function you need to calculate its key parameters using the sample data, thereafter use the appropriate R function.")     

test_output_contains("pDefective03", incorrect_msg = "You did not view the actual value of  `pDefective03`.")

success_msg("Correct! The data seems to follow a normal distribution, but there are other distributions that also have the same shape, specifically the $t$-distribution. In the next questions we are going to perform a goodness-of-fit test to see if the normal, $t$ or uniform distributions are good representations of hole-size.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:91698a409b
## Drill-hole: goodness-of-fit for the uniform distribution part 1

The first distribution that we are going to test is the uniform distribution, even though we are certain that the drilling holes do not follow this distribution. Knowing the answer in advance is useful when mastering new techniques since we can easily check if the answer from our techniques make sense.

We will first perform the goodness-of-fit test by manually calculating the $\chi^2$ value of our sample, compared to the expected uniform distribution.

Recall that for the $\chi^2$ goodness-of-fit test we work with bins, and compare the number of observed cases in each bin with the expected number of cases should our variable follow a certain distribution. Importantly, for continuous data we need to decide on the number of bins. We can do so by drawing a histogram of the variable, using the `hist` function, and then change the number of breaks in the histogram. Even better, by assigning the histogram to an object, R can automatically return the number of observations for each interval, thus we don't have to do it manually. The following code illustrates this process:

```
h <- hist(someVariable)
h$counts # this gives the number of cases in each bin of the histogram
hCounts <- h$counts # now we can use the number of counts.
```

Using the above code we can change the number of breaks in the histogram, assign the histogram to $h$ and use `h$counts` to get the count per bin.

To calculate the $\chi^2$ value we can use the following formula:

$$\chi^2 = \sum\_{i=1}^{k}\frac{(O\_{i}-E\_{i})^2}{E\_{i}},$$

where $k$ is the number of bins, $O\_{i}$ is the observed number of cases in bin $i$ and $E\_{i}$ is the expected number of cases in bin $i$ for the expected distribution.

Once we have our $\chi^2$ value we can calculate the probability of getting this value, or greater, using `pchisq(q, df, lower.tail = FALSE)` which takes as input the $\chi^2$ value, `q`, degrees-of-freedom, `df, and wether the lower (left) or upper (right) tail value should be returned. For the $\chi^2$-test the upper tail value should be returned, hence `lower.tail = FALSE`.

If the probability of getting the $\chi^2$ value is very small, we conclude that there is sufficient evidence that the variable *DOES NOT* follow the expected distribution. If $\chi^2$ is big, we say there is not sufficient evidence to discard the distribution.

Using the data available in the `holeSize` dataframe, complete this question do the following:

*** =instructions

1. Draw a histogram of the hole-size and set the number of breaks to 9 (this should give you a histogram with 10 bins).
2. Redraw the histogram bust this time assign it to the object `h`.
3. View the object `h` to make sure that it is the histogram with 10 bins.
4. View the number of observations in each bin of the histogram by printing `h$counts` to the console.
5. Assign the number of observations in each bin to `hCounts`.
6. Since we assume that hole size will follow a uniform distribution, how many cases do we expect in each bin? Hint: first determine the total number of cases, then calculate how many cases do we expect to be in each of the 10 bins. Assign your answer to `expectedCount`. Do not round your answer.
7. Using `hCounts` and `expectedCount`, calculate the $chi^2$ value and assign your answer to `chiSqr`. Hint: the following R formulate can be used `sum((hCounts - expectedCount)^2/expectedCount).`
8. Calculate the degrees-of-freedom for the test and assign your answer to `df`.
9. Calculate the $p$-value for the test and assign you answer to `p`.
10. View the `p` value and decide for yourself whether the null-hypothesis for the test should be rejected.  Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.

*** =hint

If you don't know how to calculate the chi-squared value or the degrees-of-freedom, review _Section 3.3: Testing of goodness of fit using chi-square (p.134)_ of the prescribed textbook by Diez et al (2014). Thereafter carefully go through all the background and instructions.

*** =pre_exercise_code
```{r}
set.seed(35)
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

# 1. Draw a histogram of the hole-size and set the number of breaks to 9 (this should give you a histogram with 10 bins).



#2. Redraw the histogram but this time assign it to the object `h`.

h <-

#3. View the object `h` to make sure that it is the histogram with 10 bins.



#4. View the number of observations in each bin of the histogram by printing `h$counts` to the console.



# 5. Assign the number of observations in each bin to `hCounts`.



# 6. Since we assume that hole size will follow a uniform distribution, how many cases do we expect in each bin? Hint: first determine the total number of cases, then calculate how many cases do we expect to be in each of the 10 bins. Assign your answer to `expectedCount`. Do not round your answer.



#7. Using `hCounts` and `expectedCount`, calculate the chi-squared value and assign your answer to `chiSqr`. Hint: the following R formulate can be used `sum((hCounts - expectedCount)^2/expectedCount).`



#8. Calculate the degrees-of-freedom for the test and assign your answer to `df`.



#9. Calculate the p-value for the test and assign you answer to `p`.



#10. View the `p` value and decide for yourself whether the null-hypothesis for the test should be rejected.  Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.



```

*** =solution
```{r}
hist(holeSize$holeDiameter_cm, breaks = 9)
h <- hist(holeSize$holeDiameter_cm, breaks = 9)
h
h$counts
hCounts <- h$counts
expectedCount <- nrow(holeSize)/10
chiSqr <- sum((hCounts - expectedCount)^2/expectedCount)
df <- 10 - 1
p <- pchisq(chiSqr, df, lower.tail = FALSE)
p
if(p < 0.05){rejectH0 <- TRUE}else{rejectH0 <- FALSE}
```

*** =sct
```{r}
test_function("hist", args = c("x", "breaks"), not_called_msg = "Draw a histogram with 9 breaks of the hole diameters of the samples to see what the distribution looks like.",
              incorrect_msg = "Make sure to draw the histogram with 9 breaks for the hole diameters.")

test_object("h", undefined_msg = "Make sure to define an object `h`.",
            incorrect_msg = "Make sure that you assigned the histogram with 9 breaks to the object `h`. Note that you have to call `hist` twice, once to view the histogram and the second time to assign it to `h`")     

test_output_contains("h", incorrect_msg = "You did not view the `h` to see if it's the same as the histogram.")

test_output_contains("h$counts", incorrect_msg = "You did not view the number of cases in each histogram bin using `h$counts`.")

test_object("hCounts", undefined_msg = "Make sure to define an object `hCounts`.",
            incorrect_msg = "Make sure that you assigned the number of counts for each bin to the object `hCounts`. Note that you have to call `h$counts` twice, once to view the number of counts and the second time to assign it to `hCounts`")     

test_object("expectedCount", undefined_msg = "Make sure to define an object `expectedCount`.",
            incorrect_msg = "Make sure that you calculated the expected number of counts in each of the 10 bins correctly and assigned it to `expectedCount`. You need to use the total number of samples in our calculation.")

test_object("chiSqr", undefined_msg = "Make sure to define an object `chiSqr`.",
            incorrect_msg = "Make sure that you calculated the chi-square test statistic correctly and assigned your answer to `chiSqr`.")

test_object("df", undefined_msg = "Make sure to define an object `df`.",
            incorrect_msg = "Make sure that you calculated the degrees-of-freedom correctly and assigned your answer to `df`. Note that the degrees-of-freedom is NOT based on the number of samples. Review _Section 3.3: Testing of goodness of fit using chi-square (p.134)_ of the prescribed textbook by Diez et al (2014).")

test_object("p", undefined_msg = "Make sure to define an object `p`.",
            incorrect_msg = "Make sure that you calculated the p-value for the test statistic correctly using `pchisq()` assigned your answer to `p`.")

test_output_contains("p", incorrect_msg = "You did not view the actual value of  `p`.")

test_object("rejectH0", undefined_msg = "Make sure to define a variable `rejectH0`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectH0`. View the p-value and then decide for yourself if `rejectH0<-TRUE` or `rejectH0<-FALSE`.")

success_msg("Correct! As expected the uniform distribution is not a good fit for hole-size, resulting in a very small $p$-value that allows us to reject $H_0$ for the goodness-of-fit hypothesis test. In the next section we are quickly going to redo the question using R's built in functions.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2056a3af3a
## Drill-hole: goodness-of-fit for the uniform distribution part 2

For this question we are going to perform the $\chi^2$ goodness-of-fit test using R's built in function `chisq.test`. At a minimum the function requires `x` which is the observed counts per bin. If nothing else supplied, the function assumes that the expected distribution is uniform , which in this case, is exactly what we want to test. To perform the test and view it's outputs we will simply determine the number of observations per bin in the histogram, and then perform the test and view it's outputs by calling the `chisq.test` function.

*** =instructions

1. Draw a histogram of of the hole-size variable with 9 breaks and assign the histogram to `h`.
2. Using `h$counts`, determine the number of counts per bin and assign the result to `hCount`.
3. Perform the goodness-of-fit test by calling `chisq.test(hCount)` and view the results.

** =hint

Go through the previous exercise to see how `hCount` can be calculated. Use `?chisq.test` to find out more about the function.

*** =pre_exercise_code
```{r}
set.seed(35)
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

#1. Draw a histogram of of the hole-size variable with 9 breaks and assign the histogram to `h`.



#2. Using `h$counts`, determine the number of counts per bin and assign the result to `hCount`.



#3. Perform the goodness-of-fit test by calling `chisq.test(hCount)` and view the results.



```

*** =solution
```{r}
h <- hist(holeSize$holeDiameter_cm, breaks = 9)
hCounts <- h$counts
chisq.test(hCounts)
```

*** =sct
```{r}
test_object("h", undefined_msg = "Make sure to define an object `h`.",
            incorrect_msg = "Make sure that you assigned the histogram with 9 breaks to the object `h`. Note that you have to call `hist` twice, once to view the histogram and the second time to assign it to `h`")

test_object("hCounts", undefined_msg = "Make sure to define an object `hCounts`.",
            incorrect_msg = "Make sure that you assigned the number of counts for each bin to the object `hCounts`. Note that you have to call `h$counts` twice, once to view the number of counts and the second time to assign it to `hCounts`")

test_function("chisq.test", args = c("x"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function correctly. In this question, all it needs is the counts per bin.")

success_msg("Correct! By using the `chisq.test` we can easily perform the Goodness-of-fit test. In the next question we are going to repeat the test for the normal and t-distribution.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5df974f21f
## Drill-hole: goodness-of-fit for the normal distribution

Since we expect hole-size to follow a normal distribution, it makes sense to do a $\chi^2$ goodness-of-fit test using this distribution. To do so we can use the `chisq.test`, but in this case we have to supply more than the observed cases per bin. We also need to find the expected number of cases per bin should the variable follow a normal distribution. To do so is not trivial, but fortunately there are pre-existing code that we can use for this purpose.

To test for other distributions, the `chisq.test` function needs as input `p`, which is the probability of an observation falling within each bin. Note that unlike the uniform distribution, the probability will be different for each bin. If we know the value of our bin break-points, and the sample mean and standard deviation for the normal distribution, the following code can be used as-is to calculate the bin probabilities for a normal distribution:

```
null.probs <- diff(pnorm(h$breaks, mean, sd))
```

where `h` is the histogram of our sample data assigned to the object `h`.

Thereafter we can call the `chisq.test` as follows:

```
chisq.test(x = h$counts, p = null.probs, rescale.p=TRUE, simulate.p.value=TRUE)
```

To complete the question, do the following:

*** =instructions

1. Calculate the mean and standard deviation for the drill-hole size and assign your answer to `meanHoleSize` and `sdHoleSize`.
2. Draw a histogram of the hole-size variable with *100* breaks and assign the histogram to `h1`.
3. Calculate the bin probabilities for 100 bins by calling `null.probs1 <- diff(pnorm(h1$breaks, meanHoleSize, sdHoleSize))`.
4. Draw a barplot of the probabilities, using `barplot(null.probs)`, to see that the bin probabilities follow a normal distribution.
5. Draw a histogram of the hole-size variable with *9* breaks and assign the histogram to `h2`.
6. Calculate the bin probabilities for 9 bins by calling `null.probs2 <- diff(pnorm(h2$breaks, meanHoleSize, sdHoleSize))`.
7. Perform the goodness-of-fit test by calling `chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE, simulate.p.value=TRUE)` and view the results.
8. Based on the output of the test decide for yourself whether the null-hypothesis for the test should be rejected.  Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.
9. Perform the goodness-of-fit test this time using the 100 bin histogram by calling `chisq.test(x = h1$counts, p = null.probs1, rescale.p=TRUE, simulate.p.value=TRUE)` and note the difference in the results.

*** =hint

Go through the previous exercise to see how `hCounts` can be calculated. Use `?chisq.test` to find out more about the function. Refer to the class slides.

*** =pre_exercise_code
```{r}
set.seed(35)
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```

*** =sample_code
```{r}
# The data are available in the holeSize dataframe.

#1. Calculate the mean and standard deviation for the drill-hole size and assign your answer to `meanHoleSize` and `sdHoleSize`.



#2. Draw a histogram of the hole-size variable with *100* breaks and assign the histogram to `h1`.



#3. Calculate the bin probabilities for 100 bins by calling `null.probs1 <- diff(pnorm(h1$breaks, meanHoleSize, sdHoleSize))`.



#4. Draw a barplot of the probabilities, using `barplot(null.probs)`, to see that the bin probabilities follow a normal distribution.



#5. Draw a histogram of the hole-size variable with *9* breaks and assign the histogram to `h2`.



#6. Calculate the bin probabilities for 9 bins by calling `null.probs2 <- diff(pnorm(h2$breaks, meanHoleSize, sdHoleSize))`.



#7. Perform the goodness-of-fit test by calling `chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE, simulate.p.value=TRUE)` and view the results.



#8. Based on the output of the test decide for yourself whether the null-hypothesis for the test should be rejected.  Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.



#9. Perform the goodness-of-fit test this time using the 100 bin histogram by calling `chisq.test(x = h1$counts, p = null.probs1, rescale.p=TRUE, simulate.p.value=TRUE)` and note the difference in the results.



```

*** =solution
```{r}
meanHoleSize <- mean(holeSize$holeDiameter_cm)
sdHoleSize <- sd(holeSize$holeDiameter_cm)

h1 <- hist(holeSize$holeDiameter_cm, breaks = 100)
null.probs1 <- diff(pnorm(h1$breaks, meanHoleSize, sdHoleSize))
plot(null.probs1)

h2 <- hist(holeSize$holeDiameter_cm, breaks = 9)
null.probs2 <- diff(pnorm(h2$breaks, meanHoleSize, sdHoleSize))

chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE, simulate.p.value=TRUE)

rejectH0 <- chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE, simulate.p.value=TRUE)$p.value < 0.05

chisq.test(x = h1$counts, p = null.probs1, rescale.p=TRUE, simulate.p.value=TRUE)
```

*** =sct
```{r}
test_object("meanHoleSize", undefined_msg = "Make sure to define an object `meanHoleSize`.",
            incorrect_msg = "Make sure that you calculated the mean hole size correctly and assigned your answer to `meanHoleSize`")     

test_object("sdHoleSize", undefined_msg = "Make sure to define an object `sdHoleSize`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of hole size correctly and assigned your answer to `sdHoleSize`")

test_object("h1", undefined_msg = "Make sure to define an object `h1`.",
            incorrect_msg = "Make sure that you assigned the histogram with *100* breaks to the object `h1`.")     

test_object("null.probs1", undefined_msg = "Make sure to define an object `null.probs1.",
            incorrect_msg = "Make sure that you calculate `null.probs1` using the code given.")     

test_object("h2", undefined_msg = "Make sure to define an object `h2`.",
            incorrect_msg = "Make sure that you assigned the histogram with *9* breaks to the object `h2`.")     

test_object("null.probs1", undefined_msg = "Make sure to define an object `null.probs2.",
            incorrect_msg = "Make sure that you calculate `null.probs2` using the code given.")    

test_function("chisq.test", args = c("x", "p", "rescale.p", "simulate.p.value"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` on function on `h2` as described.")

test_object("rejectH0", undefined_msg = "Make sure to define a variable `rejectH0`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectH0`. View the p-value and then decide for yourself if `rejectH0<-TRUE` or `rejectH0<-FALSE`.")

test_function("chisq.test", args = c("x", "p", "rescale.p", "simulate.p.value"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function on `h1` as described.")

success_msg("Correct! Using the built in functions and given code we can easily perform the Goodness-of-fit test for any of R's built in distribution. But note how our choice of bin size influences the results. Care has to be taken to set the bin-size appropriately. If there are too many bins, all distributions may be rejected. If there are too few, say two bins, then we aren't really checking for a continuous distribution anymore.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a483221d79
## Drill-hole: parameter estimation using bootstrapping

In the previous question we have formally tested the goodness-of-fit for the normal distribution, and found that due to the large $p$-value of the $\chi^2$ test statistic we cannot reject $H\_0$, which is that hole-size follows a normal distribution with a mean and standard deviation equal to that of the sample data. If we were to repeat the question with a new sample, we would get a different sample mean and standard deviation, even though the process output follows the same distribution.

Ideally we would like to calculate confidence intervals for the distribution parameters, and thereby make our analyses of the process output more robust. In this question we will do exactly that by using bootstrapping to calculate key distribution parameters.

The basic component of bootstrapping is as follows. Using the sample data we will do the following:

1. Sample from the sample data with replacement. The number of samples from the samples will be equal to the original sample size.
2. Calculate the parameters of interests on the sample of samples.
3. Repeat steps 1 and 2 a large number of times and capture the key parameters of interest each time. Each sample from the sample can be considered a simulation, or a bootstrap.
4. Statistically analyse the distribution of the parameters captured over the multiple runs of step 3.

The following R functions will come in handy when doing bootstrapping:

```
rep, for, sample, quantile
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
6. Use a `for` loop and repeat the `sample` function to take a sample with replacement from the hole-size samples 10000 times. In each execution of the `for` loop you can assign the results to `holeDiameter_boot_test`. Calculate the mean and standard deviation of `holeDiameter_boot` and store the results in the appropriate `i` position of `holeMeans` and `holeSD`. Hint: if you are unsure how to do this, have a look at the previous datacamp courses.
7. Draw histograms of `holeMeans` and `holeSD`.
8. Use the `quantile` function to calculate 95% confidence intervals for `holeMeans` and `holeSD` using the `quantile` function and assign the results to `meanInter` and `sdInter`. Hint: for the mean hole-size the code will be `meansInter <- quantile(holeMeans, prob = c(0.025, 0.975))`
9. View `meanInter` and `sdInter` by printing their values to the console.
10. For a worst-case scenario we can use the `holeMeansInter` value that is the furthest away from the required drill hole size of 10cm, and the `sdInter` value that is the largest as the mean and standard deviation of the drill-hole size normal distribution. Using the appropriate two values, calculate the probability of a hole falling *outside* a tolerance limit of 0.5cm. For the calculations, use `holeMeansInter` and `sdInter` directly by calling either `holeMeansInter[1]` or `holeMeansInter[2]`, depending on which can be considered as the worst-case option, and the same with `sdInter`. Assign your answer to `pDefective05`. Do not use the values displayed in the terminal. They are not accurate enough. Hint: use the `pnorm` function and remember that you will have to call it twice.
11. View the value of `pDefective05` by printing it to the console.

*** =hint

* Make sure you understand how the `sample` function works in R. Review the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) datacamp chapter if necessary.
* Make sure you understand how the `for` function works in R, and how it is used to store simulation run outputs in a vector. Review the [Foundations for inference: Sampling distributions](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-3a-foundations-for-inference-sampling-distributions?ex=1) datacamp chapter if necessary.
* Go through _Section 4.5: Bootstrapping for the standard deviation (p.195)_ of the prescribed textbook by Diez et al (2014).

*** =pre_exercise_code
```{r}
set.seed(35)
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

#10. For a worst-case scenario we can use the `holeMeansInter` value that is the furthest away from the required drill hole size of 10cm, and the `sdInter` value that is the largest as the mean and standard deviation of the drill-hole size normal distribution. Using the appropriate two values, calculate the probability of a hole falling *outside* a tolerance limit of 0.5cm. For the calculations, use `holeMeansInter` and `sdInter` directly by calling either `holeMeansInter[1]` or `holeMeansInter[2]`, depending on which can be considered as the worst-case option, and the same with `sdInter`. Assign your answer to `pDefective05`. Do not use the values displayed in the terminal. They are not accurate enough. Hint: use the `pnorm` function and remember that you will have to call it twice.



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

--- type:NormalExercise lang:r xp:100 skills:1 key:8baad0d53b
## Gautrain arrival rate: fitting the poisson distribution

In the last questions we are going to analyse the number of passengers arriving _per minute_ from the Gautrain between 16:00 and 18:00PM.
This information is necessary to determine the number of outbound access gates to activate during the peak-arrival time.
Recall that the Gautrain can easily switch  an access gate from inbound to outbound.
Approximately _two_ passengers can go through the gate per minute, and the station is considering having only _two_ outbound access-gates open during the peak-time.
The question that we need to answer is what is the probability of the two outbound access-gates being insufficient?
Insufficient means that the number of passengers arriving during the minute will be more than the access gates can process.

The arrival rate per minute of passengers between 16:00 and 18:00PM for the previous 30 working days can be found in the `nArrivePerMin` vector.

In this question we are going to fit a distribution to the arrivals per minute.

*** =instructions

1. Draw a histogram of the arrivals per minute and check if it follows a poisson distribution.
2. Calculate the mean arrival rate, $\lambda$, and assign your answer to `arriveRate.`
3. Perform a $\chi^2$ goodness-of-fit test for poisson distribution, similar to performing the test for the normal distribution. The steps to do this include: assigning a histogram to `h` (do not manually specify the number of breaks); determine the expected probability, `null.probs`, for each bin of a poisson distribution using `diff(ppois(...))`; perform the test using `chisq.test(...)` function and view the results. It should also help to draw a barplot of `null.probs` to see if you used `diff(ppois(...))` correctly.
4. Based on the output of the test decide for yourself whether the data do no follow a poisson distribution with rate equal to $\lambda$ .  Your answer should be either `TRUE` for _we reject the null hypothesis_, therefore the data do not follow a poisson distribution, or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectPoisson` variable.

*** =hint

The steps required to complete this question is the same as the previous questions.

*** =pre_exercise_code
```{r}
set.seed(31)
nArrivePerMin <- rpois(30*120, 5)
```

*** =sample_code
```{r}
# The arrivals per minute are available in the `nArrivePerMin` vector.

#1. Draw a histogram of the arrivals per minute and check if it follows a poisson distribution.



#2. Calculate the mean arrival rate, $\lambda$, and assign your answer to `arriveRate.`



#3. Perform a chi^2 goodness-of-fit test for poisson distribution, similar to performing the test for the normal distribution. The steps to do this include:

#assigning a histogram to `h` (do not manually specify the number of breaks);



#determine the expected probability, `null.probs`, for each bin of a poisson distribution using `diff(ppois(...))`;



#perform the test using `chisq.test(...)` function and view the results. It should also help to draw a barplot of `null.probs` to see if you used `diff(ppois(...))` correctly.



#4. Based on the output of the test decide for yourself whether the data do no follow a poisson distribution with rate equal to lambda .  Your answer should be either `TRUE` for _we reject the null hypothesis_, therefore the data do not follow a poisson distribution, or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectPoisson` variable.



```

*** =solution
```{r}
hist(nArrivePerMin)
arriveRate <- mean(nArrivePerMin)
h <- hist(nArrivePerMin)
null.probs <- diff(ppois(h$breaks, arriveRate))
chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE, simulate.p.value = TRUE)
chiTestResults <- chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE, simulate.p.value = TRUE)
if (chiTestResults$p.value < 0.05){rejectPoisson <- TRUE}else{rejectPoisson <- FALSE}
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the number of arrivals per minute `nArrivePerMin`.",
              incorrect_msg = "Draw a histogram of the number of arrivals per minute `nArrivePerMin`.")

test_object("arriveRate", undefined_msg = "Make sure to define an object `arriveRate`.",
            incorrect_msg = "Make sure that calculated the mean arrival rate correctly and assigned your answer to `arriveRate`.")     

test_function("chisq.test", args = c("x", "p", "rescale.p", "simulate.p.value"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function. Refer to the previous questions on instructions.")  

test_object("rejectPoisson", undefined_msg = "Make sure to define a variable `rejectPoisson`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectPoisson`. View the p-value fromo the chi-square test and then decide for yourself if `rejectPoisson<-TRUE` or `rejectPoisson<-FALSE`.")

success_msg("Correct! The chi-squared test result shows that we cannot discard the poisson distribution, but note that our p-value was quite small. If we altered the number of bins of our original histogram we may have ended up rejecting H0. The chi-squared test can be a bit unreliable when dealing with the poisson distribution, especially if the arrival rate is low. For larger arrival rates the test is more robust. This may have to do with the long tail of the normal distribution. Recall that for the chi-squared test there has to be at least 5 expected cases per bin, which does not always hold for the right tail of the poisson distribution. We have to be careful when choosing our number of bins to make sure that all the conditions for the chi-squared test are met.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:26ce6147d6
## Gautrain arrival rate: parameter estimation using bootstrapping

In the previous question we have formally tested the goodness-of-fit for the poisson distribution and found that we cannot reject $H_0$. We can therefore model the number passenger arrivals per minute, $X$, using a poisson distribution, such that:

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
