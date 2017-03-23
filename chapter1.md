--- 
title_meta  : Case study 4
title       : Case study 4 - Choosing the best vendor
description : "In this case study we will compare two product suppliers, with the objective to choose the best one to partner with. 
For the case study, best means cheapest. To compare the vendors we will rely on a sample of products for which both vendors provided unit-price quotes. We will then use inference for numerical data to decide if one vendor is better than the other."

--- type:NormalExercise lang:r xp:0 skills:1 key:1c5a628f8f
## Background

In this case study we will compare two product suppliers, with the objective to choose the best one to partner with. For the case study, best means cheapest. To compare the vendors we will rely on a sample of products for which both vendors provided unit-price quotes. We will then use inference for numerical data to decide if one vendor is better than the other.

A critical component of supply chain management is building and maintaining an effective relationship with the companies within your supply chain. Choosing which companies to do business with is also an important component.

For this Case Study we are employed by a manufacturing company and we have to decide which supplier to partner with. The supplier has to supply components for our wide range of solar powered products. To complicate matters, the product range is continuously changing, and each new product requires new components. We therefore need to choose a supplier that has a very broad range of products available, and that has the ability to provide products upon request that are currently not in their catalogue. We do not know how much the company may charge for future products.

Currently there are only two suppliers, referred to as Company A and B (and sometimes just as A and B), that meet our requirements. We have to choose which Company to partner with. To compare the companies we have taken a random sample of our current product components, and asked both companies to provide a unit-price for each of the components. The goal is to use this data to infer if one company is better than the other.

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read the instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

To continue with this chapter, hit the 'Submit Answer' button.

*** =instructions

- Hit the 'Submit Answer' button when you're done reading the instructions.

*** =hint
Just hit the 'Submit Answer'.

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
success_msg("Let's try and use our knowledge on statistical inference to assist our company.")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:a9ac72e6f7
## Statistical methods to apply

Based on the case study description, which do you think is the correct statistical method to apply?

*** =instructions
- Inference for a single proportion.
- Inference for the difference between two proportions.
- Goodness-of-fit test.
- Test for independence.
- Inference for a single mean.
- Inference for paired numerical data.
- Inference for the difference between two means.

*** =hint
Have a look at the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}
#none
```

*** =sct
```{r}
msg_bad <- "That is not correct. Have a look at the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook."

msg_success <- "Correct! We want to compare the average unit-price for the two companies, so we are dealing with inference for numerical data. Both companies provided quotes for the same randomly sampled products, therefore we are dealing with paired-data. Why? Because each case involves the same product and consists of two prices, one from Company A and one from Company B. The reason for this design is that the companies may not be willing to give us a quotation for ALL their products, hence we randomly took a sample of products. To make the comparison fair we asked the companies to provide quotations for the same products. If we randomly sampled products for Company A and then randomly sampled products for Company B, one company may seem cheaper by virtue of the products for which they were required to provide a quote being low-cost products. Further, we do not know what the companies may charge for future products. We are therefore trying to infer average future prices using a sample of their current prices."
test_mc(correct = 6, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:cd4c8c8aa5
## R functions to apply

Which built in R function can we use to apply the correct statistical method? Use the console screen on the right to investigate the different functions through `?function` command.

*** =instructions
- `mean()`
- `sd()`
- `nrow()`
- `prop.test()`
- `t.test()`
- None of the above, we will have to write our own R code to apply the method.

*** =hint

Use the `?function` to find out more about each command.

*** =pre_exercise_code
```{r}
#none
```

*** =sct
```{r}
msg_bad <- "That is not correct. Remember that you can use the `?function` to find out more about each function."
msg_success <- "Correct! By setting `paired = TRUE` inside the function call to `t.test(x, y)` we can perform a t-test for paired numerical data. In the next exercises we will first semi-manually perform the test, and in the last exercise we use the `t.test` function."
test_mc(correct = 5, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:25c0c61261
## Analysing the data

The component price data has been loaded in the workspace as the `product_comparison` dataframe. It consists of three variables. The product code (`ProductCode`), the quoted unit-price for the component from Company A (`priceCompA`) and the quoted unit-price for the component from Company B (`priceCompB`). First have a look at the data in the console using the `head(product_comparison)` command, thereafter answer the following:

*** =instructions
1. How many products did we sample to get quotations for?
2. What is the sample mean for product prices from Company A?
3. What is the sample standard deviation for products price from Company A?
4. What is the sample mean for product prices from Company B?
5. What is the sample standard deviation for products price from Company B?

*** =hint
- Use `?nrow`, `?mean` and `?sd` to find out more about the functions that you can apply to answer the question.
- To isolate and analyse a specific variable, make use of the `data.frame$variableName` command.

*** =pre_exercise_code
```{r}
nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- round(runif(n, price[1], price[2]), 2)
priceCompB <- round(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]), 2)
productCode <- paste('#', round(runif(n, 10000, 99999),0), sep= "")

product_comparison <- data.frame(productCode, priceCompA, priceCompB)

rm(nProducts)
rm(price)
rm(priceFracB)
rm(n)
rm(productCode)
rm(priceCompA)
rm(priceCompB)
```

*** =sample_code
```{r}
# The product_comparison dataframe is available in your workspace. 

# 1) Find the number of products that we sampled and assign your answer to: n_products

n_products <-

