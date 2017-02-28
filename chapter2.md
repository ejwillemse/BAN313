---
title       : Assignment test
description : Test the assignment capabilities




--- type:NormalExercise lang:r xp:100 skills:1 key:04e920022f
## More movies

Have a look at the component price data, which has been loaded as the `product_comparison` dataframe. 

*** =instructions
- How many products did we compare?
- What is the mean product price from Company A?
- What is the mean product price from Company B?
- What is the absolute mean difference in product prices? 

*** =hint
- Use `nrow()` for the first instruction.

*** =pre_exercise_code
```{r}
# no pec
nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- runif(n, price[1], price[2])
priceCompB <- abs(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]))
product <- c(1:n)

product_comparison <- data.frame(product, priceCompA, priceCompB)

rm(nProducts)
rm(price)
rm(priceFracB)
rm(n)
rm(product)
rm(priceCompA)
rm(priceCompB)
```

*** =sample_code
```{r}
# The product_comparison dataframe is available in your workspace

# 1) Find the number of products and safe your answer to: n_products

n_products <-

# 2) Find the mean of product price of Company A and safe your answer to: mean_price_A

mean_price_A <-

# 3) Find the mean of product price of Company B and safe your answer to: mean_price_B

mean_price_B <-

# 4) Find the absolute mean difference in product prices and safe your answer to: mean_diff

mean_diff <-

# 5) Print the results to the console:

n_products

mean_price_A

mean_price_B

mean_diff

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
