---
title_meta  : Case study 9
title       : Case study 9 - Developing simulation models using probability distributions (advanced)
description : "In this case study we will analyse how a random order process influences inventory levels and stock-outs of our product. We will further analyse the impact of a random production process, identical to the one from the previous chapter, on inventory levels."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In the previous case study we modelled a random production process.
In this chapter we will move to the supply-side of our business and model a random order process for our product.

**Before continuing with this chapter, please do the following:**

1. Complete [Case study 8 - Developing simulation models using probability distributions (introduction)](https://campus.datacamp.com/courses/industrial-analysis-using-r/10739?ex=1) on Datacamp.
2. Complete [Writing functions in r - A quick refresher](https://www.datacamp.com/courses/writing-functions-in-r) on Datacamp.
3. Go through the following tutorial on using the [if-else statement in R](https://www.tutorialspoint.com/r/r_if_else_statement.htm).
3. Review the lecture slides (Lecture_Week8.pdf](TBP), available on [clickUP](https://clickup.up.ac.za) under [Theme 2: Simulation models](https://clickup.up.ac.za/webapps/blackboard/content/listContent.jsp?course_id=_108170_1&content_id=_1016179_1&mode=reset).

**After completing the above, read the below instructions carefully.**

Each day, customers place orders for our product.
Each customer is unique and have different requirements, therefore the amount of products ordered differs from day-to-day.
Inventory is used to account for the daily variation.

Each day a fixed amount of products are manufactured and stored as inventory.
For low-order days, the surplus manufactured products are placed in inventory, to be used for high-order days.

As the inventory manager we wish to predict our inventory levels for the coming month and see how likely a stock-out is.

We have a fixed production-run setup, meaning we produce a fixed number of products each day.
Each day's manufactured products is moved to inventory the day after its manufactured before the start of business.

When customers place orders, it is filled form the available inventory on the same day.
If the day's orders cannot be filled, due to low inventory levels, the customers take the available inventory and purchases the balance elsewhere.

*For example*, on Day 1 we manufactured 150 products.
On the same day our starting inventory level was 100 products, and we received orders for 50 products.
Our ending inventory on Day 1 is then 50 products (100 - 50).
On Day 2 our starting inventory will increase by 150 products (the products manufactured the previous day) and we will thus have 200 products (50 + 150) in inventory.

On Day 2 we again manufactured 150 products since we have a fixed 150-product production run.
On this day our starting inventory level was 200 products, and we received orders for 225 products.
Since we received orders for more products than we have, our ending inventory will be 0 products.
We sold our available 200 products, and the customers had to purchase 25 products elsewhere.
This also means that we had a stock-out, since we ran out of inventory.
On Day 3 our starting inventory will again increase by 150 products and will we will thus have 150 products (0 + 150) in inventory.

From the above example, we want to predict our daily starting and ending inventory levels over a period of 30 days, as well as the proportion of stock-outs over the study period, which is the total number of stock-outs over the period divided by the number of days in the study period.


In this lab we do the following:

1. Develop a simulation model of our stock-outs and inventory levels based on random orders.
2. Incorporate the drilling-process simulation model from our previous chapter to account for defective products and random orders, and predict stock-outs and inventory levels.
3. Predict how increased inventory space and larger production runs will influence stock-outs and inventory levels.

The specific R functions that are applicable to this chapter are:

```
runif(), rnorm(), rep(), sample(), function(), round(), for, if
```

To find out more about the functions, type `?` followed by the R-function in the R console.

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip the each questions' background information. The question backgrounds contain valuable information to assist you in completing the questions.

To continue with this chapter confirm the following:

*** =instructions

* I have not completed all the prescribed preparation material, as listed above, or have not read all the instructions on this page.

* I confirm that I have completed the prescribed preparation material, as listed above, and have read **ALL** the instructions on this page carefully.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =sct
```{r}
msg_bad <- "Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."

msg_success <- "Let's get started with the Lab. Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success))
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:a44acfd135
## Determining the starting and ending inventory levels after a few days

Let $I\_\text{start}(t)$ be the starting inventory-level and let $I\_\text{end}(t)$ be the ending inventory-level on day $t$.
Let $P$ be the fixed amount of products produced per day, and let $O(t)$ be the number of products ordered on day $t$.

The starting and ending inventory levels can the be calculated as follow:

$$I\_\text{end}(t) = \max(0, I\_\text{start}(t) - O(t)),$$
$$I\_\text{start}(t + 1) = I\_\text{end}(t) + P.$$

Carefully study the equations above and make sure you understand how inventory levels are calculated.

The ending inventory for a specific day is equal to the starting inventory for that day minus the number of products ordered. But the level can never be less than 0. The starting inventory for the next day is then equal to the ending inventory of the previous day plus the number of products manufactured during the previous day.

For the first question we are going to look at a sequence of product orders for three consecutive days and calculate the starting inventory level for the *fourth* day.

The number of products ordered for the three days were as follows:

* Day 1: 124
* Day 2: 137
* Day 3: 101

Due to a machine break-down our starting inventory-level for Day 1 was 120 units. It was fixed, and we had a fixed production-run of 150 products for Days 1 to 3, meaning we manufacture 150 products per day.

Based on the above information, what was our **starting** inventory level on Day 4 ($I\_\text{start}(4)$)? You may use the console screen on the right to do the necessary calculations.


*** =instructions

* 62

* 205

* 212

* 235

* 355

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =hint

Use the given formulas to calculate the starting and ending inventory levels for the first three days, thereafter you the ending inventory level of Day 3 to calculate the starting inventory level for Day 4.

*** =sct
```{r}
msg_bad1 <- "That is incorrect. Carefully go through the given formulas. Remember that the starting inventory for a specific day includes the number of products that we manufactured the previous day. The given starting inventory for Day 1 thus includes the products manufactured on the previous day. So should the starting inventory for Day 4."

msg_bad2 <- "That is incorrect. Carefully go through the given formulas. And note that we can only sell what we have in-stock, so our ending-inventory level can never be negative. Remember that the starting inventory for a specific day includes the number of products that we manufactured the previous day."

msg_success3 <- "Correct. The starting inventory level for Day 4 is 212. Our starting inventory for Day 1 was given as 120 units. The ending inventory level for Day 1 is 0, since we did not have enough inventory to meet the 124 products ordered. Our starting inventory for Day 2 was 150, since the previous day's production was available in our inventory. The ending inventory for Day 2 was then 13 (150 - 137). The starting and ending inventory levels for Day 3 was 163 and 62 products, respectively. Therefore the starting inventory level for Day 4 was 62 + 150 = 212 products. We further know that we had a stock-out on Day 1 since we didn't have enough products to meet the demand for that day."

msg_bad4 <- "That is incorrect. Carefully go through the given formulas. The given starting inventory for Day 1 includes the products manufactured on the previous day. It was less than 150 products due to a machine break-down."

msg_bad5 <- "That is incorrect. Carefully go through the given formulas. Remember that the starting inventory for a specific day includes the number of products that we manufactured the previous day. The given starting inventory for Day 1 thus includes the products manufactured on the previous day."

test_mc(correct = 3, feedback_msgs = c(msg_bad1, msg_bad2, msg_success3, msg_bad4, msg_bad5))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:592f5b5fae
## Calculating starting and ending inventory levels and stock-outs

Using the following formulas we can calculate the starting and ending inventory levels for consecutive days:

$$I\_\text{end}(t) = \max\(0, I\_\text{start}(t) - O(t)),$$
$$I\_\text{start}(t + 1) = I\_\text{end}(t) + P.$$

Instead of calculating the levels by hand we can write an R programme to do so for us.

Assume that the setup is the same as before, where we manufacture 150 products per day and our starting inventory for Day 1 is 120 products. The number of products ordered, $O(t)$, for 10 consecutive days were:

```
O <- c(148, 195, 140, 147, 193, 104, 159, 144, 107, 137)
```

In the above code we have stored the number of products ordered in the vector `O`. Day's 2 orders, $O(t=2)$, is then `O[2]`. Our starting inventory for Day 1 is $I\_\text{start} = 120$, and our production batch size is $P = 150$.

In this exercise we will track our starting and ending inventory-levels from Days 1 to 10, and for each day calculate if there was a stock-out.

To do so we will initialise, populate and analyse the following three vectors:

```
I_start <- rep(NA, 6)
I_end <- rep(NA, 5)
stockOut <- rep(NA, 5)
```

The reason for using `I_start <- seq(NA, 6)` and not `I_start <- seq(NA, 5)` will be explored later on.

Since it was given that $I\_\text{start} = 120$, we have to set `I_start[1] <- 120`.

The following code calculates the ending inventory level for Day 1:

```
I_end[1] <- max(0, I_start[1] - O[1])
```

The `max(0,...)` function ensures that inventory is set to 0 when more products are ordered than available. The only problem with the above code is that it does not explicitly take stock-outs into account. If $I\_\text{end}(t) = 0$ it does not always mean a stock-out occurred on day $t$. It is possible that the number of products ordered was exactly equal to the number of products in our inventory.

An `if(){}else{}` statement can be used to model stock-outs, and to update the `stockOut` variable:

```
if(I_start[1] < O[1])
{
  stockOut[1] <- 1
  I_end[1] <- 0
}else{
  stockOut[1] <- 0
  I_end[1] <- I_start[1] - O[1]
}
```

Carefully go through the above code to see how stock-outs are modelled.

If the products ordered on Day 1 is more than the available products then we say that a stock-out occurred on that day by setting `stockOut[1] <- 1`.
We can also choose to set `stockOut[1] <- TRUE`, but using `stockOut[1] <- 1` for a stock-out and `stockOut[1] <- 0` otherwise makes it a bit easier to calculate the number of stock-outs later on. It will simply be `sum(stockOut)`.
If a stockout occurred, our ending inventory is set to 0.
If a stockout did not occur, meaning we had enough products in inventory, then our ending inventory is equal to our starting inventory minus the number of products ordered.

The last part is then to calculate the starting inventory for the next day $t + 1$:

```
I_start[2] <- I_end[1] + P
```
where `P` is the fixed number of products that we produce per day, and which is available for sale the next day.

Instead of hardcoding $t$, we can make our code more generic to calculate the starting and ending inventory, as well as the probability for a stock-out for day $t$:

```
if(I_start[t] < O[t])
{
  stockOut[t] <- 1
  I_end[t] <- 0
}else{
  stockOut[t] <- 0
  I_end[t] <- I_start[t] - O[t]
}

I_start[t+1] <- I_end[t] + P
```

For Day 1, we just need to set `t = 1` and run the above code.
For Day 2, we will then set `t = 2` and run the above code, and we can continue doing so for `t=3`, `t=4` and lastly for `t=5`.

Note that the last line of the generic code always calculates the starting inventory for the next day, $t+1$.
We will therefore end up with a calculated starting inventory level for day $t = 6$, even though we are only really interested in five days.
We can use another `if` statement to not calculate the `I_start[t+1]` if we are at the end of the study period.
But easier is to just create an additional storage space in `I_start` by initialising it as `I_start <- seq(NA, 5 + 1)`.

The last update in our code is to place the inventory-level and stock-out calculations in a `for` loop which represents our study-period:

*** =instructions
1. Complete the given code in the R script file to right to calculate `stockOut`, `I_start` and `I_end` for the five-day study period.
2. Draw a barplot of `stockOut`, `I_start` and `I_end`.
3. Calculate and view the mean ending inventory-level and assign your answer to `mean_I_end`.
4. Calculate and view the total number of stock-outs during the study period and assign your answer to `nStockOut`.
5. Calculate and view the proportion of stock-outs over the five days and assign your answer to `pStockOut`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Complete the following code to calculate `stockOut`, `I_start` and `I_end` for the five-day study period.

O <- c(148, 195, 140, 147, 193, 104, 159, 144, 107, 137)

P <- 150
I_start <- rep(NA, 6)
I_end <-
stockOut <-

I_start[1] <-

for (t in 1:5)
{
  if()
  {
    # stock-out occurred, not all order were met

  }else{
    # stock-out did not occur, all orders were met

  }
  # calculate the starting inventory for the next day, t + 1

}

#2. Draw a barplot of `stockOut`, `I_start` and `I_end`.



#3. Calculate and view the mean ending inventory-level and assign your answer to `mean_I_end`.



#4. Calculate and view the total number of stock-outs during the study period and assign your answer to `nStockOut`.



#5. Calculate and view the proportion of stock-outs over the five days and assign your answer to `pStockOut`.



```

*** =solution
```{r}
O <- c(148, 195, 140, 147, 193, 104, 159, 144, 107, 137)

startingInventory <- 120
P <- 150

I_start <- rep(NA, 6)
I_end <- rep(NA, 5)
stockOut <- rep(NA, 5)

I_start[1] <- 120

for (t in 1:5)
{
  if(I_start[t] < O[t])
  {
    stockOut[t] <- 1
    I_end[t] <- 0
  }else{
    stockOut[t] <- 0
    I_end[t] <- I_start[t] - O[t]
  }
  I_start[t+1] <- I_end[t] + P
}

barplot(stockOut)
barplot(I_start)
barplot(I_end)

mean_I_end <- mean(I_end)
nStockOut <- sum(stockOut)
pStockOut <- nStockOut/10
```

*** =sct
```{r}
test_object("stockOut", undefined_msg = "Make sure to define an object `stockOut`.",
incorrect_msg = "Something went wrong in calculating `stockOut`. Remember that it has to be calculated for each day `t`.")

test_object("I_start", undefined_msg = "Make sure to define an object `I_start`.",
incorrect_msg = "Something went wrong in calculating `I_start`. Remember that it has to be calculated for each day `t`.")

test_object("I_end", undefined_msg = "Make sure to define an object `I_end`.",
incorrect_msg = "Something went wrong in calculating `I_end`. Remember that it has to be calculated for each day `t`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `stockOut`.",
incorrect_msg = "Draw a barplot of `stockOut`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `I_start`.",
incorrect_msg = "Draw a barplot of `I_start`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `I_end`.",
incorrect_msg = "Draw a barplot of `I_end`.")

test_object("mean_I_end", undefined_msg = "Make sure to define an object `mean_I_end`.",
incorrect_msg = "Something went wrong in calculating `mean_I_end`. It should be the mean ending-inventory level `I_end`.")

test_object("nStockOut", undefined_msg = "Make sure to define an object `nStockOut`.",
incorrect_msg = "Something went wrong in calculating `nStockOut`. It should be the total number of days on which a stock-out occurred. Use `sum` to calculate it.")

test_object("pStockOut", undefined_msg = "Make sure to define an object `pStockOut`.",
incorrect_msg = "Something went wrong in calculating `pStockOut`. It should be the proportion of days on which a stock-out occurred. Use `nStockOut` and the number of days in our study period to calculate it.")

success_msg("Correct! The given code can be used to calculate the inventory-levels for any number of days based on known orders. As mentioned, orders are random. In the next question we are going to take this into account by transforming our code into a simulation model.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:42987389b3
## Simulating inventory levels and stock-outs

With the necessary code in place to calculate inventory levels and stock-outs based on product orders, we can now proceed to develop a simulation model.

The number of products ordered, $O$, on any give day is random but known to follow a uniform distribution with the following parameters:

$$O \sim \mathcal{U}(\min = 100, \max = 200).$$

Based on this information we can easily update our previous code to simulate the inventory levels and stock-outs over any study period.

In this exercise our starting inventory for Day 1 will again be 120 units and we manufacture 150 units per day. 
The only new part is to model orders as a random process that follows a uniform distribution.

We can simulate the product orders for a total of `n` days using the following code

```
O <- round(runif(n, 100, 200), 0)
```

The `round` function rounds the number of orders to the nearest interger.

Our previous code can then be used as-is to calculate inventory and stock-out levels after incorporating `n` into the necessary places in our code.

This will be left as an exercise:

*** =instructions

1. Generate random product orders for `n=30` days and assign the daily orders to `O`.
2. Update the previous code to calculate and view the inventory levels and stock-outs for the 30 days' product orders.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Generate random product orders for `n=30` days and assign the daily orders to `O`.

n = 30
O <-

#2. Update the code below to calculate and view the inventory levels and stock-outs for the 30 days' simulated product orders.

tartingInventory <- 120
P <- 150

I_start <- rep(NA, 6)
I_end <- rep(NA, 5)
stockOut <- rep(NA, 5)

I_start[1] <- 120

for (t in 1:5)
{
  if(I_start[t] < O[t])
  {
    stockOut[t] <- 1
    I_end[t] <- 0
  }else{
    stockOut[t] <- 0
    I_end[t] <- I_start[t] - O[t]
  }
  I_start[t+1] <- I_end[t] + P
}

barplot(stockOut)
barplot(I_start)
barplot(I_end)

mean_I_end <- mean(I_end)
mean_I_end

nStockOut <- sum(stockOut)
nStockOut

pStockOut <- nStockOut/10
pStockOut

```

*** =solution
```{r}
n <- 30
O <- round(runif(n, 100, 200), 0)

P <- 150

I_start <- rep(NA, n + 1)
I_end <- rep(NA, n)
stockOut <- rep(NA, n)

I_start[1] <- 120

for (t in 1:n)
{
  if(I_start[t] < O[t])
  {
    # stock-out occurred, not all order were met
    stockOut[t] <- 1
    I_end[t] <- 0
  }else{
    # stock-out did not occur, all orders were met
    stockOut[t] <- 0
    I_end[t] <- I_start[t] - O[t]
  }

  # calculate the starting inventory for the next day, t + 1
  I_start[t+1] <- I_end[t] + P
}

barplot(stockOut)
barplot(I_start)
barplot(I_end)

mean_I_end <- mean(I_end)
nStockOut <- sum(stockOut)
pStockOut <- nStockOut/n
```

*** =sct
```{r}
test_object("O", undefined_msg = "Make sure to define an object `O`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders, `O`. Use the `runif` function for the simulation.")

test_object("stockOut", undefined_msg = "Make sure to define an object `stockOut`.",
incorrect_msg = "Something went wrong in calculating `stockOut`. Remember that it has to be calculated for each day `t`.")

test_object("I_start", undefined_msg = "Make sure to define an object `I_start`.",
incorrect_msg = "Something went wrong in calculating `I_start`. Remember that it has to be calculated for each day `t`.")

test_object("I_end", undefined_msg = "Make sure to define an object `I_end`.",
incorrect_msg = "Something went wrong in calculating `I_end`. Remember that it has to be calculated for each day `t`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `stockOut`.",
incorrect_msg = "Draw a barplot of `stockOut`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `I_start`.",
incorrect_msg = "Draw a barplot of `I_start`.")

test_function("barplot", args = c("height"), not_called_msg = "Draw a barplot of `I_end`.",
incorrect_msg = "Draw a barplot of `I_end`.")

test_object("mean_I_end", undefined_msg = "Make sure to define an object `mean_I_end`.",
incorrect_msg = "Something went wrong in calculating `mean_I_end`. It should be the mean ending-inventory level `I_end`.")

test_object("nStockOut", undefined_msg = "Make sure to define an object `nStockOut`.",
incorrect_msg = "Something went wrong in calculating `nStockOut`. It should be the total number of days on which a stock-out occurred. Use `sum` to calculate it.")

test_object("pStockOut", undefined_msg = "Make sure to define an object `pStockOut`.",
incorrect_msg = "Something went wrong in calculating `pStockOut`. It should be the proportion of days on which a stock-out occurred. Use `nStockOut` and the number of days in our study period to calculate it.")

success_msg("Correct! We now have a simulation model that can be used to predict inventory levels and stock-outs for any given number of days. Each time we run the simulation, we will get different levels. This is expected since the simulation model mimics a random processes. The question is then, how do we use the model to predict inventory levels and stock-outs? The answer is that we have to run the simulation numerous times, and capture our key measurement with each simulation. We can use our above code to do so, but it will become messy since it already has a `for` loop to simulate days and an `if` statement for the stock-outs.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1968bf3b5b
## A function for simulating the proportion of stock-outs

For this exercise we are going to convert our previous code into a function that can simulate the order process and inventory levels for a specified number of days and return the proportion of stock-outs, which is our main measurement of interest.

The function will take the following inputs:

* `n`: the number of days that we wish to simulate.
* `I_start0`: the starting inventory for Day 1 of our simulation.
* `P`: the number of products produced per day.

The function will then return the following:

* `pStockOut`: the proportion of days on which a stockout occurred.

*** =instructions

1. Using the provided code, write a function that simulates the `pStockOut` and takes as input `n`, `I_start0` and `P`, in that order. Call this function `inventorySimulation`. Do not provide default values for function inputs.
2. Simulate and view `pStockOut` for a 30-day study period with 150 products produced per day and with 150 products available at the start of Day 1. Assign the result of the simulation to `invSim1`.
3. Simulate and view `pStockOut` for a 30-day study period with 175 products produced per day and with 100 products available at the start of Day 1. Assign the result of the simulation to `invSim2`.
4. Simulate and view `pStockOut` for a 30-day study period with 125 products produced per day and with 400 products available at the start of Day 1. Assign the result of the simulation to `invSim3`.
5. Run the three simulations again and view the results and note how it is different from the previous simulations. Assign the result of the simulations to  `invSim1_v2`,  `invSim2_v2` and `invSim3_v2`. Hint: if you do not give the simulation output a different name, in this case `..._vs` it will overwrite the results of the previous ones and will be flagged as incorrect.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#1. Using the provided code, write a function that simulates the `pStockOut` and takes as input `n`, `I_start0` and `P`, in that order. Call this function `inventorySimulation`

inventorySimulation <- function( )
{

  O <- round(runif(n, 100, 200), 0)

  I_start <- rep(NA, n + 1)
  I_end <- rep(NA, n)
  stockOut <- rep(NA, n)

  I_start[1] <- I_start0

  for (t in 1:n)
  {
    if(I_start[t] < O[t])
    {
      # stock-out occurred, not all order were met
      stockOut[t] <- 1
      I_end[t] <- 0
    }else{
      # stock-out did not occur, all orders were met
      stockOut[t] <- 0
      I_end[t] <- I_start[t] - O[t]
    }

    # calculate the starting inventory for the next day, t + 1
    I_start[t+1] <- I_end[t] + P
  }

  nStockOut <- sum(stockOut)
  pStockOut <- nStockOut/n
}

# 2. Simulate and view `pStockOut` for a 30-day study period with 150 products produced per day and with 150 products available at the start of Day 1. Assign the result of the simulation to `invSim1`.

invSim1 <- inventorySimulation(n = , I_start0 = 150, P = 150)
invSim1

# 3. Simulate and view `pStockOut` for a 30-day study period with 175 products produced per day and with 100 products available at the start of Day 1. Assign the result of the simulation to `invSim2`.

invSim2 <- inventorySimulation(n = , I_start0 = , P = )
invSim2

# 4. Simulate and view `pStockOut` for a 30-day study period with 125 products produced per day and with 400 products available at the start of Day 1. Assign the result of the simulation to `invSim3`.

invSim3 <- inventorySimulation(n = , I_start0 = , P = )
invSim3

# 5. Run the three simulations again and view the results and note how it is different from the previous simulations. Assign the result of the simulations to  `invSim1_v2`,  `invSim2_v2` and `invSim3_v2`. Hint: if you do not give the simulation output a different name, in this case `..._vs` it will overwrite the results of the previous ones and will be flagged as incorrect.

invSim1_v2 <- inventorySimulation(n = , I_start0 = , P = )
invSim1_v2

invSim2_v2 <- inventorySimulation(n = , I_start0 = , P = )
invSim2_v2

invSim3_v2 <- inventorySimulation(n = , I_start0 = , P = )
invSim3_v2

```

*** =solution
```{r}
inventorySimulation <- function(n, I_start0, P)
{

  O <- round(runif(n, 100, 200), 0)

  I_start <- rep(NA, n + 1)
  I_end <- rep(NA, n)
  stockOut <- rep(NA, n)

  I_start[1] <- I_start0

  for (t in 1:n)
  {
    if(I_start[t] < O[t])
    {
      # stock-out occurred, not all order were met
      stockOut[t] <- 1
      I_end[t] <- 0
    }else{
      # stock-out did not occur, all orders were met
      stockOut[t] <- 0
      I_end[t] <- I_start[t] - O[t]
    }

    # calculate the starting inventory for the next day, t + 1
    I_start[t+1] <- I_end[t] + P
  }

  nStockOut <- sum(stockOut)
  pStockOut <- nStockOut/n
}

invSim1 <- inventorySimulation(n = 30, I_start0 = 150, P = 150)
invSim2 <- inventorySimulation(n = 30, I_start0 = 100, P = 175)
invSim3 <- inventorySimulation(n = 30, I_start0 = 400, P = 125)

invSim1_v2 <- inventorySimulation(n = 30, I_start0 = 150, P = 150)
invSim2_v2 <- inventorySimulation(n = 30, I_start0 = 100, P = 175)
invSim3_v2 <- inventorySimulation(n = 30, I_start0 = 400, P = 125)
```

*** =sct
```{r}
test_object("invSim1", undefined_msg = "Make sure to define an object `invSim1`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim1` and that you specified the input parameters correctly.")

test_object("invSim2", undefined_msg = "Make sure to define an object `invSim2`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim2` and that you specified the input parameters correctly.")

test_object("invSim3", undefined_msg = "Make sure to define an object `invSim3`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim3` and that you specified the input parameters correctly.")

test_object("invSim1_v2", undefined_msg = "Make sure to define an object `invSim1_v2`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim1_v2` and that you specified the input parameters correctly.")

test_object("invSim2_v2", undefined_msg = "Make sure to define an object `invSim2_v2`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim2_v2` and that you specified the input parameters correctly.")

test_object("invSim3_v2", undefined_msg = "Make sure to define an object `invSim3_v2`.",
incorrect_msg = "Something went wrong in simulating 30 days' worth of orders and calculating the proportion of days with stockouts. Make sure to assign the output of the simulation to `invSim3_v2` and that you specified the input parameters correctly.")

success_msg("Correct! By converting the simulation model into a function we can now more easily perform the simulation for different inputs. In the next question we will run the simulation model numerious times, and statistically analyse the simulation output.")
```