# 2) Calculate the sample mean for product prices from Company A and assign your answer to: mean_price_A

mean_price_A <-

# 3) Calculate the sample standard deviation for product prices from Company A and assign your answer to: sd_price_A

sd_price_A <-

# 4) Calculate the sample mean for product prices from Company B and assign your answer to: mean_price_B

mean_price_B <-

# 4) Calculate the sample standard deviation for product prices from Company B and assign your answer to: sd_price_B

sd_price_B <-

# The following code will print your answers to the console

n_products
mean_price_A
sd_price_A

mean_price_B
sd_price_B
sd_price_B
```

*** =solution
```{r}
# The product_comparison dataframe is available in your workspace. 
# Make sure you get this part right before going to the next section since you will have to reuse the code in the following questions.

# 1) Find the number of products and safe your answer to: n_products

n_products <- nrow(product_comparison)

# 2) Calculate the sample mean for product prices from Company A and assign your answer to: mean_price_A

mean_price_A <- mean(product_comparison$priceCompA)

# 3) Calculate the sample standard deviation for product prices from Company A and assign your answer to: sd_price_A

sd_price_A <- sd(product_comparison$priceCompA)

# 4) Calculate the sample mean for product prices from Company B and assign your answer to: mean_price_B

mean_price_B <- mean(product_comparison$priceCompB)

# 4) Calculate the sample standard deviation for product prices from Company B and assign your answer to: sd_price_B

sd_price_B <- sd(product_comparison$priceCompB)

# The following code will print your answers to the console

n_products
mean_price_A
sd_price_A

mean_price_B
sd_price_B
sd_price_B

```

*** =sct
```{r}
test_object("n_products", undefined_msg = "Make sure to define a variable `n_products`.",
            incorrect_msg = "Make sure that you calculated the number of samples correctly and assigned your answer to `n_products`.") 

test_function('nrow', args = "x", not_called_msg = "There are different ways to calculate the number of samples. To ensure that your code produce consistent results, regardless of the dataset used, use the `nrow` command to determine the number of samples in the data frame. Type `?nrow` in the console if you are unsure how it works.")
            
test_object("mean_price_A", undefined_msg = "Make sure to define a variable `mean_price_A`.",
            incorrect_msg = "Make sure that you calculated the sample mean for product prices from Company A correctly and assigned your answer to `mean_price_A`.")
            
test_object("sd_price_A", undefined_msg = "Make sure to define a variable `sd_price_A`.",
            incorrect_msg = "Make sure that you calculated the standard deviation for product prices from Company A correctly and assigned your answer to `sd_price_A`.")
            
test_object("mean_price_B", undefined_msg = "Make sure to define a variable `mean_price_B`.",
            incorrect_msg = "Make sure that you calculated the sample mean for product prices from Company B correctly and assigned your answer to `mean_price_B`.")
            
test_object("sd_price_B", undefined_msg = "Make sure to define a variable `sd_price_B`.",
            incorrect_msg = "Make sure that you calculated the standard deviation for product prices from Company B correctly and assigned your answer to `sd_price_B`.")
            
success_msg("Good job! By using the `sd()`, `mean()` and `nrow()` commands we can manually calculate the sample statistics necessary to perform inference for numerical data. Although in this case study we can't use them separately on the product prices for each company since we are performing numerical inference for paired data.")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:23cc0a8c6e
## Requirements for working with paired data

For the case study we wish to analyse paired numerical data.
If we are to do the analyse manually, we will need to perform some calculations on the original data set.
Which of the following calculations do we need to perform?

*** =instructions

- Subtract the price from Company A from the price from Company B for each product.
- Calculate the mean price for Company A and the mean price for Company B and then calculate the difference between the two.
- Determine which products from Company A are cheaper than those from Company B and calculate the proportion of cheaper products.
- Draw a side-by-side boxplot for the product prices from the companies to see which Company is cheaper.

*** =hint
Have a look at Chapter 4 of the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}
#none
```

*** =sct
```{r}
msg_bad <- "That is not correct. Have a look at Chapter 4 on inference for numerical data in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook."

msg_success <- "Correct! To perform inference for paired data we will analyse the difference in prices for each product. The first step is therefore to calculate this difference in R, which is what we will do in the next question."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```


--- type:NormalExercise lang:r xp:100 skills:1 key:19491ec2a5
## Analysing the difference between product prices

The component price data has been loaded in the workspace as the `product_comparison` dataframe. Note that sample is different from the previous question. 

Before we can apply inference for paired data, we need to calculate the difference in price from Company A and Company B per product. Thereafter we need to check the conditions for numerical inference. These are that:

- samples should be independent; and
- the distribution of the variable should be near normal.

We can assume the first condition holds since we took a random sample of product. For the second condition we need to analyse the distribution of the sample and specifically check for skewness, and then look at the number of samples that we have. If we less than 30 samples, then the sample distribution has to be nearly symmetrical. If we have more than 30 but less than 60 samples then moderate skewness can be tolerated, and if we have more then 60 samples then heady skewness can be tolerated.

After checking the conditions we need calculate the necessary sample statistics on the difference in product prices. We will reuse our code for doing so in the following exercises.

*** =instructions

1. Calculate the difference between Company A's price and that of Company B for each product (price of A - price of B), and assign the answer to the vector `price_diff`.
2. Draw a histogram of the price difference to analyse the sample distribution.
3. Determine the number of samples and assign the answer to the variable `n`. 
4. Look at the number of samples in conjunction with the histogram and decide if the distribution of the variable is near normal. If you decide it is near-normal set `normalConditionMet <- TRUE`, if not set `normalConditionMet <- FALSE`.
5. Calculate the sample mean and standard deviation of the price differences and assign the answers to `mean_diff` and `s_diff`.

*** =hint

The only new component is to calculate the difference between the price of Company A and Company B per product, and store the result as a newly created vector.
Recall the function `data.frame$variableName` returns a vector of the values of the `variableName` variable. Have a look at documentation on [vector arithmetics](http://www.r-tutor.com/r-introduction/vector/vector-arithmetics).


*** =pre_exercise_code
```{r}
nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- round(runif(n, price[1], price[2]), 2)
priceCompB <- round(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]), 2)
productCode <- paste('#', round(runif(n, 10000, 99999),0), sep= "")

