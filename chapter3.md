---
title_meta  : Case study 6
title       : Case study 6 - Different applications of probability distributions
description : In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. The distribitions that we look are normal, poisson and exponential, power-law, and uniform. After eye-balling the applicable distribution we will answer find the appropraite distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.

--- type:NormalExercise lang:r xp:0 skills:1 key:ec139ff825
## Background

In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. 
The distribitions that we look are normal, poisson and exponential, power-law, and uniform. 
After eye-balling the applicable distribution we will answer find the appropraite distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters. $x$.

A critical component of manufacturing is production planning, which includes, amongst others:

1. Determine the resource and material requirements for production.
2. Scheduling the work to be conducted in the facility, including the purchase and delivery of materials.
3. Setting up and delivering production orders to clients.

We are responsible for production planning at luxury yachts manufacturing company. 
Given the uniques of our products, our production operates on the produce-to-order principle, whereby manufacturing only starts once an order has been placed for a yacht. 
Clients are also in full control of the yacht specifications, and can decide on the size of the yacht, its interior and a bunch of other customisations.
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

Before continuing, have a quick look at the following tutorial as you may need to refer to it during the course of the chapter: 

* Refer to [http://www.cyclismo.org/tutorial/R/linearLeastSquares.html](http://www.cyclismo.org/tutorial/R/linearLeastSquares.html).

After going through the tutorial and to continue with this chapter, hit the 'Submit Answer' button.

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
success_msg("Let's try and use our knowledge on regression to assist our company.")
```
