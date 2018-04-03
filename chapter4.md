---
title_meta  : Case study 7
title       : Case study 7 - Fitting probability distributions using chi-squared goodness-of-fit test
description : "In this case study we revisit the examples from Case study 6, where we analysed data for different processes, and eyeballed the data to see if the processes can be modelled using specific probability distribution functions. In this case study we will use formal methods to test if a distribution is appropriate, using the chi-squared goodness-of-fit test."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In this case study we will revisit the examples of [Case study 6: Different applications of probability distributions](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) and use formal methods to test whether the processes can be modelled using specific probability distribution functions.

**Before continuing with this chapter, please do the following:**

1. Complete [Case study 6: Different applications of probability distributions](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) on Datacamp.
2. Make sure you understand how the `sample` function works in R. Review the [Probability](https://campus.datacamp.com/courses/statistical-inference-and-data-analysis/lab-2-probability?ex=1) datacamp chapter if necessary.
3. Review _Section 3.3: Testing of goodness of fit using chi-square (p.134)_ of the prescribed textbook by Diez et al (2014).
4. Go through the goodness-of-fit using R tutorial, available from [this link](https://www.r-bloggers.com/goodness-of-fit-test-in-r/).
5. Review the lecture slides `Lecture_Week8.pdf`.

The prescribed textbook is:

* Diez, D. M., Barr, C. D., and Çetinkaya Rundel, M. (2014). _Introductory Statistics With Randomization and Simulation_. CreateSpace Independent Publishing Platform. Available at [this link](https://www.openintro.org/stat/textbook.php?stat_book=isrs), 1st edition.

**After completing the above, read the below instructions carefully.**

In this lab we will look at the same examples from Case study 6, and do the following:

1. Visualise the process data using histograms and guess what distribution can be used to model the process.
2. Use the statistical chi-square goodness-of-fit test to formally test if the distribution is appropriate.

The specific R functions that are applicable to this chapter are:

```
pnorm(), ppois(), pexp(), punif(), pt(), pchisq()
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
## Drill-hole sizes

We wish to replace our current very old drilling machine.
The machine is required to drill a hole with a diameter of 10cm within a 0.5cm tolerance.
If the hole is larger than 10.5cm, the part has to be scrapped.
Similarly, if the hole is smaller than 9.5cm the part will also be scrapped.

We also want to use the machine to drill holes for other products that have different tolerances.
One product may have a 0.25cm tolerance, another 1cm.

We have identified a possible replacement machine, and its supplier assured us that it will perform according to specifications.
We insisted that the supplier provide us with data on its performance, so that we can make a more informed decision on whether it will meet our requirements.

The supplier has given us data on different holes drilled by the machine during its testing.
The data consist of a number of drill samples, with the hole diameter for each hole measured and captured.
The data is available in the `holeSize` dataframe.

For the first part of this question, analyse the distribution of hole sizes from the machine and identify the distribution that best describes the hole-sizes. Find the key parameters from the sample for the distribution. Use R's built-in function and the _distribution_ (not the sample data) to find the probability of a hole drilled by the machine to fall outside a **0.30cm** tolerance limit.

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

success_msg("Correct! The data seems to follow a normal distribution, but there are other distributions that also have the same shape. In the next questions we are going to perform a goodness-of-fit test to see if the normal or uniform distributions are good representations of hole-size.")
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

Once we have our $\chi^2$ value we can calculate the probability of getting this value, or greater, using `pchisq(q, df, lower.tail = FALSE)` which takes as input the $\chi^2$ value, `q`, degrees-of-freedom, `df`, and wether the lower (left) or upper (right) tail value should be returned. For the $\chi^2$-test the upper tail value should be returned, hence `lower.tail = FALSE`.

If the probability of getting the $\chi^2$ value is very small, we conclude that there is sufficient evidence that the variable *DOES NOT* follow the expected distribution. If $\chi^2$ is big, we say there is not sufficient evidence to discard the distribution.

Using the data available in the `holeSize` dataframe, complete this question by doing the following:

*** =instructions

1. Draw a histogram of the hole-size and set the number of breaks to 9 (this should give you a histogram with 10 bins).
2. Redraw the histogram bust this time assign it to the object `h`.
3. View the object `h` to make sure that it is the histogram with 10 bins.
4. View the number of observations in each bin of the histogram by printing `h$counts` to the console.
5. Assign the number of observations in each bin to `hCounts`.
6. Since we assume that hole size will follow a uniform distribution, how many cases do we expect in each bin? Assign your answer to `expectedCount`. Do not round your answer. Hint: first determine the total number of cases, then calculate how many cases do we expect to be in each of the 10 bins.
7. Using `hCounts` and `expectedCount`, calculate the $\chi^2$ value and assign your answer to `chiSqr`. Hint: the following R formulate can be used `sum((hCounts - expectedCount)^2/expectedCount).`
8. Calculate the degrees-of-freedom for the test and assign your answer to `df`.
9. Calculate the $p$-value for the test and assign you answer to `p`.
10. View the `p` value and decide for yourself whether the null-hypothesis for the test should be rejected.  Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.

*** =hint

If you don't know how to calculate the chi-squared value or the degrees-of-freedom, review _Section 3.3: Testing of goodness of fit using chi-square (p.134)_ of the prescribed textbook by Diez et al (2014). Thereafter carefully go through all the background information and instructions.

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



# 6. Since we assume that hole size will follow a uniform distribution, how many cases do we expect in each bin? Assign your answer to `expectedCount`. Do not round your answer. Hint: first determine the total number of cases, then calculate how many cases do we expect to be in each of the 10 bins.



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

For this question we are going to perform the $\chi^2$ goodness-of-fit test using R's built in function `chisq.test`. At a minimum the function requires `x` which is the observed counts per bin. If nothing else is supplied, the function assumes that the expected distribution is uniform , which in this case, is exactly what we want to test. To perform the test and view it's outputs we will simply determine the number of observations per bin in the histogram, and then perform the test and view it's outputs by calling the `chisq.test` function.

*** =instructions

1. Draw a histogram of of the hole-size variable with 9 breaks and assign the histogram to `h`.
2. Using `h$counts`, determine the number of counts per bin and assign the result to `hCounts`.
3. Perform the goodness-of-fit test by calling `chisq.test(hCounts)` and view the results.

*** =hint

Go through the previous exercise to see how `hCounts` can be calculated. Use `?chisq.test` to find out more about the function.

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



#2. Using `h$counts`, determine the number of counts per bin and assign the result to `hCounts`.



#3. Perform the goodness-of-fit test by calling `chisq.test(hCounts)` and view the results.



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

success_msg("Correct! By using the `chisq.test` we can easily perform the Goodness-of-fit test. In the next question we are going to repeat the test for the normal distribution.")
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
chisq.test(x = h$counts, p = null.probs, rescale.p=TRUE)
```

Note that in the above `simulate.p.value=FALSE`. In practice, we would call `chisq.test(x = h$counts, p = null.probs, rescale.p=TRUE, simulate.p.value=TRUE`), but this does take some computational effort, which some older PCs cannot cope with. Therefore, in this course we will set `simulate.p.value=FALSE`, which is the default when calling `chisq.text()`.

To complete the question, do the following:

*** =instructions

1. Calculate the mean and standard deviation for the drill-hole size and assign your answer to `meanHoleSize` and `sdHoleSize`.
2. Draw a histogram of the hole-size variable with *100* breaks and assign the histogram to `h1`.
3. Calculate the bin probabilities for 100 bins by calling `null.probs1 <- diff(pnorm(h1$breaks, meanHoleSize, sdHoleSize))`.
4. Draw a barplot of the probabilities, using `barplot(null.probs1)`, to see that the bin probabilities follow a normal distribution.
5. Draw a histogram of the hole-size variable with *9* breaks and assign the histogram to `h2`.
6. Calculate the bin probabilities for 9 bins by calling `null.probs2 <- diff(pnorm(h2$breaks, meanHoleSize, sdHoleSize))`.
7. Perform the goodness-of-fit test by calling `chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE)` and view the results.
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



#7. Perform the goodness-of-fit test by calling `chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE)` and view the results.



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

rejectH0 <- chisq.test(x = h2$counts, p = null.probs2, rescale.p=TRUE)$p.value < 0.05

chisq.test(x = h1$counts, p = null.probs1, rescale.p=TRUE)
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

test_function("chisq.test", args = c("x", "p", "rescale.p"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` on function on `h2` as described.")

test_object("rejectH0", undefined_msg = "Make sure to define a variable `rejectH0`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectH0`. View the p-value and then decide for yourself if `rejectH0<-TRUE` or `rejectH0<-FALSE`.")

test_function("chisq.test", args = c("x", "p", "rescale.p"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function on `h1` as described.")

success_msg("Correct! Using the built in functions and given code we can easily perform the Goodness-of-fit test for any of R's built in distributions. But note how our choice of bin size influences the results. Care has to be taken to set the bin-size appropriately. If there are too many bins, all distributions may be rejected. If there are too few, say two bins, then we aren't really checking for a continuous distribution anymore.")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:8baad0d53b
## Gautrain arrival rate: fitting the poisson distribution

In the last questions we are going to analyse the number of passengers arriving _per minute_ from the Gautrain between 16:00 and 18:00PM.
The arrival rate per minute of passengers between 16:00 and 18:00PM for the previous 30 working days can be found in the `nArrivePerMin` vector.
In this question we are going to fit a distribution to the arrivals per minute.

*** =instructions

1. Draw a histogram of the arrivals per minute and check if it follows a poisson distribution.
2. Calculate the mean arrival rate, $\lambda$, and assign your answer to `arriveRate.`
3. Perform a $\chi^2$ goodness-of-fit test for poisson distribution, similar to performing the test for the normal distribution. The steps to do this include: assigning a histogram to `h` (do not manually specify the number of breaks); determine the expected probability, `null.probs`, for each bin of a poisson distribution using `diff(ppois(...))`; perform the test using `chisq.test(...)` function and view the results. It should also help to draw a barplot of `null.probs` to see if you used `diff(ppois(...))` correctly.
4. Based on the output of the test decide for yourself whether the data do not follow a poisson distribution with rate equal to $\lambda$ .  Your answer should be either `TRUE` for _we reject the null hypothesis_, therefore the data do not follow a poisson distribution, or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectPoisson` variable.

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
chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE)
chiTestResults <- chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE)
if (chiTestResults$p.value < 0.05){rejectPoisson <- TRUE}else{rejectPoisson <- FALSE}
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the number of arrivals per minute `nArrivePerMin`.",
              incorrect_msg = "Draw a histogram of the number of arrivals per minute `nArrivePerMin`.")

test_object("arriveRate", undefined_msg = "Make sure to define an object `arriveRate`.",
            incorrect_msg = "Make sure that calculated the mean arrival rate correctly and assigned your answer to `arriveRate`.")     

test_function("chisq.test", args = c("x", "p", "rescale.p"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function. Refer to the previous questions on instructions.")  

test_object("rejectPoisson", undefined_msg = "Make sure to define a variable `rejectPoisson`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectPoisson`. View the p-value fromo the chi-square test and then decide for yourself if `rejectPoisson<-TRUE` or `rejectPoisson<-FALSE`.")

success_msg("Correct! The chi-squared test result shows that we cannot discard the poisson distribution, but note that our p-value was quite small. If we altered the number of bins of our original histogram we may have ended up rejecting H0. The chi-squared test can be a bit unreliable when dealing with the poisson distribution, especially if the arrival rate is low. For larger arrival rates the test is more robust. This may have to do with the long tail of the normal distribution. Recall that for the chi-squared test there has to be at least 5 expected cases per bin, which does not always hold for the right tail of the poisson distribution. We have to be careful when choosing our number of bins to make sure that all the conditions for the chi-squared test are met.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8baad0d53b
## Gautrain inter-arrival time: fitting the exponential distribution

Another distribution often used to model arrivals is the exponential distribution. In this case, instead of modelling the number of arrivals per time-interval, we measure the inter-arrival time between two consecutive arrivals. This is simply the time that we expect will elapse after a customer arrives, until the next customer arrive. For example, assume that the first customer one arrives at 60 seconds after the station opens, the second customer 98 seconds after the station opens, and the third customer 145 seconds after the station opens. The inter-arrival time between customer two and customer one is `98-60=38` seconds, and between the second and third customer is `145-98=47`.

Importantly, inter-arrival time is ALWAYS measured between two consecutive customers.

There is a direct link between arrival rate and the inter-arrival time. If, on average there 15 customers per minute (the arrival rate), then the average inter-arrival time of customers will be `15/60=0.25` minutes. This simply means that on average 0.25 minutes will pass after a customer arrives before the next customer will arrive. Often, the arrival rate can be modelled as a poisson distribution, in which case the inter-arrival time can be modelled as an exponential distribution. The exponential distribution is simply the inverse of the poisson distribution. In fact, its key parameter is the mean arrival rate, as calculated for the poisson distribution. This implies that $\lambda = \frac{1}{X}$ where $X$ is the mean inter-arrival time.

When building simulation models, it is important to be able to use both the poisson and exponential distributions to model arrivals. In this exercise we will repeat the previous exercise, this time working with the inter-arrival time of customers.

The arrival time in seconds for 120 customers, starting at time zero when the station opened, can be found in the `arrivalTimeSec` vector.
In this question we are first going to calculate the inter-arrival time of customers using the `diff()` function and the fit the exponential distribution to the inter-arrival time.

*** =instructions

1. Draw a histogram of `arrivalTimeSec` and note how it does not say much, the reason being that it gives the arrival time since the station opened.
2. Calculate the inter-arrival of customers using the `diff()` function and assign your answer to `interArriveSec`.
3. Draw a histogram of `interArriveSec` to confirm that it follows an exponential distribution.
2. Calculate the mean arrival rate, $\lambda$, and assign your answer to `arriveRate.` Hint: to do so, calculate the mean inter-arrival time, and use that to calculate the mean arrival rate. You will use this value in `pexp(...)`
3. Perform a $\chi^2$ goodness-of-fit test for exponential distribution, similar to performing the test for the normal distribution. The steps to do this include: assigning a histogram to `h` (do not manually specify the number of breaks); determine the expected probability, `null.probs`, for each bin of a exponential distribution using `diff(pexp(...))`; perform the test using `chisq.test(...)` function and view the results. It should also help to draw a barplot of `null.probs` to see if you used `diff(pexp(...))` correctly.
4. Based on the output of the test decide for yourself whether the data do not follow an exponential distribution with rate equal to $\lambda$.  Your answer should be either `TRUE` for _we reject the null hypothesis_, therefore the data do not follow an exponential distribution, or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectExp` variable.

*** =hint

The steps required to complete this question is the same as the previous questions.

*** =pre_exercise_code
```{r}
set.seed(31)
arrivalTimeSec <- cumsum(round(rexp(120, 5/60), 0))
```

*** =sample_code
```{r}
# The inter-arrival times are available in the `arrivalTimeSec` vector.

#1. Draw a histogram of ``arrivalTimeSec`.



#2. Calculate the inter-arrival of customers using the `diff()` function and assign your answer to `interArriveSec`.



#3. Draw a histogram of `interArriveSec` to confirm that it follows an exponential distribution.



#4. Calculate the mean arrival rate, $\lambda$, and assign your answer to `arriveRate.`



#5. Perform a chi^2 goodness-of-fit test for exponential distribution, similar to performing the test for the poisson distribution. The steps to do this include:

#assigning a histogram to `h` (do not manually specify the number of breaks);



#determine the expected probability, `null.probs`, for each bin of the exponential distribution using `diff(pexp(...))`;



#perform the test using `chisq.test(...)` function and view the results. It should also help to draw a barplot of `null.probs` to see if you used `diff(pexp(...))` correctly.



#4. Based on the output of the test decide for yourself whether the data do not follow an exponential distribution with rate equal to $\lambda$.  Your answer should be either `TRUE` for _we reject the null hypothesis_, therefore the data do not follow an exponential distribution, or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectExp` variable.



```

*** =solution
```{r}
hist(arrivalTimeSec)
interArriveSec <- diff(arrivalTimeSec)
hist(interArriveSec)
arriveRate <- 1/mean(interArriveSec)
h <- hist(interArriveSec)
null.probs <- diff(pexp(h$breaks, arriveRate))
chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE)
chiTestResults <- chisq.test(x = h$counts, p = null.probs, rescale.p = TRUE)
if (chiTestResults$p.value < 0.05){rejectExp <- TRUE}else{rejectExp <- FALSE}
```

*** =sct
```{r}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of arrival times `arrivalTimeSec`.",
              incorrect_msg = "Draw a histogram of arrival times `arrivalTimeSec`.")

test_object("interArriveSec", undefined_msg = "Make sure to define an object `interArriveSec`.",
              incorrect_msg = "Make sure that calculated the inter-arrival times correctly using the `diff` function and assigned your answer to `interArriveSec`.")  

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of inter-arrival times `interArriveSec`.",
              incorrect_msg = "Draw a histogram of the number of inter-arrival times `interArriveSec`.")

test_object("arriveRate", undefined_msg = "Make sure to define an object `arriveRate`.",
            incorrect_msg = "Make sure that you calculated the mean arrival rate correctly and assigned your answer to `arriveRate`.")     

test_function("chisq.test", args = c("x", "p", "rescale.p"), not_called_msg = "Use the built-in function `chisq.test` to perform the goodness-of-fit test.",
              incorrect_msg = "Make sure to use the `chisq.test` function. Refer to the previous questions on instructions.")  

test_object("rejectPoisson", undefined_msg = "Make sure to define a variable `rejectExp`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectExp`. View the p-value from the chi-square test and then decide for yourself if `rejectExp<-TRUE` or `rejectExp<-FALSE`.")

success_msg("Correct! The chi-squared test result shows that we cannot discard the exponential distribution.")
```