product_comparison <- data.frame(productCode, priceCompA, priceCompB)

rm(nProducts)
rm(price)
rm(priceFracB)
rm(n)
rm(productCode)
rm(priceCompA)
rm(priceCompB)
```

*** =sample_code
```{r}
# The product_comparison dataframe is available in your workspace. 

# 1) Calculate the difference between Company A's price and that of Company B for each product, and assign the answer to the vector: price_diff

price_diff <- 

# 2) Draw a histogram of the price difference to analyse the sample distribution (use control-enter to execute your code and view the results).



# 3) Determine the number samples and, assign the answer to n, and print the answer to the console (use control-enter to execute your code and view the results).

n <- 
n

# 4a) Look at the sample distribution and number of samples to decide if the normal condition has been met. If you decide it is near-normal set: normalConditionMet <- TRUE. If not set: `normalConditionMet <- FALSE`.



# 4b) Calculate the sample mean for the difference in prices and assign your answer to: mean_diff



# 5) Calculate the standard deviation for the difference in prices and assign your answer to: s_diff



# The following code will print your answers to the console

n
normalConditionMet
mean_diff
s_diff


```

*** =solution
```{r}
# The product_comparison dataframe is available in your workspace. 

# 1) Calculate the difference between Company A's price and that of Company B for each product, and assign the answer to the vector: price_diff

price_diff <- product_comparison$priceCompA - product_comparison$priceCompB

# 2) Draw a histogram of the price difference to analyse the sample distribution (use control-enter to execute your code and view the results).

hist(price_diff)

# 3) Determine the number samples, assign the answer to n, and print the answer to the console (use control-enter to execute your code and view the results).

n <- nrow(product_comparison)
n

# 4) Use the sample distribution and number of samples to determine if the normal condition has been met. Assign your TRUE or FALSE answer to:  normalConditionMet

normalConditionMet <- TRUE

# 5) Calculate the sample mean for the difference in prices and assign your answer to: mean_diff

mean_diff <- mean(price_diff)

# 5) Calculate the standard deviation for the difference in prices and assign your answer to: s_diff

s_diff <- sd(price_diff)

# The following code will print your answers to the console

n
normalConditionMet
mean_diff
s_diff
```

*** =sct
```{r}
test_object("price_diff", undefined_msg = "Make sure to calculate the price difference per product and assigned your answer to `price_diff`. Also make sure that you calculated it as the price from Company A minus the price from Company B, and that you calculated it per product.")

test_function('hist', args = "x", not_called_msg = "You did not draw a histogram of the price differences.",)

test_function('nrow', args = "x", not_called_msg = "There are different ways to calculate the number of samples. To ensure that your code produce consistent results, regardless of the dataset used, use the `nrow` command to determine the number of samples in the data frame. Type `?nrow` in the console if you are unsure how it works.",)

test_object("n", undefined_msg = "Make sure to define a variable `n`.",
            incorrect_msg = "Make sure that you calculated the number of samples correctly and assigned your answer to `n`.") 

test_output_contains("n", incorrect_msg = "You did not view the actual value `n`. You need to check the sample size to determine if the normal condition is met.")

test_object("normalConditionMet", undefined_msg = "Make sure to define a variable `normalConditionMet`.",
            incorrect_msg = "You should base your answer on histogram and number samples and assign it as either `TRUE` or `FALSE` to `normalConditionMet`.")
            
test_object("mean_diff", undefined_msg = "Make sure to define a variable `mean_diff`.",
            incorrect_msg = "Make sure that you calculated the mean price differences correctly and assigned your answer to `mean_diff`.")
            
test_object("s_diff", undefined_msg = "Make sure to define a variable `s_diff`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of the price differences correctly and assigned your answer to `s_diff`.")
            
success_msg("Well done! By calculating the price difference per product we can now treat the differences as a single numerical variable, and apply the appropriate statistical techniques. by using the `mean()`, `sd()` and `nrow()` functions, we were able to extract the sample statistics from the data necessary to manually apply the methods for numerical inference. Before we perform more calculations, let's take a step back and see what is were are trying to achieve by performing inference on paired data.")
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:638c133d39
## How do we determine which company is better

Recall from the problem description that we have to decide on which company to use to supply parts, and that we wish to use the cheapest company. To compare the companies we took a random sample of products and asked each company to provide a unit price quote for the products. Previously we calculated the difference between unit prices (Company A's price minus Company B's price), and we said the we will use the differences to decide on the best company. How can we decide on the best company?

