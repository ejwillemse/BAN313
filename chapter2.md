---
title       : Assignment test
description : Test the assignment capabilities

--- type:NormalExercise lang:r xp:100 skills:1 key:03c9a0637d
## More movies

Have a look at the component price data, which has been loaded as the `product_comparision` dataframe. 

*** =instructions
- How many products did we compare?

*** =hint
- Use `nrow()` for the first instruction.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

nProducts <- c(100, 200)
price <- c(100, 10000)
priceFracB <- c(1, 0.05)
n <- runif(1, nProducts[1], nProducts[2])
priceCompA <- runif(n, price[1], price[2])
priceCompB <- abs(priceCompA*rnorm(n, priceFracB[1], priceFracB[2]))
product <- c(1:n)

productComparison <- data.frame(product, priceCompA, priceCompB)

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
# The product_comparision dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products

# Print the results to the console:

```

*** =solution
```{r}
# The product_comparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products
n_products <- nrow(productComparison)

# Print the results to the console:
n_products
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_object("n_products")
test_function("nrow", args = "x",
              not_called_msg = "It seems that you didn't use a built in function to calculate the number of products. Hint: see `?nrow`")
              
test_output_contains("n_products", "You forgot to print your answer to the console...")
test_error()
success_msg("Good work!")
```