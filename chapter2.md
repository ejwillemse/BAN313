--- 
title_meta  : Case study 5
title       : Case study 5 - Yacht production planning
description : In this case study we will try and predict the production time of luxary yachts using data on previous productions. Our objective is to develop a predictive model to be used for production planning. To develop the model we will analyse different variables from previous productions to identify the best predictor, and thereafter use linear-regression to fit a least-squared regression line, to be used for predicting the manufacturing time of our yachts.

--- type:NormalExercise lang:r xp:0 skills:1 key:ec139ff825
## Background

In this case study we will try and predict the production time of luxary yachts using data on previous productions. 
Our objective is to develop a predictive model to be used for production planning. 
To develop the model we will analyse different variables from previous productions to identify the best predictor, and thereafter use linear-regression to fit a least-squared regression line, to be used for predicting the manufacturing time of our yachts.

A critical component of manufacturing is production planning, which includes, amongst others:

1. Determine the resource and material requirements for production.
2. Scheduling the work to be conducted in the facility, including the purchase and delivery of materials.
3. Setting up and delivering production orders to clients.

We are responsible for production planning at luxary yachts manufacturing company. 
Given the uniques of our products, our production operates on the produce-to-order prinicple, whereby manufacturing only starts once an order has been placed for a yacht. 
Clients are also in full control of the yacht specifications, and can decide on the size of the yacht, its interior and a bunch of other custimisations.
As a result, each order is unique and has its own materials and manufacturing requirements.
To manufacture the different yachts, we have a general purpose yacht-building facility, allowing us to build any type of yacht.
The only-limitation that we have is that we can only manufacture one yacht at a time.

A key input into production planning is the expected time that an order will take to complete.
This information is critical to communicate to the customer.
It is also used to predict when the next built will start, and to schedule its material delivery accordingly.

In this case study we will develop a simple predictive model, in the form of a linear-regression model, to predict how long a yacht will take to complete based on some of its key specifications.

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read the instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

To continue with this chapter, hit the 'Submit Answer' button.

*** =instructions

- Hit the 'Submit Answer' button when you're done reading the instructions.

*** =hint
Just hit the 'Submit Answer'.

```{r cars}
summary(cars)
```

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
success_msg("Let's try and use our knowledge on statistical inference to assist our company.")
```