*** =instructions

- Take the mean of the product price differences. If the mean is less than 0 we know that Company A is cheaper. If the mean is more than 0 we know that Company B is cheaper. If the mean is zero we know the Companies are the same.
- Draw a side-by-side boxplot of the prices of Company A and B and analyse the median price and variance. The company with the lowest medium price and lowest variance is the better company.
- Calculate summary statistics, using the `summary()` command in R. The company with the lowers mean or median, and lowest 25th and 75th percentile values is the better company.
- Conduct a hypothesis test using the price differences to determine if there is a significant price difference between the prices of Company A and B.
- Conduct a hypothesis test using the prices from Company A and B to determine if there is a significant difference in the mean price of Company A and the mean price of Company B.

*** =hint
Have a look at the case study background, shown in the first question, and the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg_bad <- "That is not correct."
msg_success <- "Correct! We should do a hypothesis test to see if the sample data provide enough evidence of there being a difference between Company A and B products. Since we are dealing with a sample, we have to apply inference. We are also dealing with paired data, so we will look at the price differences, and not the prices directly. Before conducting the test, let's first setup the hypotheses."
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success, msg_bad))
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:71bd4684d4
## Setting up the hypotheses

Which is the correct null and alternative hypothesis for the case study problem:

*** =instructions

- H0: there is no difference between the prices of Company A and Company B, HA: there is a difference between the prices of Company A and Company B.
- H0: there is no difference between the mean prices of Company A and Company B, HA: there is a difference between the average prices of Company A and Company B.
- H0: the price differences between Company A and Company B is zero, HA: the price differences between Company A and Company B is not zero.
- H0: the mean of price differences between Company A and Company B is zero, HA: the mean of price differences between Company A and Company B is not zero.

*** =hint
Have a look at the case study background, shown in the first question, and the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg_bad <- "That is not correct."
msg_success <- "That is correct! Under the null-hypothesis we will assume that the mean of price differences between the Companies is zero, meaning there prices are the same. We will then look at the actual mean of the price differences and see how likely this value is under the assumption that null hypothesis is true. We will then use the value to decide if it provides sufficient evidence against the null hypothesis, and then decide if we want to reject or not-reject our null hypothesis. Now that we have the hypothesis test setup, let's conduct the test."
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:b5c98f6548
## Overview of R's `pnorm` and `pt` functions

To conduct the hypothesis test semi-manually we need to do the following:

1. Calculate the Standard Error (SE) of our sample statistics using an appropriate SE formula.
2. Calculate the T-score for our sample mean when dealing with numerical data, and the Z-score for our proportion when dealing with categorical data.
3. Find the p-value of the T-score or Z-score.
4. Compare the p-value to our alpha value and conclude the hypothesis test.

To find the p-value we can use probability tables. The only problem with this approach is that if we were to get a new sample, we will have to manually look-up the p-value. An advantage of writing a program for the hypothesis test is that it should automate our calculations. We therefore have to find a better way to determine the p-value, ideally using R's built in functions.

The opposite also occurs when we want to calculate a confidence interval. The confidence interval calculations includes a Margin of Error, which, again depends on a critical z or t value. These values are determined based on the required confidence level, and for the critical t value, by the degrees of freedom. Here we also want to automatically get the critical z or t value using R's built in function, instead of relying on distribution tables or external sources.

To calculate these values we can use R's built in probability distribution functions. For the normal distribution we can use the `dnorm` and `qnorm` functions, and for the t-distribution we can use the `pt` and `qt` functions. To find out more about the functions, type `?dt` and `?pt` in the console. 

First, we will consider `pnorm` to calculate the p-value. At a minimums, `pnorm(Z_score)` takes the Z-score (`Z_score`) of our observed mean, and returns the probability of obtaining a Z-score of less than that value. It will thus give the area to the left of the Z-score under the curve. If `Z_score <- -1.1` then `pnorm(Z_score) = 0.136`. This means that the probability of obtaining a Z-score of less than -1.1 is about to 0.136. 

Will `pnorm(Z_score)` always give us our p-value for our hypothesis test? No it won't. It depends on our hypothesis test and which area under the normal curve we are interested in. For a double-sided hypothesis test it becomes a bit more tricky. Here we are interested in observing a similar difference from the assumed mean under H0, and we don't care if the difference is positive or negative. We therefore have to look at both the left and right tail areas of our Z-score. If `Z_score <- -1.1`, then `pnorm(Z_score) = 0.136`. But we also want to find the probability of having a Z-score of _greater_ than 1.1 and add this to the previous value. Since the normal distribution is symmetrical we know that P(Z-score < -1.1) = P(Z-score > 1.1), so the p-value is going to be equal to `2*pnorm(Z_score) = 0.272`. But what happens if `Z_score <- 2.1`? Now `2*pnorm(Z_score) = 1.964`, which means our p-value is greater than 1, _which is impossible_! So why is this calculation wrong? 

