---
title       : Assignment test
description : Test the assignment capabilities




--- type:NormalExercise lang:r xp:100 skills:1 key:04e920022f
## More movies

Have a look at the component price data, which has been loaded as the `productComparison` dataframe. 

*** =instructions
- How many products did we compare?

*** =hint
- Use `nrow()` for the first instruction.

*** =pre_exercise_code
```{r}
# no pec
a = 1000
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
x <- a

# Print the results to the console:
x
```

*** =sct
```{r}
test_object("x", undefined_msg = "Make sure to define a variable `x`.",
            incorrect_msg = "Make sure that you assign the correct value to `x`.") 
success_msg("Good job! Have you noticed that R does not print the value of a variable to the console when you did the assignment? `x <- 42` did not generate any output, because R assumes that you will be needing this variable in the future. Otherwise you wouldn't have stored the value in a variable in the first place, right? Proceed to the next exercise!")
```
