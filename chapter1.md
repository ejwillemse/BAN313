--- 
title_meta  : Case study 4
title       : Choosing the best vendor
description : "In this case study we will compare vendors, with the objective to choose the best one, which in this case means cheapest, to partner with. To do so we will rely on a sample of products and compare the prices of the vendors for the products. Thereafter we will use inference on decide on the best vendor."

--- type:NormalExercise lang:r xp:0 skills:1 key:1c5a628f8f
## Background

A critical component of supply chain management is building and maintaining an effective relationship with the companies within your supply chain. Choosing which companies to do business with is also an important component.

For this Case Study we have been approached by a manufacturing company to assist them on deciding on which supplier to use for supplying materials for our wide range of solar powered products. To complicate matters, the product range is continuously changing, and each new product requires different parts. We therefore need to choose a supplier that has a very broad range of products available, and with the ability to provide products upon request that are currently not in their catalogue. We also do not know how much the company may charge for future products.

Currently there are only two suppliers, referred to as Company A and B, to choose between. To compare the companies we have taken a random sample of our current input products, and asked both companies to provide a unit-price for each of the products. We will use this data to infer if there is one company is better than the other.

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
success_msg("Let's try and use our knowledge on statistical inference to assist the company.")
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
msg_bad <- "That is not correct."
msg_success <- "Correct! We want to compare the average unit-price for the two companies, meaning we are dealing with inference for numerical data. We further asked both companies to provide a quote for the same randomly sampled products, therefore we are dealing with paired-data. Why? Becayse each case involves the same product."
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
msg_bad <- "That is not correct."
msg_success <- "Correct! By setting `paired = TRUE` inside the function call to `t.test(x, y)` we can perform a t-test for paired numerical data. In the next exercise we will first manually peform the test and thereafter compare our results against that of the function."
test_mc(correct = 5, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:25c0c61261
## Analysing the data

The component price data has been loaded in the workspace as the `product_comparison` dataframe. It consists of three variables. The product code (`ProductCode`), the quoted unit-price for the component from Company A (`priceCompA`) and the quoted unit-price for the component from Company B (`priceCompB`).

Using the data, answer the following:

*** =instructions
- How many products did we sample to get quotations from?
- What is the sample mean for product prices from Company A?
- What is the sample standard deviation for products price from Company A?
- What is the sample mean for product prices from Company B?
- What is the sample standard deviation for products price from Company B?

*** =hint
- Use `nrow()` for the first instruction.

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

# 1) Find the number of products and safe your answer to: n_products

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
# The product_comparison dataframe is available in your workspace

# 1) Find the number of products and save your answer to: n_products

n_products <- nrow(product_comparison)

# 2) Find the mean of product price of Company A and save your answer to: mean_price_A

mean_price_A <- mean(product_comparison$priceCompA)

# 3) Find the mean of product price of Company B and save your answer to: mean_price_B

mean_price_B <- mean(product_comparison$priceCompB)

# 4) Find the absolute mean difference in product prices and save your answer to: mean_diff

mean_diff <- abs(mean(product_comparison$priceCompA - product_comparison$priceCompB))

# 5) Print the results to the console:

n_products

mean_price_A

mean_price_B

mean_diff

```

*** =sct
```{r}
test_object("n_products") 
test_object("mean_price_A")
test_object("mean_price_B")
test_object("mean_diff")
success_msg("Good job!")
```