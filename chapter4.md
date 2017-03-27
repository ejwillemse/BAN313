---
title_meta  : Case study 7
title       : Case study 7 - Fitting probability distributions and estimating distribution parameters
description : "In this case study we revisit the examples from Case study 6, where we analysed data for different processes, and eyeballed the data to see if the processes can be modelled using specific probability distribution functions. In this case study we will use formal methods to test if a distribution is appropriate, using the chi-squared goodness-of-fit test. We will also use boot-strapping methods to estimate model parameters."

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:8eaf7b9ebd
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

$$\chi^2 = \sum\_{i=1}^{k}\frac{(O\_{i}-E\_{i})^2}{E\_{i}}$$,

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
