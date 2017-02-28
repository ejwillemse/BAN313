---
title       : Assignment test
description : Test the assignment capabilities

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:a9ac72e6f7
## A really bad movie

*** =pre_exercise_code
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer

drillHoles = rnorm(100, 100, 10)
```

The mean drill size is?

*** =instructions
- `r median(drillHoles)`
- `r mean(drillHoles)`
- `r sd(drillHoles)`
- `r sum(drillHoles)/(n-1)`

*** =hint
Have a look at the plot. Which color does the point with the lowest rating have?

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

msg_bad <- "That is not correct!"
msg_success <- "Exactly! There seems to be a very bad action movie in the dataset."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```