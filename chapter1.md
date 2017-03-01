--- 
title_meta  : Case study 4
title       : Choosing the best vendor
description : "In this case study we will compare two product suppliers, with the objective to choose the best one to partner with. 
For the case study, best means cheapest. To compare the vendors we will rely on a sample of products for which both vendors provided unit-price quotes. We will then use inference for numerical data to decide if one vendor is better than the other."

--- type:NormalExercise lang:r xp:0 skills:1 key:1c5a628f8f
## Background

A critical component of supply chain management is building and maintaining an effective relationship with the companies within your supply chain. Choosing which companies to do business with is also an important component.

For this Case Study we are employed by a manufacturing company we have to decide which supplier to partner with to supplying materials for our wide range of solar powered products. To complicate matters, the product range is continuously changing, and each new product requires different parts. We therefore need to choose a supplier that has a very broad range of products available, and with the ability to provide products upon request that are currently not in their catalogue. We do not know how much the company may charge for future products.

Currently there are only two suppliers, referred to as Company A and B, to choose from. To compare the companies we have taken a random sample of our current input products, and asked both companies to provide a unit-price for each of the products. We will use this data to infer if one company is better than the other.

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

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

--- type:MultipleChoiceExercise lang:r xp:25 skills:1 key:a9ac72e6f7
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

--- type:MultipleChoiceExercise lang:r xp:25 skills:1 key:cd4c8c8aa5
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
msg_success <- "Correct! By setting `paired = TRUE` inside the function call to `t.test(x, y)` we can perform a t-test for paired numerical data. In the next exercise we will first manually perform the test and thereafter compare our results against that of the function."
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

# 1) Find the number of products that we sampled and safe your answer to: n_products

n_products <-

# 2) Calculate the sample mean for product prices from Company A and safe your answer to: mean_price_A

mean_price_A <-

# 3) Calculate the sample standard deviation for product prices from Company A and safe your answer to: mean_price_A

sd_price_A <-

# 4) Calculate the sample mean for product prices from Company B and safe your answer to: mean_price_B

mean_price_B <-

# 4) Calculate the sample standard deviation for product prices from Company B and safe your answer to: mean_price_B

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

# 2) Calculate the sample mean for product prices from Company A and safe your answer to: mean_price_A

mean_price_A <- mean(product_comparison$priceCompA)

# 3) Calculate the sample standard deviation for product prices from Company A and safe your answer to: mean_price_A

sd_price_A <- sd(product_comparison$priceCompA)

# 4) Calculate the sample mean for product prices from Company B and safe your answer to: mean_price_B

mean_price_B <- mean(product_comparison$priceCompB)

# 4) Calculate the sample standard deviation for product prices from Company B and safe your answer to: mean_price_B

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
test_function('nrow', args = "x", not_called_msg = "There are different ways to calculate the number of samples. To ensure that your code produce consistent results, regardless of the dataset used, use the `nrow` command to determine the number of samples in the data frame. Type `?nrow` in the console if you are unsure how it works.",)

test_object("n_products", undefined_msg = "Make sure to define a variable `n_products`.",
            incorrect_msg = "Make sure that you calculated the number of samples correctly and assigned your answer to `n_products`.") 
            
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

--- type:MultipleChoiceExercise lang:r xp:25 skills:1 key:23cc0a8c6e
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

We can assume the first condition holds since we took a random sample of product. For the second condition we need to analyse the distribution of the sample and specifically check for skewness, and then look at the number of samples that we have. If we less than 30 samples, then the sample distribution has to be nearly symmetrical. If we have more than 30 but less than 60 samples then moderate skewness can be tollerated, and if we have more then 60 samples then heady skewness can be tollorated.

After checking the conditions we need calculate the necessary sample statistics on the difference in product prices. We will reuse our code for doing so in the following exercises.

*** =instructions

1. Calculate the difference between Company A's price and that of Company B for each product (price of A - price of B), and assign the answer to the vector `price_diff`.
2. Draw a histogram of the price difference to analyse the sample distribution.
3. Determine the number of samples and assign the answer to the variable `n`. 
4. Use the number of samples in conjunction with the histogram to check if the distribution of the variable is near normal: assing your answer, which can either be `TRUE` or `FALSE` to the `normalConditionMet` variable.
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

# 4a) Use the sample distribution and number of samples to determine if the noraml condition has been met. Assign your TRUE or FALSE answer to:  normalConditionMet



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

# 4) Use the sample distribution and number of samples to determine if the noraml condition has been met. Assign your TRUE or FALSE answer to:  normalConditionMet

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
            incorrect_msg = "Make sure that you calculated the mean price differencse correctly and assigned your answer to `mean_diff`.")
            
test_object("s_diff", undefined_msg = "Make sure to define a variable `s_diff`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of the price differences correctly and assigned your answer to `s_diff`.")
            
success_msg("Well done! By calculating the price difference per product we can now treat the differences as a single numerical variable, and apply the apporpriate statistical techniques. by using the `mean()`, `sd()` and `nrow()` functions, we were able to extract the sample statistics from the data necessary to manauly apply the methods for numerical inference. Before we perform more calculations, let's take a step back and see what is were are trying to achieve by performing inference on paired data.")
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:638c133d39
## How do we determine which company is better

Recall from the problem description that we have to decide on which company to use to supply parts, and that we wish to use the cheapest company. To copmare the companies we took a random sample of products and asked each company to provide a unit price quaote for the products. Previously we calculated the difference between unit prices (Company A's price minus Company B's price), and we said the we will use the differences to decide on the best company. How can we decide on the best company?

*** =instructions

- Take the mean of the product price differences. If the mean is less than 0 we know that Company A is cheaper. If the mean is more than 0 we know that Company B is cheaper. If the mean is zero we know the Companies are the same.
- Draw a side-by-side boxplot of the prices of Copmany A and B and analyse the mediun price and variance. The company with the lowest medium price and lowest variance is the better company.
- Calculate summary statistics, using the `summary()` command in R. The company with the lowers mean or median, and lowest 25st and 75th percentile values is the better company.
- Conduct a hypothesis test using the price differences to determine if there is a significant price difference between the prices of Company A and B.
- Conduct a hypothesis test using the prices from Copmany A and B to determin if there is a significant difference in mean price of Company A the mean price of Company B.

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
## Which is the correct null and alternative hypothesis for the case study problem:

*** =instructions

- H0: there is no difference between the prices of Company A and Company B, HA: there is a difference between the prices of Company A and Company B.
- H0: there is no difference between the mean prices of Company A and Company B, HA: there is a difference between the average prices of Company A and Company B.
- H0: the price differences between Company A and Company B is zero, HA: the price differences between Company A and Company B is not zero.
- H0: the mean price difference between Company A and Company B is zero, HA: the mean price difference between Company A and Company B is not zero.

*** =hint
Have a look at the case study background, shown in the first question, and the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook.

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
msg_bad <- "That is not correct."
msg_success <- "That is correct! Under the null-hypothesis we will assume that the mean of price differences between the Copmanies is zero, meaning there prices are the same. We will then look at the actual mean of the price differences and see how likely this value is under the assumption that null hypothesis is true. We will then use the value to decide if it provides sufficient effidence against the null hypothesis, and then decide if we want to reject or not-reject our null hypothesis. Now that we have the hypothesis test setup, let's conduct the test."
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success))
```
