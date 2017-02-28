---
title       : Assignment test
description : Test the assignment capabilities

--- type:NormalExercise lang:r xp:100 skills:1 key:03c9a0637d
## More movies

Have a look at the component price data, which has been loaded as the `productComparison` dataframe. 

*** =instructions
- How many products did we compare?

*** =hint
- Use `nrow()` for the first instruction.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

a <- 1000
```

*** =sample_code
```{r}
# The productComparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products

n_products <-

# Print the results to the console:

n_products

```

*** =solution
```{r}
# The productComparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products
n_products <- a

# Print the results to the console:
n_products
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_object("n_products")
test_output_contains("n_products", "You forgot to print your answer to the console...")
test_error()
success_msg("Good work!")
```