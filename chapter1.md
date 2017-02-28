--- 
title_meta  : Case study 4
title       : Choosing the best vendor
description : "In this case study we will compare vendors, with the objective to choose the best one, which in this case means cheapest, to partner with. To do so we will rely on a sample of products and compare the prices of the vendors for the products. Thereafter we will use inference on decide on the best vendor."

--- type:NormalExercise lang:r xp:0 skills:1 key:1c5a628f8f
## Background

A critical component of supply chain management is building and maintaining an effective relationship with the companies within your supply chain. Choosing which companies to do business with is also an important component.

For this Case Study we have been approached by a manufacturing company to assist them on deciding on which supplier to use for supplying materials for our wide range of solar powered products. To complicate matters, the product range is continuously changing, and each new product requires different parts. We therefore need to choose a supplier that has a very broad range of products available, and with the ability to provide products upon request that are currently not in their catalogue. We also do not know how much the company may charge for future products.

Currently there are only two suppliers, referred to as Company A and B, to choose between. To compare the companies we have taken a random sample of our current input products, and asked both companies to provide a unit-price for each of the products. We will use this data to infer if there is one company is better than the other.

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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:a9ac72e6f7
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
msg_success <- "Correct! We want to compare the average unit-price for the two companies, meaning we are dealing with inference for numerical data. We further asked both companies to provide a quote for the same randomly sampled products, therefore we are dealing with paired-data since each case deals with the same product."
test_mc(correct = 6, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_bad, msg_bad, msg_success, msg_bad))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:03c9a0637d
## More movies

In the previous exercise, you saw a dataset about movies. In this exercise, we'll have a look at yet another dataset about movies!

A dataset with a selection of movies, `movie_selection`, is available in the workspace.

*** =instructions
- Check out the structure of `movie_selection`.
- Select movies with a rating of 5 or higher. Assign the result to `good_movies`.
- Use `plot()` to  plot `good_movies$Run` on the x-axis, `good_movies$Rating` on the y-axis and set `col` to `good_movies$Genre`.

*** =hint
- Use `str()` for the first instruction.
- For the second instruction, you should use `...[movie_selection$Rating >= 5, ]`.
- For the plot, use `plot(x = ..., y = ..., col = ...)`.

*** =pre_exercise_code
```{r}
# You can also prepare your dataset in a specific way in the pre exercise code

library(MindOnStats)
data(Movies)
movie_selection <- Movies[Movies$Genre %in% c("action", "animated", "comedy"),c("Genre", "Rating", "Run")]

# Clean up the environment
rm(Movies)
```

*** =sample_code
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection


# Select movies that have a rating of 5 or higher: good_movies


# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre

```

*** =solution
```{r}
# movie_selection is available in your workspace

# Check out the structure of movie_selection
str(movie_selection)

# Select movies that have a rating of 5 or higher: good_movies
good_movies <- movie_selection[movie_selection$Rating >= 5, ]

# Plot Run (i.e. run time) on the x axis, Rating on the y axis, and set the color using Genre
plot(good_movies$Run, good_movies$Rating, col = good_movies$Genre)
```

*** =sct
```{r}
# SCT written with testwhat: https://github.com/datacamp/testwhat/wiki

test_function("str", args = "object",
              not_called_msg = "You didn't call `str()`!",
              incorrect_msg = "You didn't call `str(object = ...)` with the correct argument, `object`.")

test_object("good_movies")

test_function("plot", args = "x")
test_function("plot", args = "y")
test_function("plot", args = "col")

test_error()

success_msg("Good work!")
```