Remember that `pnorm(Z_score)` returns the probability of obtaining a Z-score of less than `Z_score` value, hence the area to the left of `Z_score` under the normal curve. For a double-sided hypothesis we are _always_ interested in the tail areas, that is the area to the right of the positive Z-score value, and to the left of the negative Z-score value. We have to consider this when calculating the p-value. To get the right-tail area under the normal curve for a positive Z-score we can, use `1-pnorm(Z_score)`, which is simply the full area under the normal curve, minus the area to the left. Now that we have to correct area under the curve we just need to multiply it by 2. For a double-sided hypothesis test, how we calculate the p-value depends on whether our Z-score is positive or negative. For a single-sided hypothesis test it will depend on whether our HA states that the population mean is less than a certain value (meaning we need to calculate the area to the left) and we can use `pnorm(Z_score)` as-is, or whether the population mean is more than a certain value (meaning we need to calculate the area to the right) and have to use `1-pnorm(Z_score)`. The same principles apply to to `pq` function for the t-distribution. The only difference is that we always have to specify the degrees of freedom when calling the function. So if our degrees of freedom is say 10, and the critical T-score is 2.5, then the function should be called as `pt(-2.5, 10) = 0.016`. Whether this is our actual p-value depends on the hypotheses. 

If you still don't quite understand how it works, have a look at:

