---
title       : Assignment test
description : Test the assignment capabilities




--- type:NormalExercise lang:r xp:100 skills:1 key:04e920022f
## More movies

Have a look at the component price data, which has been loaded as the `product_comparison` dataframe. 

*** =instructions
- How many products did we compare?

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

# Find the number of products and safe your answer to: n_products

n_products <-

# Print the results to the console:

n_products

```

*** =solution
```{r}
# The product_comparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products

n_products <- nrow(product_comparison)

# Print the results to the console:

n_products

```

*** =sct
```{r}
test_object("n_products", undefined_msg = "Make sure to define a variable `n_products`.",
            incorrect_msg = "Make sure that you assign the correct value to `n_products`.") 
success_msg("Good job! Have you noticed that R does not print the value of a variable to the console when you did the assignment? `x <- 42` did not generate any output, because R assumes that you will be needing this variable in the future. Otherwise you wouldn't have stored the value in a variable in the first place, right? Proceed to the next exercise!")
```
