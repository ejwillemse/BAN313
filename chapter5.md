---
title_meta  : Case study 8
title       : Case study 8 - Developing simulation models using probability distributions (introduction)
description : "In this case study we will revisit the drilling hole example and see what influence re-work will have on our process output. First we will analyse the process output and do a goodness-of-fit test to see which distribution hole-sizes follow. Thereafter we will perform bootstrap analysis on the key parameters of the distribution. We will then use the distribution and parameters to simulate the process to answer a few basic questions."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In this case study we will revisit the drilling hole example where we are investigating the production of a product.
During the product's manufacturing process, a hole with a diameter of 10cm has to be drilled into the product by a drill-press.
Due to process randomness, such as vibrations, the product moving around in the drill-clamp, defects in the product material, etc. the drill-hole is not always the same diameter.
It differs from hole to hole, and thus represents a random process.

An imperfect hole is expected and the product is designed to account for this. The hole has a 0.25cm tolerance limit.
If a hole diameter is within (9.75cm, 10.25cm) the product will work according to specifications, otherwise it is defective.

As the production planner we want to answer the following simple question.
If we manufacture 1000 products, how many will be defective because of the drill hole-size being either too large or too small?

**Before continuing with this chapter**, complete [Case study 7 - Fitting probability distributions and estimating distribution parameters](https://campus.datacamp.com/courses/industrial-analysis-using-r/10364?ex=1)

The specific R functions that are applicable to this chapter are:

```
rnorm(), seq(), for
```

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
success_msg("Let's complete the questions.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a44acfd135
## Simulating the drilling process

From a previous exercise we know the diameter of the drill holes, $X$, follows a normal distribution with the following parameters:

$$X \sim \mathcal{N}(\mu = 10, \sigma = 0.2)$$

The upper and lower tolerance limits for the hole are:

* $X\_{low} = 10 - 0.25 = 9.75$
* $X\_{up} = 10 + 0.25 = 10.25$

The probability, $p$, of a drill-hole falling outside the tolerances can be easily calculated using the following code:

```
p <- pnorm(9.75, 10, 0.2) + pnorm(10.25, 10, 0.2, lower.tail = FALSE)
```

If were were to manufacture 1000 products, we would expect $1000 \times p$ products to be defective.

An alternative way to answer the question to simulate the drilling process. We can artificially produce drill-holes and then check the number of drillholes that fall outside the specification limits.

This can be done using the following R function

```
rnorm()
```

You can type `?rnorm` in the console to find out more about the function. 

Similar to `pnorm`, it takes as input the mean and standard deviation of a normal distribution. It also takes `n` which is the number of samples that we want to draw from the normal distribution with the specified mean and standard deviation.

To simulate 1000 drill-holes we can use the function as follows:

```
drillHoleDiameters <- rnorm(n=1000, mean = 10, sd = 0.2)
```

To simulate a single drill-hole we can use the function as follows:

```
drillHoleDiameters <- rnorm(n=1, mean = 10, sd = 0.2)
```

Note that the same structure can be used to simulate any of R's built in distribution.

To complete this question answer the following:

*** =instructions

1. Simulate 1000 drill-holes sizes and assign your answer to `drillHoleDiameters`.
2. View a histogram of the simulated drill-holes.
3. Calculate and view the mean and standard deviation of the simulated drill-holes. You only have to view the two values.
4. Calculate the number of simulated drill-holes that fall outside the specification limits and assign your answer to `nOutsideSpec`. Hint: since `drillHoleDiameters` is a vector, we need to use the `length` function instead of `nrows`.
5. Is this number higher or lower than the expected number of holes out of 1000 that fall outside the specification limit? Use the $1000 \times p$ calculation as given in the question background. You just have to view the two numbers.
6. Simulate and view a single drill-hole size and assign your answer to `singleDrillHole`.
7. What is the probability of get the single drill-hole size or _smaller_? Assign your answer to `pSmaller` and view the result. Hint: use the `pnorm` function.

*** =hint

Use `drillHoleDiameters <- rnorm(n=1000, mean = 10, sd = 0.2)` to simulate a 1000 holes, and `singleDrillHole <- rnorm(n=1, mean = 10, sd = 0.2)` to simulate one hole. Use the subset operator with R's or command `|` to return drill holes that are outside specification. Use the `length` command to count the number of observations outside specification. The `hist` function can be used to view the distribution of the 1000 holes. To calculate `pSmaller` use `pnorm(singleDrillHole, mean = , sd = )`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Simulate 1000 drill-holes sizes and assign your answer to `drillHoleDiameters`.



#2. View a histogram of the simulated drill-holes.



#3. Calculate and view the mean and standard deviation of the simulated drill-holes. You only have to view the two values.



#4. Calculate the number of simulated drill-holes that fall outside the specification limits and assign your answer to `nOutsideSpec`. Hint: since `drillHoleDiameters` is a vector, we need to use the `length` function instead of `nrows`.



#5. Is this number higher or lower than the expected number of holes out of 1000 that fall outside the specification limit? Use the 1000 x p calculation as given in the question background. You just have to view the two numbers.



#6. Simulate and view a single drill-hole size and assign your answer to `singleDrillHole`.



#7. What is the probability of get the single drill-hole size or _smaller_? Assign your answer to `pSmaller` and view the result. Hint: use the `pnorm` function.



```

*** =solution
```{r}
drillHoleDiameters <- rnorm(n=1000, mean = 10, sd = 0.2)
hist(drillHoleDiameters)
nOutsideSpec <- length(subset(drillHoleDiameters, drillHoleDiameters < 10-0.25 | drillHoleDiameters > 10+0.25))
singleDrillHole <- rnorm(n=1, mean = 10, sd = 0.2)
pSmaller <- pnorm(singleDrillHole, 10, 0.2)
```

*** =sct
```{r}
test_object("drillHoleDiameters", undefined_msg = "Make sure to define an object `drillHoleDiameters`.", incorrect_msg = "Make sure that you sampled 1000 drill-holes using the `rnorm` function and assigned your answer to `drillHoleDiameters`")       

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of hole `drillHoleDiameters`.", incorrect_msg = "Draw a histogram of `drillHoleDiameters`.")

test_object("nOutsideSpec", undefined_msg = "Make sure to define an object `nOutsideSpec`.",
incorrect_msg = "Make sure that you calculated the number of simulated drill-holes that fall outside the hole-specifications correctly and assigned your answer to `nOutsideSpec`.")

test_object("singleDrillHole", undefined_msg = "Make sure to define an object `singleDrillHole`.", incorrect_msg = "Make sure that you sampled one drill-hole using the `rnorm` function and assigned your answer to `singleDrillHole`")  

test_object("pSmaller", undefined_msg = "Make sure to define an object `pSmaller`.", incorrect_msg = "Make sure that you calculated the probability of getting a smaller drill-hole than the one just sampled correctly using the `pnorm` function and assigned your answer to `pSmaller`")

success_msg("Correct! By using R's built-in sampling functions we can simulate processes by randomly sampling from the process' distribution. In the next question we will look at a more complex version of the drill-hole example.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:21188c8230
## Simulating the drilling process with rework

We know the diameter of the drill-holes is $X \sim \mathcal{N}(\mu = 10, \sigma = 0.2)$, and that the drill-hole should have a diameter of 10cm with a tolerance of 0.25cm.

In practice we have an option to rework defective products.
Basically we can try to repair the defective product so that it meets specification.
Repairing is not always possible, and when it is possible it is not always effective.

For our drill-hole example we have the following repair options.
If a hole is too big the product is scrapped since we cannot make the hole smaller.
If a hole is too small, we can attempt to re-drill the hole using a specialized drilling machine.
With this second attempt, much can go wrong. The part might crack or the resulting hole may be too big, in which case the products will be scrapped in any-case. The probability of the rework being successful is known to be **65%**.


To simulate the rework process we can use the `sample` function with replacement.
First we need to determine the number of products that we will rework, `nRework`, and then use the probability of a rework being successful, `pFixed`.
Thereafter we can simulate which products are reworked successfully and which are note using the following code:

```
outcomes <- c(TRUE, FALSE) # rework can either be successful or not
pOutcome <- c(pFixed, 1 - pFixed) # probability of TRUE and FALSE
reworkSimulation <- sample(x = outcomes, size = nRework, replace = TRUE, pOutcome)
```

To simulate the number of products that are defective out of a production batch of 1000 we need to determine the number of products that will be immediately scrapped plus the number of products that will be scrapped after rework.

We can actually calculate the proportion of scrapped products analytically using the following formulas:

$$p_\text{scrap} = P(X > 10.25) + P(X < 9.75)\times (1-0.65),$$

where $P(X > 10.25)$ is the probability of a hole being larger than 10.25cm and $P(X < 10.25)$ is the probability of a hole being smaller than 9.75cm. The second term is multiplied by (1-0.65), which is the probability of a reworked product being unsuccessfully repaired, and therefore scrapped.

The formula thus means:

_The probability of a product being scrapped is equal to the probability of its hole being larger than 10.25cm **or** being smaller than 9.75cm **and*** being unsuccessfully reworked_.

To calculate $P(X > 10.25)$ and $P(X < 9.75)$ we can use the provided information that hole-size follows a normal distribution, and R's `pnorm` function. For example: `pnorm(9.75, 10, 0.25)` gives us the probability that the hole will be smaller than 9.75 and will be reworked.

To complete the question do the following:

*** =instructions

1. Simulate 1000 drill-holes sizes and assign your answer to `drillHoleDiameters`.
2. Calculate and view the number of simulated drill-holes that are too big and will immediately be scrapped. Assign your answer to `nScrap`.
3. Calculate and view the number of simulated drill-holes that are too small and will be reworked. Assign your answer to `nRework`.
4. Simulate the rework process using the `sample` function and assign your results to `reworkSimulation`.
5. Calculate and view the number of reworked products that were unsuccessfully repaired and scrapped. Assign your answer to `nReworkScrap`.
6. Calculate and view the total number of products, out of the 1000 simulated products, that were scrapped. Assign your answer to `nScrapTotal`.
7. From the simulation results, what proportion of products were scrapped? Assign your answer to `pScrappedSim`.
8. Calculate the analytical proportion of scrapped using the given formula and see how it compares the simulated proportion. Assign your answer to `pScrappedAct`.

*** =hint

Refer to the previous question on how to simulate drill-hole sizes. To simulate the rework, refer to the question background information. To count the number of successful reworks, either use the `subset` or `table` command, similar to how it was used for `pFixed`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Simulate 1000 drill-holes sizes and assign your answer to `drillHoleDiameters`.



#2. Calculate and view the number of simulated drill-holes that are too big and will immediately be scrapped. Assign your answer to `nScrap`.



#3. Calculate and view the number of simulated drill-holes that are too small and will be reworked. Assign your answer to `nRework`.



#4. Simulate the rework process using the `sample` function and assign your results to `reworkSimulation`.



#5. Calculate and view the number of reworked products that were unsuccessfully repaired and scrapped. Assign your answer to `nReworkScrap`.



#6. Calculate and view the total number of products, out of the 1000 simulated products, that were scrapped. Assign your answer to `nScrapTotal`.



#7. From the simulation results, what proportion of products were scrapped? Assign your answer to `pScrappedSim`.



#8. Calculate the analytical proportion of scrapped using the given formula and see how it compares the simulated proportion. Assign your answer to `pScrappedAct`.



```

*** =solution
```{r}
drillHoleDiameters <- rnorm(1000, 10, 0.2)
nScrap <- length(subset(drillHoleDiameters, drillHoleDiameters > 10 + 0.25))
nRework <- length(subset(drillHoleDiameters, drillHoleDiameters < 10 - 0.25))
pFixed <- 0.65
reworkSimulation <- sample(c(TRUE, FALSE), nRework, replace = TRUE, prob = c(pFixed, 1-pFixed))
nReworkScrap <- length(subset(reworkSimulation, reworkSimulation == TRUE))
nScrapTotal = nScrap + nReworkScrap
pScrappedSim <- nScrapTotal/1000
pScrappedAct <- pnorm(10 + 0.25, 10, 0.2, lower.tail = FALSE) + 0.36*pnorm(10 - 0.25, 10, 0.2, lower.tail = TRUE)
```

*** =sct
```{r}
test_object("drillHoleDiameters", undefined_msg = "Make sure to define an object `drillHoleDiameters`.", incorrect_msg = "Make sure that you sampled 1000 drill-holes using the `rnorm` function and assigned your answer to `drillHoleDiameters`")       

test_object("nScrap", undefined_msg = "Make sure to define an object `nScrap`.", incorrect_msg = "Make sure that you calculated the number of products that will be scrapped immediately correctly and assign your answer to `nScrap`.")

test_object("nRework", undefined_msg = "Make sure to define an object `nRework`.", incorrect_msg = "Make sure that you calculated the number of products that will be reworked correctly and assign your answer to `nRework`.")

test_object("reworkSimulation", undefined_msg = "Make sure to define an object `reworkSimulation`.", incorrect_msg = "Make sure that you simulated the rework process by using the `sample` function and the given probability of a rework being successful and assign your answer to `reworkSimulation`. You have to take `nRework` samples.")

test_object("nReworkScrap", undefined_msg = "Make sure to define an object `nReworkScrap`.", incorrect_msg = "Make sure that you calculated the number of reworked products that were unsuccessfully repaired and scrapped correctly and assign your answer to `nReworkScrap`.")

test_object("nScrapTotal", undefined_msg = "Make sure to define an object `nScrapTotal`.", incorrect_msg = "Make sure that you calculated the total number of scrapped correctly and assign your answer to `nScrapTotal`.")

test_object("pScrappedSim", undefined_msg = "Make sure to define an object `pScrappedSim`.", incorrect_msg = "Make sure that you calculated the proportion of scrapped products correctly and assign your answer to `pScrapped`. The proportion can be calculated using `nScrapTotal` and the number of holes drilled in the simulation.")

test_object("pScrappedAct", undefined_msg = "Make sure to define an object `pScrappedAct`.", incorrect_msg = "Make sure that you calculated the analytical proportion of scrapped products correctly and assign your answer to `pScrappedAct`. The proportion should be calculated using the given formulas and the `pnorm` function.")

success_msg("Correct! By using R's built in functions we simulated the drilling process with rework, which is a bit more complex than the previous example. The simulated proportion is different from the actual proportion since the simulated proportion presents one random instance of manufacturing a 1000 product batch. Should we simulate another batch we will get a different proportion. It's not always easy, or even possible, to calculate the true proportion analytically. Sometimes we have to develop a simulation model to analyse process output. Having the simulation also allows us to use it for inference. This will be illustrated in the next question.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:99f60f5510

### Inference with simulation: scrapping too many parts (Part 1)

A production manager was recently appointed to oversee the production line that includes the drill-press process. In the first day of his appointed a small production run was completed of 30 products. Of the 30 products, 8 had to be scrapped due to the hole size.

Management is very concerned about the high-scrap proportion of 27%, especially since we indicated to Management that the probability of a product been scrapped is only 14%. Management is considering firing the production line manager.

Is there sufficient evidence to do so?

We wish to determine if the process is producing the same outputs as before, or whether the process has somehow worsen under his supervision.

We can formally investigate the issue using inference for proportion. The two competing hypotheses that we can test are:

* $H_0$: The true proportion of defective products manufactured under the new supervisor is 14%;
* $H_A$: The true proportion of defective products manufacture under the new supervisor is **more** than 14%.

Consistent with a hypothesis test we will assume that $H_0$ is true, and then calculate the probability of getting a defect-proportion from a batch of 30 products of 27% or more. To calculate the probability we will simulate $H_0$ using the code from our previous example.

To complete the question do the following:

*** =instructions

1. Simulate 30 drill-holes sizes and assign your answer to `drillHoleDiameters`.
2. Calculate the total number of products, out of the 30 simulated products, that were scrapped. Assign your answer to `nScrapTotal`. This will include the products that were immediately scrapped and that were scrapped after rework.
3. From the simulation results, what proportion of products were scrapped? Assign your answer to `pScrappedSim` and view the result and compare it against the 27% scrap-proportion of the production manager.

*** =hint

This is a repeat of the previous exercise. Set the number of drill-holes equal to 30 instead of 1000.

*** =pre_exercise_code
```{r}
# 1. Simulate 30 drill-holes sizes and assign your answer to `drillHoleDiameters`.



# 2. Calculate the total number of products, out of the 30 simulated products, that were scrapped. Assign your answer to `nScrapTotal`. This will include the products that were immediately scrapped and that were scrapped after rework.



# 3. From the simulation results, what proportion of products were scrapped? Assign your answer to `pScrappedSim` and view the result and compare it against the 27% scrap-proportion of the production manager.



```

*** =solution
```{r}
batchSize = 30
drillHoleDiameters <- rnorm(batchSize, 10, 0.2)
nScrap <- length(subset(drillHoleDiameters, drillHoleDiameters > 10 + 0.25))
nRework <- length(subset(drillHoleDiameters, drillHoleDiameters < 10 - 0.25))
pFixed <- 0.65
reworkSimulation <- sample(c(TRUE, FALSE), nRework, replace = TRUE, prob = c(pFixed, 1-pFixed))
nReworkScrap <- length(subset(reworkSimulation, reworkSimulation == TRUE))
nScrapTotal = nScrap + nReworkScrap
pScrappedSim <- nScrapTotal/batchSize
```

*** =sct
```{r}
test_object("drillHoleDiameters", undefined_msg = "Make sure to define an object `drillHoleDiameters`.", incorrect_msg = "Make sure that you sampled 30 drill-holes using the `rnorm` function and assigned your answer to `drillHoleDiameters`")       

test_object("nScrapTotal", undefined_msg = "Make sure to define an object `nScrapTotal`.", incorrect_msg = "Make sure that you calculated the total number of scrapped correctly and assign your answer to `nScrapTotal`.")

test_object("pScrappedSim", undefined_msg = "Make sure to define an object `pScrappedSim`.", incorrect_msg = "Make sure that you calculated the proportion of scrapped products correctly and assign your answer to `pScrapped`. The proportion can be calculated using `nScrapTotal` and the number of holes drilled in the simulation.")

success_msg("Correct! We can easily simulate 30 products using our previous code. The only question is, does this provide evidence that the manager is somehow resulted in the process performing poorly?")
```

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:00816b640d

### Inference with simulation: scrapping too many parts (Part 2)

Comparing the simulated 30 product scrap-proportion against the manager's 30 product scrap-proportion, do you believe that the new manager should be fired?

*** =instructions

* Yes, he should be fired.

* No, he should not be fired.

* Cannot say, we have to perform additional analysis.

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_bad <- "That is incorrect."

msg_success <- "Correct. It may be tempting to make a decision using the manager's actual proportion and the simulated proportion, but we only performed one-simulation. We know the process is random so using one simulation doesn't proof anything. It's similar to management firing the manager based on one production run of 30 products. Ideally management should evaluate the manager over thousands of production runs, and thereby see if the bad production run was just due to bad-luck, or if it happens consistently. The same principle applies to the simulation. We need to perform multiple simulations and look at the distribution of the simulation results. Only then can we see how rare the manager's proportion is. This is easy to do with computers and R."
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:06ab68ef37

### Inference with simulation: scrapping too many parts (Part 3)

Recall that we wish to determine if the process is producing the same outputs as before, or whether the process has somehow worsen under his supervision.

The two competing hypotheses that we can test are:

* $H_0$: The true proportion of defective products manufactured under the new supervisor is 14%;
* $H_A$: The true proportion of defective products manufacture under the new supervisor is **more** than 14%.

Consistent with a hypothesis test we will assume that $H_0$ is true, and then calculate the probability of getting a defect-proportion from a batch of 30 products of 27% or more. To calculate the probability we will simulate $H_0$ using the code from our previous example.

This requires us to perform thousands of simulations and capture the scrap-proportion of each. Thereafter we can analyse the distribution of the scrapping-proportion, and lastly calculate the number of simulations which had a scrapping-proportion of more than 27%. This number can then be used to calculate the probability of having hist scrapping-proportion or higher, thus representing our $p$-value. If the value is small we can conclude that there is sufficient evidence that the manager did indeed have an influence on the scrapping-proportion. The issue can then be investigate further.

To calculate the probability, which is basically just our $p\text{-value}$, we need to run our simulation numerous times (thousands if it runs quick enough), and capture the proportion for each run. Thereafter we can analyse the simulated proportions statistically and find the number of simulations that produced a scrapping-proportion that was similar or higher. This value can then be used to calculate the $p\text{-value}$ which is the probability of observing the a 27% scrapping-proportion or higher when the drilling-process is still the same and the manager did not influence it.

A `for` loop is required for this question, as well as initiating an empty vector, using the `rep` command, to store the simulation results into. We simply need to place the previous simulation code inside the `for` the loop. We will then repeat the simulation 10000 times and analyse the results.

To complete the question do the following:

*** =instructions

1. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `propSimulated`.
2. Simulate the 30 batch drilling-process 10000 times, calculate each simulation's scrapping-proportion and store the results in the appropriate `i` position of `propSimulated`.
3. Draw a histogram of `propSimulated`.
4. Calculate the **number** of simulations which had a scrapping-proportion of 27% or more and assign your answer to `nScrap27`.
5. Using `nScrap27`, calculate the $p\text{-value}$ for the hypothesis test and assign your answer to `p_value`.
6. Assuming a significance level of $\alpha = 0.05$, decide if there is sufficient evidence to reject the null hypothesis. Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.

*** =hint
```{r}

```

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Use the `rep` function to initiate a vector consisting 10000 `NAs` and assign it to `propSimulated`.

propSimulated <- rep(

#2. Simulate the 30 batch drilling-process 10000 times, calculate each simulation's scrapping-proportion and store the results in the appropriate `i` position of `propSimulated`.

for (i in 1:10000)
{
  drillHoleDiameters <- rnorm(
  nScrap <-
  nRework <-

  pFixed <- 0.65
  reworkSimulation <- sample(
  nReworkScrap <-

  nScrapTotal = nScrap + nReworkScrap

  pScrappedSim <-
  propSimulated[i] <- pScrappedSim
}

#3. Draw a histogram of `propSimulated`.

hist(propSimulated)

#4. Calculate the **number** of simulations which had a scrapping-proportion of 27% or more and assign your answer to `nScrap27`.



#5. Using `nScrap27`, calculate the p-value for the hypothesis test and assign your answer to `p_value`.



#6. Assuming a significance level of alpha = 0.05, decide if there is sufficient evidence to reject the null hypothesis. Your answer should be either `TRUE` for _we reject the null hypothesis_ or `FALSE` for _we do not have enough evidence to reject the null hypothesis_. Assign your `TRUE` or `FALSE` answer to the `rejectH0` variable.



```

*** =solution

```{r}
propSimulated <- rep(NA, 10000)
for (i in 1:10000)
{
  drillHoleDiameters <- rnorm(30, 10, 0.2)
  nScrap <- length(subset(drillHoleDiameters, drillHoleDiameters > 10 + 0.25))
  nRework <- length(subset(drillHoleDiameters, drillHoleDiameters < 10 - 0.25))

  pFixed <- 0.65
  reworkSimulation <- sample(c(TRUE, FALSE), nRework, replace = TRUE, prob = c(pFixed, 1-pFixed))
  nReworkScrap <- length(subset(reworkSimulation, reworkSimulation == TRUE))

  nScrapTotal = nScrap + nReworkScrap

  pScrappedSim <- nScrapTotal/30
  propSimulated[i] <- pScrappedSim
}

hist(propSimulated)
nScrap27 <- length(subset(propSimulated, propSimulated >= 0.27))

p_value <- nScrap27/10000

if(p_value < 0.05){rejectH0 <- TRUE}else{rejectH0 <- FALSE}
```
*** =sct
```{r}
test_object("propSimulated", undefined_msg = "Make sure to define an object `propSimulated`.", incorrect_msg = "Make sure that you stored the scrapping-proportion of each of the 10000 30-drill-holes simulations in `propSimulated`.")       

test_function("hist", args = c("x"), undefined_msg = "Draw a histogram of `propSimulated`",
incorrect_msg = , undefined_msg = "Draw a histogram of `propSimulated`.")

test_object("nScrap27", undefined_msg = "Make sure to define an object `nScrap27`.", incorrect_msg = "Make sure that you calculated the total number of simulations which had a scrapping-proportion of 27% or higher correctly and assign your answer to `nScrap27`.")

test_object("p_value", undefined_msg = "Make sure to define an object `p_value`.", incorrect_msg = "Make sure that you calculated the probability of having a scrapping-proportion of 27% or higher correctly and assign your answer to `nScrap27`.")

test_object("rejectH0", undefined_msg = "Make sure to define a variable `rejectH0`.",
            incorrect_msg = "Based on `p_value`, make sure that you correctly assigned the `TRUE` or `FALSE` value to `rejectH0`.")

success_msg("Correct! We have now used the simulation model to perform a hypothesis test. The last question is then, how do we use the model results to assist management in their decision the production manager.")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:698fda148f

### Inference with simulation: scrapping too many parts (Part 4)

Based on the hypothesis, should the new manager should be fired?

*** =instructions

* Yes, there is enough evidence that he had a negative effect on the process.

* No, there is not enough evidence that he had a negative effect on the process.

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_bad <- "That is incorrect."

msg_success <- "Correct. There is not enough evidence to reject the null-hypothesis. Therefore we tell management there is not enough evidence to infer that he had a negative effect on the process. He may have just been unlucky overseeing a `bad` production batch, which happens by chance. On another day, a good production batch may happen whereby very few products are scrapped. This is just part of the randomness of the process."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success))
```
