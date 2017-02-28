---
title       : Assignment test
description : Test the assignment capabilities




--- type:NormalExercise lang:r xp:100 skills:1 key:04e920022f
## More movies

Have a look at the component price data, which has been loaded as the `productComparisontest` dataframe. 

*** =instructions
- How many products did we compare?

*** =hint
- Use `nrow()` for the first instruction.

*** =pre_exercise_code
```{r}
# no pec
```

*** =sample_code
```{r}
# The productComparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products

x <-

# Print the results to the console:

x

```

*** =solution
```{r}
# The productComparison dataframe is available in your workspace

# Find the number of products and safe your answer to: n_products
x <- 1000

# Print the results to the console:
x
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_object("x", undefined_msg = "Make sure to define a variable `x`.",
            incorrect_msg = "Make sure that you assign the correct value to `x`.") 
test_output_contains("x", "You forgot to print your answer to the console...")
success_msg("Good work!")
```
