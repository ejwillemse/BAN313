---
title       : Assignment test
description : Test the assignment capabilities

--- type:NormalExercise xp:100 skills:1 key:5f200ffd43
## Variable assignment 

A basic concept in (statistical) programming is called a **variable**. 

A variable allows you to store a value (e.g. 4) or an object (e.g. a function description) in R. You can then later use this variable's name to easily access the value or the object that is stored within this variable. 

You can assign a value 4 to a variable `my_var` with the command

```
my_var <- 4
```

*** =instructions
Over to you: complete the code in the editor such that it assigns the value 42 to the variable `x` in the editor. Click 'Submit Answer'. Notice that when you ask R to print `x`, the value 42 appears.

*** =hint
Look at how the value 4 was assigned to `my_variable` in the exercise's assignment. Do the exact same thing in the editor, but now assign 42 to the variable `x`.

*** =pre_exercise_code
```{r}
# no pec
```

*** =sample_code
```{r}
# Assign the value 42 to x
x <- 

# Print out the value of the variable x
x
```

*** =solution
```{r}
# Assign the value 42 to x
x <- 42

# Print out the value of the variable x
x
```

*** =sct
```{r}
test_object("x", undefined_msg = "Make sure to define a variable `x`.",
            incorrect_msg = "Make sure that you assign the correct value to `x`.") 
success_msg("Good job! Have you noticed that R does not print the value of a variable to the console when you did the assignment? `x <- 42` did not generate any output, because R assumes that you will be needing this variable in the future. Otherwise you wouldn't have stored the value in a variable in the first place, right? Proceed to the next exercise!")
```