- [this website](http://www.dummies.com/education/math/statistics/how-to-determine-a-p-value-when-testing-a-null-hypothesis/) that explains the concept of calculating the `p-value` 
- [this website](http://www.cyclismo.org/tutorial/R/pValues.html) which shows how the p-value can be calculated in R.

As an exercise, calculate the p-value for the following scenarios:

*** =instructions

1. Double-sided hypothesis where the T-score is 2.3 and the degrees of freedom is 15. Assign your answer to `p_value_1`.
2. Single-sided hypothesis where the alternative hypothesis is x < 0.23, the T-score is -1.1 and the degrees of freedom is 23. Assign your answer to `p_value_2`.
3. Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is 2.4 and the degrees of freedom is 89. Assign your answer to `p_value_3`.
4. Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is -1.5 and the degrees of freedom is 10. Assign your answer to `p_value_4`.
5. Double-sided hypothesis where the T-score is -3.3 and the degrees of freedom is 23. Assign your answer to `p_value_5`.

*** =hint

Have a look at the recommended websites.

[This website](http://www.dummies.com/education/math/statistics/how-to-determine-a-p-value-when-testing-a-null-hypothesis/) explains the concept in more detail. [This website](http://www.cyclismo.org/tutorial/R/pValues.html) shows how the p-value can be calculated in R.


*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# Use the pt function to calculate p-values for the following:

# 1) Double-sided hypothesis where the T-score is 2.3 and the degrees of freedom is 15. Assign your answer to p_value_1.

p_value_1 <-

# 2) Single-sided hypothesis where the alternative hypothesis is x < 0.23, the T-score is -1.1 and the degrees of freedom is 23. Assign your answer to p_value_2.

p_value_2 <-

# 3) Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is 2.4 and the degrees of freedom is 89. Assign your answer to p_value_3.

p_value_3 <-

# 4) Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is -1.5 and the degrees of freedom is 10. Assign your answer to `p_value_4`.

p_value_4 <-

# 5) Double-sided hypothesis where the T-score is -3.3 and the degrees of freedom is 23. Assign your answer to `p_value_5`.

p_value_5 <-

```

*** =solution
```{r}
# Use the pt function to calculate p-values for the following:

# 1) Double-sided hypothesis where the T-score is -2.3 and the degrees of freedom is 15. Assign your answer to p_value_1.

p_value_1 <- 2*pt(-2.3, 15)

# 2) Single-sided hypothesis where the alternative hypothesis is x < 0.23, the T-score is -1.1 and the degrees of freedom is 23. Assign your answer to p_value_2.

p_value_2 <-  pt(-1.1, 23)

# 3) Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is 2.4 and the degrees of freedom is 89. Assign your answer to p_value_3.

p_value_3 <- 1-pt(2.4, 89)

# 4) Single-sided hypothesis where the alternative hypothesis is x > 0.86, the T-score is -1.5 and the degrees of freedom is 10. Assign your answer to `p_value_4`.

p_value_4 <-  1-pt(-1.5, 10)

# 5) Double-sided hypothesis where the T-score is -3.3 and the degrees of freedom is 23. Assign your answer to `p_value_5`.

p_value_5 <- 2*pt(-3.3, 23)

```

*** =sct
```{r}
test_object("p_value_1", undefined_msg = "Make sure to define a variable `p_value_1`.",
            incorrect_msg = "Make sure that you calculated the `p_value_1` correctly, and take note of the sign of the T-score and that it is a double-sided hypothesis test.") 
            
test_object("p_value_2", undefined_msg = "Make sure to define a variable `p_value_2`.",
            incorrect_msg = "Make sure that you calculated the `p_value_2` correctly, and take note of the sign of the T-score and of the alternative hypothesis statement.") 

test_object("p_value_3", undefined_msg = "Make sure to define a variable `p_value_3`.",
            incorrect_msg = "Make sure that you calculated the `p_value_3` correctly, and take note of the sign of the T-score and of the alternative hypothesis statement.") 
            
test_object("p_value_4", undefined_msg = "Make sure to define a variable `p_value_4`.",
            incorrect_msg = "Make sure that you calculated the `p_value_4` correctly, and take note of of the sign of the T-score and the alternative hypothesis statement.") 
            
test_object("p_value_5", undefined_msg = "Make sure to define a variable `p_value_5`.",
            incorrect_msg = "Make sure that you calculated the `p_value_5` correctly, and take note of the sign of the T-score and that it is a double-sided hypothesis test.") 
```
--- type:NormalExercise lang:r xp:100 skills:1 key:803d9a69c1
## Conducting the hypothesis test

Our hypotheses are as follow:

- H0: the mean price difference between Company A and Company B is zero,
- HA: the mean price difference between Company A and Company B is not zero,

and in symbolic form:

- H0: `mu_diff = 0`,
- HA: `mu_diff != 0`.

Where `mu_diff` is the mean difference for the of the entire population. In this exercise we will use a _new_ sample of products and their prices from Company A and B and conduct the hypothesis. The new sample is available in the `product_comparison` dataframe.

Since we are dealing with a new sample we will again have to calculate difference in prices per product, get the sample size, and calculate the mean and standard deviation of sample.

Once we have the required sample statistics we need to determine how likely our observation or more in favour of the hypothesis is, under the assumption that the null hypothesis is true. This "likely-hood" is our p-value which we can compare against a significance level (alpha value). Since none were supplied we will use an alpha value of 0.05.

Recall from [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs), Chapter 4 that to conduct a hypothesis test for a single numerical variable we need to calculate the Standard Error for the sample, calculate the number of SEs that our observation is away from the assumed mean under the null hypothesis (this will be the T-score for our observation), and then calculate the probability of observing the value or in favour of the alternative hypothesis using the t-distribution. The t-distribution also requires us to calculate the degrees of freedom (df). All the formulas required for the calculations can be found in the [textbook](https://www.openintro.org/stat/textbook.php?stat_book=isrs).

To calculate the p-value we will be using the `pt()` function, as described in the previous exercise. 

To successfully complete this lab, do the following, and note that you have to define the appropriate answer variables yourself:

*** =instructions

1. Determine the number of samples and calculate the difference in prices per product, as well as the mean and standard deviation of the sample. Assign your answers to the `n`, `price_diff`, `mean_diff` and `sd_diff` variables, same as before.
2. Calculate the Standard Error of our sample mean and assign your answer to the `SE` variable.
3. Calculate the T-score of the sample mean and assign your answer to the `T_score` variable.
4. Calculate the degrees of freedom associated with our hypothesis test and assign your answer to the `df` variable.
5. Calculate the p-value of our T-score using the `pt()` function and assign your answer to the `p_value` variable.
6. Lastly, use an alpha value of 0.05 and decide if there is sufficient evidence to reject the null hypothesis. Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.

*** =hint

Go through the previous exercises and carefully look at your calculations. The R formulas for the Standard Error, T-score and Degrees of Freedom are:

```
SE = sd_diff/sqrt(n)
T_score = mean_diff/SE
df = n - 1
```

Calculating the p-value is tricky. Start by carefully going through the documentation by typing `?pt` in the console. 

The actual p-value depends on the actual value of `T_score`. We know that `pt(T_score, df)` will give the area to the left of `T_score` for a t-distribution with degrees of freedom of `df`. If `T_score < 0` then the correct area is given by the function and we simply need to multiply it by two since we want to also include the same area under the right side of the tail. Remember that we are dealing with a double-sided hypothesis test, so we don't care whether the mean difference is positive or negative. We are only really interested in the absolute difference. 

If `T_score > 0` we know that the incorrect area is given, since we need the area to the right of `T_score` in the tail (not the left). To fix this we can either call `pt(T_score, df,  lower.tail = FALSE)` which will then give the upper tail, or we can use `1 - pt(T_score, df)`. After that we still have to multiply the value by two.

*** =pre_exercise_code
```{r}
nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- round(runif(n, price[1], price[2]), 2)
priceCompB <- round(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]), 2)
productCode <- paste('#', round(runif(n, 10000, 99999),0), sep= "")

product_comparison <- data.frame(productCode, priceCompA, priceCompB)

rm(nProducts)
rm(price)
rm(priceFracB)
rm(n)
rm(productCode)
rm(priceCompA)
rm(priceCompB)
```

*** =sample_code
```{r}
# The product_comparison dataframe is available in your workspace. Remember to create and assign your answers to the correct variables as specified in the instructions. Where necessary, view the outcome of your calculation by printing them to the console. You can also view the values of your variables by typing them in the console and pressing enter.

# 1) Determine the number of samples and calculate the difference in prices per product, as well as the mean and standard deviation of the sample. 

# 2) Calculate the Standard Error of the sample mean. 

# 3) Calculate the T-score of the sample mean.

# 4) Calculate the degrees of freedom associated with our hypothesis test.

# 5) Calculate the p-value of our T-score using the `pt()` function.

# 6) Use an alpha value of 0.05 and decide if there is sufficient evidence to reject the null hypothesis.

# You can print whatever you need to view here. For advanced users, you can check if the answers make sense by using the t.test function.

```

*** =solution
```{r}
# Do not just view the answers!!!!!!! 

# You will not have this option available in test and exams. You have to try and figure out the solutions on your own, otherwise you will do poorly in the tests.

# 1) Determine the number of samples and calculate the difference in prices per product, as well as the mean and standard deviation of the sample, and create and assign your answers to the correct variables (this applies to all the following questions). 

price_diff <- product_comparison$priceCompA - product_comparison$priceCompB
n <- nrow(product_comparison)
mean_diff <- mean(price_diff)
sd_diff <- sd(price_diff)

# 2) Calculate the Standard Error of the sample mean. 

SE <- sd_diff/sqrt(n)

# 3) Calculate the T-score of the sample mean.

T_score <- mean_diff/SE

# 4) Calculate the degrees of freedom associated with our hypothesis test.

df = n - 1

# 5) Calculate the p-value of our T-score using the `pt()` function.

if(T_score < 0){p_value = 2*pt(T_score, df)}else{p_value = 2*(1-pt(T_score, df))}
p_value

# 6) Use an alpha value of 0.05 and decide if there is sufficient evidence to reject the null hypothesis.

rejectH0 <- p_value < 0.05
rejectH0

# You can print whatever you need to view here. For advanced users, you can check if the answers make sense by using the t.test function. And nope, the answer the print part is not included in here. You will have to figure it out for yourself.

```

*** =sct
```{r}
test_object("price_diff", undefined_msg = "Make sure to calculate the price difference per product and assigned your answer to `price_diff`. Also make sure that you calculated it as the price from Company A minus the price from Company B, and that you calculated it per product.")

test_function('nrow', args = "x", not_called_msg = "There are different ways to calculate the number of samples. To ensure that your code produce consistent results, regardless of the dataset used, use the `nrow` command to determine the number of samples in the data frame. Type `?nrow` in the console if you are unsure how it works.",)

test_object("n", undefined_msg = "Make sure to define a variable `n`.",
            incorrect_msg = "Make sure that you calculated the number of samples correctly and assigned your answer to `n`.") 
            
test_object("mean_diff", undefined_msg = "Make sure to define a variable `mean_diff`.",
            incorrect_msg = "Make sure that you calculated the mean price difference correctly and assigned your answer to `mean_diff`.")
            
test_object("sd_diff", undefined_msg = "Make sure to define a variable `sd_diff`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of the price differences correctly and assigned your answer to `sd_diff`.")
            
test_object("SE", undefined_msg = "Make sure to define a variable `SE`.",
            incorrect_msg = "Make sure that you calculated the standard error of the sample mean price differences correctly and assigned your answer to `SE`. Refer to the prescribed textbook for the correct standard error formula to use.")
            
test_object("T_score", undefined_msg = "Make sure to define a variable `T_score`.",
            incorrect_msg = "Make sure that you calculated the T-score of the sample mean price differences correctly and assigned your answer to `T_score`. Refer to the prescribed textbook for the correct T-score formula to use.")

test_object("df", undefined_msg = "Make sure to define a variable `df`.",
            incorrect_msg = "Make sure that you calculated the degrees of freedom correctly  and assigned your answer to `df`. Refer to the prescribed textbook for the correct degrees of freedom formula to use.")
            
test_object("p_value", undefined_msg = "Make sure to define a variable `p_value`.",
            incorrect_msg = "Make sure that you calculated the p-value correctly and assigned your answer to `p_value`. Refer to the prescribed textbook on how to determine the p-value of doubled sided hypothesis test using the t-distribution. You may also use probability tables to check your calculated value that you got using the `pt()` function.")

test_output_contains("p_value", incorrect_msg = "We have to view the `p_value` to see what it is, so update your code to print its value to the console. Knowing the `p_value` is important for decision support. For instance, what would happen if `p_value = 0.50001`? Would we still want to not reject H0?.")

test_object("rejectH0", undefined_msg = "Make sure to define a variable `rejectH0`.",
            incorrect_msg = "Make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectH0`. Refer to the prescribed textbook on how to determine if we can reject (`TRUE`) or not reject (`FALSE`) the null hypothesis based on alpha.")
            
test_output_contains("rejectH0", incorrect_msg = "It's probably a good idea to actually view the `rejectH0` to see what the outcome of the test was, so update your code to print its value to the console. We need to know the outcome of the test to determine if there is a difference between the suppliers.")

success_msg("Congrats! You have successfully completed the hypothesis test using actual data. The last part is to make a recommendation in terms of which supplier to use. If we could not reject H0, then we can choose either supplier or use other criteria to make a decision. If we could reject H0, then we can take a new sample of products, and do a one sided hypothesis test to see if the suspected cheaper company is indeed significantly cheaper. Alternatively we can calculate a confidence interval on the mean difference and use that to decide on whether one company is cheaper. In the last question, coming up next, we will (again) look at a new sample and compute a confidence interval for the true mean difference. Thereafter we will make a final recommendation on whether we should choose one company over the other, and if so which company to choose. We will do this with minimal instructions, hints and error messages. Instead we will use the built in function `t.test` to check if our answers make sense.")
```



--- type:NormalExercise lang:r xp:100 skills:1 key:de00830dd6
## Calculating a confidence interval

For the last question we are going to calculate a confidence interval, and then use that to determine if one company is better than the other, and if so, which one is better. We will reuse much of our previous calculations. To test whether you know how to apply the methods, available hints, instructions and error messages will be kept to a minimum. You can however consult previous exercises as well as [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs), which contains all the formulas that you need to successfully answer this question.

In this exercise we will use a _new_ sample of products and their prices from Company A and B and conduct the hypothesis. The new sample is available in the `product_comparison` dataframe.

For this question, you need to semi-manually calculate a 98% confidence interval to compare Company A and B, and then make a final recommendation on which company to choose. Note that the you have to use R's built in functions to calculate the critical t-value for the margin of error. You cannot use the probability tables. [This website](http://www.cyclismo.org/tutorial/R/confidence.html?highlight=confidence%20interval) illustrates how to do so in R. Take special note of the required confidence levels in the examples, and what they then put into the R functions. There is some manipulation that takes place first.

Thereafter you have to use the built in function, `t.test` to check if your answer makes sense. See [this website](http://www.cyclismo.org/tutorial/R/pValues.html) for more information on using the function, and remember that we are dealing with paired data. 

*** =instructions

1. Manually calculate a 98% confidence interval and assign the lower confidence interval value to `CI_low` 
and the higher confidence interval value to `CI_high`. Refer to [this website](http://www.cyclismo.org/tutorial/R/confidence.html?highlight=confidence%20interval) which illustrates how to calculate Confidence Intervals in R.
2. Use the confidence interval to make a recommendation on which company to choose by setting `chooseCompanyA` AND `chooseCompanyB` to either `TRUE` or `FALSE`, depending on the company to choose. If the sample indicates that there is not a significant difference between the two, set both variables equal to `TRUE`, since we can choose either one.
3. Conduct a hypothesis test by using the `t.test` function and compare the results to our calculated confidence interval. Remember to set an appropriate confidence interval level. See [this website](http://www.cyclismo.org/tutorial/R/pValues.html) for more information on using the function, and remember that we are dealing with paired data. 

*** =hint

None. You have to figure this one out for yourself.

*** =pre_exercise_code
```{r}
nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1.1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- round(runif(n, price[1], price[2]), 2)
priceCompB <- round(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]), 2)
productCode <- paste('#', round(runif(n, 10000, 99999),0), sep= "")

