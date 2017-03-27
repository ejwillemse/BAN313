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

When completing the chapter, read the all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip the preceding question background. The question background contains valuable information to assist you in completing the question.

*** =instructions

* I confirm that I have completed the prescribed preparation material, as listed in the 7 tasks above, and have read **ALL** the instructions on this page carefully.

Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material.

*** =hist

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_success <- "Let's get started with the Lab. Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."
test_mc(correct = 1, feedback_msgs = c(msg_success))
```
