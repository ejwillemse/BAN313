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
#none
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
- How many products did we sample to get quotations for?
- What is the sample mean for product prices from Company A?
- What is the sample standard deviation for products price from Company A?
- What is the sample mean for product prices from Company B?
- What is the sample standard deviation for products price from Company B?

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
# Make sure you get this part right before going to the next section since you will have to reuse the code in the following questions.

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

# 5) The following code will print your answers to the console

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

--- type:NormalExercise lang:r xp:25 skills:1 key:5921b08b52
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
msg_bad <- "That is not correct. Have a look at the chapters on inference in the [Introductory Statistics with Randomization and Simulation](https://www.openintro.org/stat/textbook.php?stat_book=isrs) textbook."

msg_success <- "Correct! To perform inference for paired data we will analyse the difference in prices for each product. The first step is therefore to calculate this difference in R, as we will do in the next question."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```