product_comparison <- data.frame(productCode, priceCompA, priceCompB)

rm(nProducts)
rm(price)
rm(priceFracB)
rm(n)
rm(productCode)
rm(priceCompA)
rm(priceCompB)
```

*** =sample_code
```{r}
# You have a blank canvas, just make sure to assign your final answers to the correct variables.

#Just DON'T overwrite existing functions when creating new variables. For example, mean = mean(x) will basically make the program crash, since you have now over-written R's mean function with the actual mean value of x. Don't do it.
```

*** =solution
```{r}
# Fine, if you want to know the final answer, here it is. Just remember that part of learning and understanding something is to figure out how it works for yourself. And of course, you won't have a show answer option in tests, exams and in industry. Just to be nasty we have calculated most of the stuff in a different way...

CI_level <- 0.98

pd <- product_comparison$priceCompA - product_comparison$priceCompB
ME <- qt((1-CI_level)/2, nrow(product_comparison) - 1, lower.tail = FALSE)*sd(pd)/sqrt(nrow(product_comparison))
CI_low <- mean(pd) - ME
CI_high <- mean(pd) + ME

CI_low
CI_high

CI <- c(mean(pd) - ME, mean(pd) + ME)

if (CI[2] < 0)
{
    chooseCompanyA <- TRUE
    chooseCompanyB <- FALSE
}else if(CI[1] > 0){
    chooseCompanyA <- FALSE
    chooseCompanyB <- TRUE
}else{
    chooseCompanyA <- TRUE
    chooseCompanyB <- TRUE    
}

t.test(x = product_comparison$priceCompA, y = product_comparison$priceCompB, alternative = "two.sided", paired = TRUE, conf.level = 0.98)
```

*** =sct
```{r}
test_function('nrow', args = "x", not_called_msg = "You have to semi-manually do the calculations. You are missing some function calls to do so.")
test_function('mean', args = "x", not_called_msg = "You have to semi-manually do the calculations. You are missing some function calls to do so.")
test_function('sd', args = "x", not_called_msg = "You have to semi-manually do the calculations. You are missing some function calls to do so.")

test_object("CI_low", undefined_msg = "Your confidence interval is incorrect.", incorrect_msg = "Your confidence interval is incorrect.")
test_object("CI_high", undefined_msg = "Your confidence interval is incorrect.", incorrect_msg = "Your confidence interval is incorrect.")

#test_output_contains("CI_low", incorrect_msg = "You need to print some of your answers to the console to make a recommendation.")
#test_output_contains("CI_high", incorrect_msg = "You need to print some of your answers to the console to make a recommendation.")

test_object("chooseCompanyA", undefined_msg = "Almost there, but your final recommendation on which company to choose is incorrect.", incorrect_msg = "Almost there, but your final recommendation on which company to choose is incorrect.")
test_object("chooseCompanyB", undefined_msg = "Almost there, but your final recommendation on which company to choose is incorrect.", incorrect_msg = "Almost there, but your final recommendation on which company to choose is incorrect.")

test_function('t.test', args = c("x", "y", "alternative", "paired", "conf.level"), not_called_msg = "How about using the `t.test` function to check your answer?", args_not_specified_msg = "You need to specify certain input arguments for the function. Have a look at the documentation and figure out which input arguments to use. You also need to set the input arguments to the correct value inside the function call. For example, `t.test(x = variableA, y = ...)`", incorrect_msg = "Some of the input arguments that you specified for the function is incorrect. Have a look at the documentation and figure out which input arguments to use. For example, `t.test(x = variableA, y = ...)`")

success_msg("Congratulations! You have successfully completed the chapter by applying inference for paired numerical data. In the last exercise you were able to correctly apply the methods with the minimum amount of guidance, as will be the case in industry. You were also able to use the `t.test` function to perform the analysis. Feel free to retry the chapter for practice.")
```
