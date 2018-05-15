---
title: Case study 6 - Different applications of probability distributions
description: >-
  In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions. The distributions that we will look at are normal, poisson and exponential and uniform distributions. After eye-balling the applicable distribution we will find the appropriate distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.


---
## Background

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 4fdf3ba090
```

**Before continuing with this chapter, complete all previous chapters in this datacamp course.**

In this case study we will analyse data for different processes, and see whether the processes can be modelled using specific probability distribution functions.
The distributions that we will look at are:

* normal,
* poisson and exponential,  and
* uniform.

To find out more about all R's built-in distribution functions, type `help("distributions")` in the R console.

After eye-balling the applicable distribution we will find the appropriate distribution parameters, using the data, and then empirically answer some basic planning decisions using the appropriate distribution with its parameters.

We will do the following:

1. Draw a histogram of the sample data from our process.
2. See what distribution seems like a good fit for the process (eye-balling),
3. Estimate the distribution parameters using the sample data.
4. Use the R functions of the distribution to calculate percentiles and probabilities as required.

The specific R functions that are applicable to this chapter are:

```
pnorm(), qnorm(), ppois(), qpois(), pexp(), qexp(), punif(), qunif()
```

To find out more about the functions, type `?function` in the R console. The basic usage of the functions is described in [this page](http://www.cyclismo.org/tutorial/R/probability.html).

Please take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip each questions' background information. The question backgrounds contain valuable information to assist you in completing the questions.

To continue with this chapter confirm the following:

`@instructions`
* I have read **ALL** the instructions on this page carefully and have completed all the prescribed preperation material.

`@hint`





`@sct`
```{undefined}
msg_bad <- "Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."

msg_success <- "Let's get started with the Lab. Note that if you have not completed the prescribed preparation material you may not be able to complete this Chapter. Further, you will **NOT** receive any assistance from the lab lecturer and assistants on any issues covered in the preparation material."
test_mc(correct = 1, feedback_msgs = c(msg_success))
```





---
## Drill-hole sizes

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: a44acfd135
```

We wish to replace our current very old drilling machine.
The machine is required to drill a hole with a diameter of 10cm within 0.5cm tolerance.
If the hole is larger than 10.5cm, the part has to be scrapped.
Similarly, if the hole is smaller than 9.5cm the part will also be scrapped.

We also want to use the machine to drill holes for other products that have different tolerances.
One product may have a 0.25cm tolerance, another 1cm.

We have identified a possible replacement machine, and its supplier assured us that it will perform according to specifications.
We insisted that the supplier provide us with data on its performance, so that we can make a more informed decision on whether it will meet our requirements.

The supplier has given us data on different holes drilled by the machine during its testing.
The data consist of a number of drill samples, with the hole diameter for each hole measured and captured.
The data is available in the `holeSize` dataframe.

For the first part of this question, analyse the distribution of hole sizes from the machine and identify the distribution that best describes the hole-sizes from the machine.
In the following parts we will find the key parameters of the distribution and use the distribution function to answer some basic planning questions.

`@instructions`
- Analyse the distribution of the hole-sizes using the `hist()` function and by playing around with the `breaks` parameter.
- How many samples fell outside the 0.5cm tolerance limit and are therefore defective: assign your answer to `nDefective05`.
- How many samples fell outside the 0.25cm tolerance limit and are therefore defective: assign your answer to `nDefective025`.
- How many samples fell outside the 1cm tolerance limit and are therefore defective: assign your answer to `nDefective1`.
- View the values by printing them to the console output via the `script.R` file.

`@hint`
Use `hist(holeSize$holeDiameter_cm)` to analyse the distribution and see which of the four distributions

* normal,
* poisson and exponential,
* power-law, or
* uniform,

it most closely represents.

Use `nDefective <- nrow(subset(holeSize, holeDiameter_cm < lower_limit | holeDiameter_cm > upper_limit))` to find the number of effective products for different tolerance limits.

`@pre_exercise_code`
```{undefined}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```
`@sample_code`
```{undefined}
# The data are available in the holeSize dataframe.

# 1. Analyse the distribution of the hole-sizes using the `hist()` function, and by playing around with `breaks` parameter.



# 2. How many samples fell outside the 0.5cm tolerance limit and are therefore defective: assign your answer to `nDefective05`



# 3. How many samples fell outside the 0.25cm tolerance limit and are therefore defective: assign your answer to `nDefective025`



# 4. How many samples fell outside the 1cm tolerance limit and are therefore defective: assign your answer to `nDefective1`



# 5. View the values by printing them to the console output via the `script.R` file. Print the values here:
```
`@solution`
```{undefined}
hist(holeSize$holeDiameter_cm)
nDefective05 <- nrow(subset(holeSize, holeDiameter_cm > 10.5 | holeDiameter_cm < 9.5 ))
nDefective025 <- nrow(subset(holeSize, holeDiameter_cm > 10.25 | holeDiameter_cm < 9.75 ))
nDefective1 <- nrow(subset(holeSize, holeDiameter_cm < 9 | holeDiameter_cm > 11 ))
nDefective05
nDefective025
nDefective1
```
`@sct`
```{undefined}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the hole diameters of the samples to see what the distribution looks like.",
              incorrect_msg = "Make sure to draw the histogram for the hole diameters. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")

test_object("nDefective05", undefined_msg = "Make sure to define an object `nDefective05`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 0.5cm tolerance and assigned the result to `nDefective05`")     

test_object("nDefective025", undefined_msg = "Make sure to define an object `nDefective025`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 0.25cm tolerance and assigned the result to `nDefective025`")

test_object("nDefective1", undefined_msg = "Make sure to define an object `nDefective1`.",
            incorrect_msg = "Make sure that you calculated the number of holes that are outside the 1cm tolerance and assigned the result to `nDefective1`")

test_output_contains("nDefective05", incorrect_msg = "You did not view the actual value of  `nDefective05`.")
test_output_contains("nDefective025", incorrect_msg = "You did not view the actual value of  `nDefective025`.")
test_output_contains("nDefective1", incorrect_msg = "You did not view the actual value of  `nDefective1`.")

success_msg("Correct! Now that we looked at the distribution of hole-sizes, and have an idea of the number of products that are not according to specification, we can decide which distribution best represent the hole-sizes resulting from the drilling machine.")
```





---
## Choosing the best distribution for the drill-hole sizes

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 8eaf7b9ebd
```

Based on the histogram of the drill-hole sizes, which of the following distributions best represent this random variable?

`@instructions`
* normal,
* poisson and exponential,
* power-law, or
* uniform.

`@hint`
Have a look at the distribution that you generated in the previous exercise.




`@sct`
```{undefined}
msg_bad <- "Incorrect. You may need to redo the previous exercise. The data is randomly generated, so there may be chance that the distribution looks like something else by accident."
msg_success <- "That is correct! The distribution that best describes the resulting hole-size is the normal distribution. Next we need to find the key parameters for the distribution."
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad, msg_bad))
```





---
## Drill-hole size distribution parameters.

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: f49344f14e
```

Which distribution parameters do we need to determine to model the hole-size as a normal distribution?

`@instructions`
* min and max,
* mean or arrival rate, or
* mean and standard deviation.

`@hint`
Have a look at the class slides available from [this link](https://clickup.up.ac.za/bbcswebdav/pid-1016180-dt-content-rid-11418452_1/courses/ban313_s1_2017/Lecture_Week6.pdf).




`@sct`
```{undefined}
msg_bad <- "Incorrect. That is not the necessary parameters for the normal distribution."
msg_success <- "That is correct! For a normal distribution we need the mean and standard deviation of the random variable"
test_mc(correct = 3, feedback_msgs = c(msg_bad, msg_bad, msg_success))
```





---
## Using the best distribution for the drill-hole sizes

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 54412012be
```

To use the distribution, $X \sim \mathcal{N}(\mu, \sigma)$ we first need to estimate its key parameters, namely the mean, $\mu$, and standard deviation, $\sigma$, of the drill-hole sizes.
Thereafter we will empirically calculate the fraction of products that meet different specification limits using the `pnorm()` function.

The data that we will use to estimate the key parameters are available in the `holeSize` dataframe. Visit [this page](http://www.cyclismo.org/tutorial/R/probability.html) to find out more about using the `pnorm` function.

To assist in answering the question, on a piece of paper draw a figure of the normal distribution of the hole-size and check where the lower and upper tolerance limits lie. Pay special attention that the tolerance limits may not necessarily be symmetrically around the mean, in which case you need to calculate the probability for each tolerance limit, and calculate the area between the upper and lowered limit. This is covered in Example 2.49 on page 93 in the [prescribed textbook](https://www.openintro.org/stat/textbook.php?stat_book=isrs).

`@instructions`
- Estimate $\mu$ for the normal distribution and assign your answer to `meanHoleSize`.
- Estimate $\sigma$ for the normal distribution and assign your answer to `sdHoleSize`.
- Use the normal distribution probability function and determine the fraction of holes that will fall within a _0.5cm_ tolerance limit and assign your answer to `fWork05`.
- Use the normal distribution probability function and determine the fraction of holes that will fall within a _0.75cm_ tolerance limit and assign your answer to `fWork075`.
- View all the above values by printing them to the console output via the `script.R` file.

`@hint`
To calculate $\mu$ and $\sigma$, simply use `mean` and `sd` on `holeSize$holeDiameter_cm`.

Visit [this page](http://www.cyclismo.org/tutorial/R/probability.html) to find out more about using the `pnorm` function.

The `pnorm` function has three inputs, `q`, $\mu$ and $\sigma$. To calculate the fraction of products that fall within a lower and upper limit, you need to call `pnorm` twice: once by setting `q = lower_limit` and once by setting `q = upper_limit`. The function gives the probability of getting a value lower than `q`, so you have to then figure you which probability value to subtract from which to get the area between two limits. This is covered in Example 2.49 on page 93 in the [prescribed textbook](https://www.openintro.org/stat/textbook.php?stat_book=isrs).

`@pre_exercise_code`
```{undefined}
n <- runif(1, 300, 400)
holeSize <- data.frame(sampleNumber = c(1:n), holeDiameter_cm = round(rnorm(n, 10.1, 0.35), 2))
rm(n)
```
`@sample_code`
```{undefined}
# The data are available in the holeSize dataframe.

# 1. Estimate mu for the normal distribution and assign your answer to `meanHoleSize`.



# 2. Estimate sigma for the normal distribution and assign your answer to `sdHoleSize`.



# 3. Use the normal distribution probability function and determine the fraction of holes that will fall within a 0.5cm tolerance limit and assign your answer to `fWork05`.



# 4. Use the normal distribution probability function and determine the fraction of holes that will fall within a 0.75cm tolerance limit and assign your answer to `fWork075`.



# 5. View all the above values by printing them to the console output via the `script.R` file.  Print the values here:
```
`@solution`
```{undefined}
meanHoleSize <- mean(holeSize$holeDiameter_cm)
sdHoleSize <- sd(holeSize$holeDiameter_cm)
fWork05 <- pnorm(10+0.5, mean = meanHoleSize, sd = sdHoleSize) - pnorm(10-0.5, mean = meanHoleSize, sd = sdHoleSize)
fWork075 <- pnorm(10+0.75, mean = meanHoleSize, sd = sdHoleSize) - pnorm(10-0.75, mean = meanHoleSize, sd = sdHoleSize)
meanHoleSize
sdHoleSize
fWork05
fWork075
```
`@sct`
```{undefined}
test_object("meanHoleSize", undefined_msg = "Make sure to define an object `meanHoleSize`.",
            incorrect_msg = "Make sure that you calculated the mean hole size correctly and assigned your answer to `meanHoleSize`")     

test_object("sdHoleSize", undefined_msg = "Make sure to define an object `sdHoleSize`.",
            incorrect_msg = "Make sure that you calculated the standard deviation of hole size correctly and assigned your answer to `sdHoleSize`")     

test_object("fWork05", undefined_msg = "Make sure to define an object `fWork05`.",
            incorrect_msg = "Make sure that you calculated the fraction of holes that are _within_ the 0.5cm tolerance correctly and assigned the result to `fWork05`")

test_object("fWork075", undefined_msg = "Make sure to define an object `fWork075`.",
            incorrect_msg = "Make sure that you calculated the fraction of holes that are _within_ the 0.75cm tolerance correctly and assigned the result to `fWork075`")

test_output_contains("meanHoleSize", incorrect_msg = "You did not view the actual value of `meanHoleSize`.")
test_output_contains("sdHoleSize", incorrect_msg = "You did not view the actual value of `sdHoleSize`.")
test_output_contains("fWork05", incorrect_msg = "You did not view the actual value of `fWork05`.")
test_output_contains("fWork075", incorrect_msg = "You did not view the actual value of `fWork075`.")

success_msg("Correct! We have successfully fitted a normal distribution to the hole size variable and used the distribution to empirically calculate the fraction of products that are within specification.")
```





---
## Predicting soft-drink orders for the next day

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: acbbc34771
```

We are responsible for the inventory management for a soft-drink company.
Due to a strike, the company cannot produce soft-drinks.
The company is afraid that it may not be able to fulfil customer orders for the next day.
Human resources believe that the strike will end this evening, and we therefore need to determine if we have enough inventory for tomorrow's orders.
Orders are only placed by our clients in the morning and delivered on the same day.
Our clients are unaware of the strike, and management has no intention of informing them of it.
We currently have 210 units of the soft-drink in stock.

Management wishes to know the probability of running out of stock during the next day.
Depending on the probability they can either import soft-drinks from another manufacturing plant via expensive air-delivery, or decide to take their chances and wait for the strike to end and resume manufacturing.

The sales data for the previous 30 days' orders are available in the `clienOrders` dataframe, which consists of the day and the number of units ordered on that day by all our clients.

For the first part of this question, analyse the distribution of the number of units ordered per day and identify the distribution that best describes this random variable.
In the following parts we will find the key parameters of the distribution and use the distribution function to answer the above question.

`@instructions`
- Analyse the distribution of the number of units ordered per day using the `hist()` function and by playing around with `breaks` parameter.
- How many times over the last 30 days did customers order _less_ than 210 units: assign your answer to `less210`.
- View the values by printing them to the console output via the `script.R` file.

`@hint`
Use `hist(clienOrders$ordersPerDay)` to analyse the distribution and see which of the four distributions

- normal,
- poisson and exponential,
- power-law, or
- uniform,

that it most closely represents.

Use `less210 <- nrow(subset(clienOrders, ordersPerDay < lower_limit))` to find the number of days that less than the specified number of products were ordered.

`@pre_exercise_code`
```{undefined}
clienOrders <- data.frame(day = 1:30, ordersPerDay = round(runif(30, 150, 250), 0))
```
`@sample_code`
```{undefined}
# The data are available in the clienOrders dataframe.

# 1. Analyse the distribution of the number of units ordered per day using the `hist()` function and by playing around with `breaks` parameter.



# 2. How many times over the last 30 days did customers order _less_ than 210 units: assign your answer to `less210`.



# 3. View the values by printing them to the console output via the `script.R` file. Print the values here:
```
`@solution`
```{undefined}
hist(clienOrders$ordersPerDay)
less210 <- nrow(subset(clienOrders, ordersPerDay < 210))
less210
```
`@sct`
```{undefined}
test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the number of units ordered per day to see what the distribution looks like.",
              incorrect_msg = "Make sure to draw the histogram for the number of units ordered per day. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")

test_object("less210", undefined_msg = "Make sure to define an object `less210`.",
            incorrect_msg = "Make sure that you calculated the number of times that clients ordered less than 210 units and assigned the result to `less210`")     

test_output_contains("less210", incorrect_msg = "You did not view the actual value `less210`.")

success_msg("Correct! Now that we looked at the distribution of the number of units ordered per day, and have an idea of the number of times that our clients ordered less units than what is in our inventory, we can decide which distribution best represents the number of units ordered per day and predict the probability of running out of stock tomorrow.")
```





---
## Choosing the best distribution for soft-drink orders

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 83144b0372
```

Based on the histogram of the number of units ordered per day, which of the following distributions best represent the variable?

`@instructions`
* normal,
* poisson and exponential,
* power-law, or
* uniform.

`@hint`
Have a look at the distribution that you generated in the previous exercise.




`@sct`
```{undefined}
msg_bad <- "Incorrect. You may need to redo the previous exercise. The data is randomly generated, so there may be chance that the distribution looks like something else by accident."
msg_success <- "That is correct! The distribution that best describes the number of units ordered per day is the uniform distribution. Next we need to find the key parameters for the distribution."
test_mc(correct = 4, feedback_msgs = c(msg_bad, msg_bad, msg_bad, msg_success))
```





---
## Soft-drink orders distribution parameters.

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 4085ce9982
```

Which distribution parameters do we need to determine to model the number of units ordered per day as a uniform distribution?

`@instructions`
* min and max,
* mean or arrival rate, or
* mean and standard deviation.

`@hint`
Have a look at the class slides available from [this link](https://clickup.up.ac.za/bbcswebdav/pid-1016180-dt-content-rid-11418452_1/courses/ban313_s1_2017/Lecture_Week6.pdf).




`@sct`
```{undefined}
msg_bad <- "Incorrect. That is not the necessary parameters for the uniform distribution."
msg_success <- "That is correct! For a uniform distribution we need the minimum and maximum value for the random variable"
test_mc(correct = 1, feedback_msgs = c(msg_success, msg_bad, msg_bad))
```





---
## Using the best distribution for soft-drink orders

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 91698a409b
```

To use the distribution, $X \sim \mathcal{U}(\min, \max)$ we first need to estimate its key parameters, namely the minimum and maximum value for the number of units ordered per day.
Thereafter we will empirically calculate the fraction of days in which more than 210 units will be ordered using the `punif()` function.
This represents the probability that we will run out of stock on the next day.

The data that we will use to estimate the key parameters are available in the `clienOrders` dataframe.

`@instructions`
- Estimate $\min$ for the uniform distribution and assign your answer to `minOrders`.
- Estimate $\max$ for the uniform distribution and assign your answer to `maxOrders`.
- Use the uniform distribution probability function and determine the probability that more than 210 units will be ordered to tomorrow and assign your answer to `pStockOut`.
- View all the above values by printing them to the console output via the `script.R` file.

`@hint`
To calculate $\min_{x}$ and $\max_{x}$, simply use `min` and `max` on `clienOrders$ordersPerDay`.

The `punif` function has three inputs, `q`, `min` and `max`. Remember to check whether it returns the probability of having a value less than `q` or more than `q`.

`@pre_exercise_code`
```{undefined}
clienOrders <- data.frame(day = 1:30, ordersPerDay = round(runif(30, 150, 250), 0))
```
`@sample_code`
```{undefined}
# The data are available in the clienOrders dataframe.

# 1. Estimate min_x for the uniform distribution and assign your answer to `minOrders`.



# 2. Estimate max_x for the normal distribution and assign your answer to `maxOrders`.



# 3. Use the uniform distribution probability function and determine the probability that more than 210 units will be ordered tomorrow and assign your answer to `pStockOut`.



# 4. View all the above values by printing them to the console output via the `script.R` file.  Print the values here:
```
`@solution`
```{undefined}
minOrders <- min(clienOrders$ordersPerDay)
maxOrders <- max(clienOrders$ordersPerDay)
pStockOut <- punif(q = 210, min = minOrders, max = maxOrders, lower.tail = FALSE)
minOrders
maxOrders
pStockOut
```
`@sct`
```{undefined}
test_object("minOrders", undefined_msg = "Make sure to define an object `minOrders`.",
            incorrect_msg = "Make sure that you calculated the minimum number of units ordered per day correctly and assigned your answer to `minOrders`")     

test_object("maxOrders", undefined_msg = "Make sure to define an object `maxOrders`.",
            incorrect_msg = "Make sure that you calculated the maximum number of units ordered per day correctly and assigned your answer to `maxOrders`")       

test_object("pStockOut", undefined_msg = "Make sure to define an object `pStockOut`.",
            incorrect_msg = "Make sure that you calculated the probability of stockout correctly and assigned your answer to `pStockOut`")

test_output_contains("minOrders", incorrect_msg = "You did not view the actual value `minOrders`.")
test_output_contains("maxOrders", incorrect_msg = "You did not view the actual value `maxOrders`.")
test_output_contains("pStockOut", incorrect_msg = "You did not view the actual value `pStockOut`.")

success_msg("Correct! We have successfully fitted a uniform distribution to the number of units ordered per day and used the distribution to empirically calculate the probability of a stockout.")
```





---
## Gautrain arrival rate

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 8baad0d53b
```

We wish to determine the number of passengers arriving _per minute_ from the Gautrain between 16:00 and 18:00PM.
This information is necessary to determine the number of outbound access gates to activate during the peak-arrival time.
The Gautrain can easily switch  an access gate from inbound to outbound.
Approximately _two_ passengers can go through the gate per minute, and the station is considering having only _two_ outbound access-gates open during the peak-time.
The question that we need to answer is what is the probability of the two outbound access-gates being insufficient?
Insufficient means that the number of passengers arriving during the minute will be more than the access gates can process.

Data on the arrival time of passengers between 16:00 and 18:00PM for the previous 30 working days can be found in the `gauArrive` dataframe.

The columns (variables) of the dataset and the variable types are as follow:

* `day`: day when the arrival was captured, ranging from \{1, 2, \ldots, 30\}.
* `dow`: the day of the week when the arrival was captured, and limited to \{`Monday`, `Tuesday`, `Wednesday`, `Thursday`, `Friday`\}.
* `hour`: the hour of the day during which the passenger arrived, from \{16, 17}, where 16 represents 16:00PM--17:00PM and 17 represents 17:00PM--18:00PM,.
* `minute`: the minute of the hour during which the passenger arrived, from \{00, 01, \ldots, 59\}, where 00 represents the time from 16:00PM--16:01PM or 17:00PM--18:01PM. Note that the time is reset at the start of each hour.
* `second`: the second of the minute during which the passenger arrived, from from \{00, 01, \ldots, 59\}.
* `timeStamp`: exact time of the passenger arrival, in the format `hh:mm:ss`.
* `cardLoad`: whether the passenger had to load money on his or her card using the terminal-teller, which is either `TRUE` if the loaded money, or `FALSE` if they did not.
* `origin`: the station where the passenger first got on the train, and limited to \{`Pretoria`, `Centurion`, `Midrand`, `Marlboro`, `Sandton`, `Rosebank`, `Park`, `Rhodesfield`, `OR-Tambo`\}

For the first part of this question, analyse the distribution of the number of passengers arriving per minute and identify the distribution that best describes this variable.
In the following parts we will find the key parameters of the distribution and use the distribution function to answer some basic planning support questions.

`@instructions`
- Determine the number of customers that arrived within each minute interval and assign your answer to `nArrivePerMin`. Note that you have to calculate the number of customers that arrived during each minute interval in the dataset. This can be done using the `table` command, but take care to calculate it over each unique day, hour and minute combination, otherwise the command will sum the number of arrivals within the minute interval over different hours and days. To do so use `table(factor1, factor2, factor3)`. You do not have to use subset.
- Analyse the distribution of the number of passengers arriving per minute using the `hist()` function and by playing around with `breaks` parameter.

`@hint`
First calculate the number of customers that arrived within each minute interval using the `table()` command.
Remember to call the command over the `day`, `hour` and `minute` factors:  `table(dataframe$factor1, dataframe$factor2, dataframe$factor3)`.
The resulting table can be directly plotted using `hist(nArrivePerMin)`.

Use the histogram to analyse the distribution and see which of the four distributions

* normal,
* poisson and exponential,
* power-law, or
uniform,

best describe the variable.

`@pre_exercise_code`
```{undefined}
tod <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(todFormatted)
}

tomns <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(mints)
}

tosecs <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(secs)
}

genData <- function()
{
  nCustomersPerWeek <- 63000/8*5
  dayPeak <- c(2,2,2,1,1)
  dayPeakNorm <- dayPeak/sum(dayPeak)
  arrivalPeak <- c(1, 1, 2, 4, 2, 1, 1, 1, 1, 2, 4, 6, 6, 6, 4, 2, 1, 1)
  arrivalPeakNorm <- arrivalPeak/sum(arrivalPeak)

  pCardLoad <- c(0.05, 0.05, 0.025, 0.025, 0.025)

  dows <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
  dayHrs <- 16:17
  dayMinutes <- 1:60
  stations <- c('Pretoria', 'Centurion', 'Midrand', 'Marlboro', 'Sandton', 'Rosebank',
                'Park', 'Rhodesfield', 'OR-Tambo')

  mydataTemplate <- data.frame(day = numeric(),
                               dow = character(),
                               hour = numeric(),
                               timeStamp = character(),
                               cardLoad = logical(),
                               origin = character())

  mydata <- mydataTemplate

  i = 0
  for (nWeek in 1:(30/length(dows)))
  {
    for (d in 1:length(dows))
    {
      i = i + 1
      nDay = nCustomersPerWeek*dayPeakNorm[d]
      for (h in 1:length(dayHrs))
      {
        arrivalRatePerHour <- nDay*arrivalPeakNorm[h]
        startTime <- 0
        arrivalTime <- cumsum(rexp(arrivalRatePerHour*2, rate = arrivalRatePerHour)*60*60)
        timeStamp <- tod(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        nArrivals <- length(timeStamp)

        day <- rep(i, nArrivals)
        dow <- rep(dows[d], nArrivals)
        hour <- rep(dayHrs[h], nArrivals)
        minute <- tomns(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        second <- tosecs(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        cardLoad <- runif(nArrivals, 0, 1) < pCardLoad[d]
        origin <- sample(stations, size = nArrivals, replace = T)
        hourFrame <- data.frame(day, dow, hour, minute, second, timeStamp, cardLoad, origin)
        mydata <- rbind(mydata, hourFrame)
      }
    }
  }
  return(mydata)
}

gauArrive <- genData()
rm(genData)
rm(tod)
rm(tomns)
rm(tosecs)
```
`@sample_code`
```{undefined}
# The data are available in the gauArrive dataframe.

# 1. Determine the number of customers that arrived within each minute interval and assign your answer to `nArrivePerMin`. Note that you have to calculate the number of customers that arrived during each minute interval in the dataset. This can be done using the `table` command, but take care to calculate it over each unique day, hour and minute combination, otherwise the command will sum the number of arrivals within the minute interval over different hours and days. To do so use `table(factor1, factor2, factor3)`. You do not have to use subset.



# 2. Analyse the distribution of the number of passengers arriving per minute using the `hist()` function and by playing around with `breaks` parameter.
```
`@solution`
```{undefined}
nArrivePerMin <- table(gauArrive$day, gauArrive$hour, gauArrive$minute)
hist(nArrivePerMin)
```
`@sct`
```{undefined}
test_object("nArrivePerMin", undefined_msg = "Make sure to define an object `nArrivePerMin`.",
            incorrect_msg = "Make sure that you calculated the number of customers that arrived during each minute interval and assigned your answer to `nArrivePerMin`")     

test_function("hist", args = c("x"), not_called_msg = "Draw a histogram of the number of customers that arrived per minute to see what the distribution looks like.",
              incorrect_msg = "Make sure to draw the histogram for the number of customers that arrived per minute. If you used the `breaks` command, it's good that you played around with it, but to go past this answer you have to call `hist()` without it.")

success_msg("Correct! Now that we looked at the distribution of the number of customers that arrive per minute, we can decide which distribution best represent this variable.")
```





---
## Choosing the best distribution for passenger arrivals

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 1c8e286d4a
```

Based on the histogram of the number of passengers arriving per minute, which of the following distributions best represent this random variable?

`@instructions`
* normal,
* poisson and exponential,
* power-law, or
* uniform.

`@hint`
Have a look at the distribution that you generated in the previous exercise.




`@sct`
```{undefined}
msg_bad <- "Incorrect. You may need to redo the previous exercise and increase or reduce the number of breaks."
msg_success <- "That is correct! The distribution that best describes the number of passenger arrivals per minute is the poisson or exponential distribution. Next we need to find the key parameters for the distribution."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad, msg_bad))
```





---
## Passenger arrival distribution parameter.

```yaml
type: MultipleChoiceExercise
lang: r
xp: 50
skills: 1
key: 13682c55f6
```

Which distribution parameter(s) do we need to determine to model the number of passenger arrivals?

`@instructions`
* min and max,
* mean or arrival rate, or
* mean and standard deviation.

`@hint`
Have a look at the class slides available from [this link](https://clickup.up.ac.za/bbcswebdav/pid-1016180-dt-content-rid-11418452_1/courses/ban313_s1_2017/Lecture_Week6.pdf).




`@sct`
```{undefined}
msg_bad <- "Incorrect. That is not the necessary parameter for the poisson distribution."
msg_success <- "That is correct! For a poisson distribution we need the arrival rate of the random variable."
test_mc(correct = 2, feedback_msgs = c(msg_bad, msg_success, msg_bad))
```





---
## Using the best distribution for passenger arrivals

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 97f9e17aa1
```

To use the distribution, $X \sim \text{Poisson}(\lambda)$, we first need to estimate its key parameter, namely the expected number of customers that arrive per minute, $\lambda$.
Thereafter we will empirically calculate the probability of the two outbound access-gates being insufficient during any minute interval using the `ppois()` function.
Recall that the station is considering installing _two_ outbound access-gates and each gate can process approximately _two_ passengers per minute.
Insufficient means that the number of passengers arriving during the minute will be more than the access gates can process.
For now we will ignore the knock-on effect that insufficient or excess resources during a time-interval will have on the following time-interval.

The data that we will use to estimate the key parameters are available in the `gauArrive` dataframe. The number of customers that arrived within each minute interval has been pre-calculated and is available as the `nArrivePerMin` object.

`@instructions`
- Estimate $\lambda$ for the poisson distribution and assign your answer to `arrivalRate`.
- Use the poisson distribution probability function and determine the probability that the two outbound access-gates will be insufficient during any minute interval and assign your answer to `pInsufficent`.
- View the above values by printing them to the console output via the `script.R` file.

`@hint`
To calculate $arrivalRate$ simply calculate the mean number of arrivals over all the minute-intervals.

To calculate the probability of the two-access gates being insufficient we first need to establish the maximum number of customers that two gates can cope with within a minute, which in this case is $2 \times 2 = 4$ and then calculate the probability of more than 4 customers arriving during a minute interval.

`@pre_exercise_code`
```{undefined}
tod <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(todFormatted)
}

tomns <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(mints)
}

tosecs <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(secs)
}

genData <- function()
{
  nCustomersPerWeek <- 63000/8*5
  dayPeak <- c(2,2,2,1,1)
  dayPeakNorm <- dayPeak/sum(dayPeak)
  arrivalPeak <- c(1, 1, 2, 4, 2, 1, 1, 1, 1, 2, 4, 6, 6, 6, 4, 2, 1, 1)
  arrivalPeakNorm <- arrivalPeak/sum(arrivalPeak)

  pCardLoad <- c(0.05, 0.05, 0.025, 0.025, 0.025)

  dows <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
  dayHrs <- 16:17
  dayMinutes <- 1:60
  stations <- c('Pretoria', 'Centurion', 'Midrand', 'Marlboro', 'Sandton', 'Rosebank',
                'Park', 'Rhodesfield', 'OR-Tambo')

  mydataTemplate <- data.frame(day = numeric(),
                               dow = character(),
                               hour = numeric(),
                               timeStamp = character(),
                               cardLoad = logical(),
                               origin = character())

  mydata <- mydataTemplate

  i = 0
  for (nWeek in 1:(30/length(dows)))
  {
    for (d in 1:length(dows))
    {
      i = i + 1
      nDay = nCustomersPerWeek*dayPeakNorm[d]
      for (h in 1:length(dayHrs))
      {
        arrivalRatePerHour <- nDay*arrivalPeakNorm[h]
        startTime <- 0
        arrivalTime <- cumsum(rexp(arrivalRatePerHour*2, rate = arrivalRatePerHour)*60*60)
        timeStamp <- tod(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        nArrivals <- length(timeStamp)

        day <- rep(i, nArrivals)
        dow <- rep(dows[d], nArrivals)
        hour <- rep(dayHrs[h], nArrivals)
        minute <- tomns(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        second <- tosecs(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        cardLoad <- runif(nArrivals, 0, 1) < pCardLoad[d]
        origin <- sample(stations, size = nArrivals, replace = T)
        hourFrame <- data.frame(day, dow, hour, minute, second, timeStamp, cardLoad, origin)
        mydata <- rbind(mydata, hourFrame)
      }
    }
  }
  return(mydata)
}

gauArrive <- genData()
rm(genData)
rm(tod)
rm(tomns)
rm(tosecs)

nArrivePerMin <- table(gauArrive$day, gauArrive$hour, gauArrive$minute)
```
`@sample_code`
```{undefined}
# The number of customers that arrived within each minute interval has been pre-calculated and is available as the `nArrivePerMin` object.

# 1. Estimate lambda for the poisson distribution and assign your answer to `arrivalRate`.



# 2. Use the poisson distribution probability function and determine the probability that the two outbound access-gates will be insufficient during any minute interval and assign your answer to `pInsufficent`.



# 3. View the above values by printing them to the console output via the `script.R` file.
```
`@solution`
```{undefined}
arrivalRate <- mean(nArrivePerMin)
pInsufficent <- ppois(2*2, arrivalRate, lower.tail = FALSE)
arrivalRate
pInsufficent
```
`@sct`
```{undefined}
test_object("arrivalRate", undefined_msg = "Make sure to define an object `arrivalRate`.",
            incorrect_msg = "Make sure that you calculated lambda correctly and assigned your answer to `arrivalRate`")     

test_object("pInsufficent", undefined_msg = "Make sure to define an object `pInsufficent`.",
            incorrect_msg = "Make sure that you calculated the probability of the two gates being _insufficient_, where each gate can process two customers per minute, correctly and assign your answer to `pInsufficent`")

test_output_contains("arrivalRate", incorrect_msg = "You did not view the actual value `arrivalRate`.")
test_output_contains("pInsufficent", incorrect_msg = "You did not view the actual value `pInsufficent`.")

success_msg("Correct! We have now successfully fitted a poisson distribution to number of customers that arrive within a one-minute interval and used the distribution to empirically calculate the probability of two access-gates being insufficient.")
```





---
## Using the best distribution to determine the number of access gates

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 3c7b8033f9
```

The station wants the probability of their access-gates being insufficient to be less than 0.05.
Recall that a gate can process approximately 2 passengers per minute.
What is the minimum number of access-gates required to ensure that this is the case?

The number of customers that arrived within each minute interval has again been pre-calculated and is available as the `nArrivePerMin` object.

`@instructions`
- Determine the minimum number of access-gates required to ensure that the probability of the gates being insufficient is less than 0.05. For this question, use the `qpois` function. Assign your answer to `minGates`.
- View the above value by printing it to the console output via the `script.R` file.

`@hint`
use the `qnorm` function to determine the 95th percentile point, which represents the number of passenger arrivals, or more, that will occur with a probability of 0.05.
Thereafter use this number to determine the minimum number of access-gates.

`@pre_exercise_code`
```{undefined}
tod <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(todFormatted)
}

tomns <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(mints)
}

tosecs <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(secs)
}

genData <- function()
{
  nCustomersPerWeek <- 63000/8*5
  dayPeak <- c(2,2,2,1,1)
  dayPeakNorm <- dayPeak/sum(dayPeak)
  arrivalPeak <- c(1, 1, 2, 4, 2, 1, 1, 1, 1, 2, 4, 6, 6, 6, 4, 2, 1, 1)
  arrivalPeakNorm <- arrivalPeak/sum(arrivalPeak)

  pCardLoad <- c(0.05, 0.05, 0.025, 0.025, 0.025)

  dows <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
  dayHrs <- 16:17
  dayMinutes <- 1:60
  stations <- c('Pretoria', 'Centurion', 'Midrand', 'Marlboro', 'Sandton', 'Rosebank',
                'Park', 'Rhodesfield', 'OR-Tambo')

  mydataTemplate <- data.frame(day = numeric(),
                               dow = character(),
                               hour = numeric(),
                               timeStamp = character(),
                               cardLoad = logical(),
                               origin = character())

  mydata <- mydataTemplate

  i = 0
  for (nWeek in 1:(30/length(dows)))
  {
    for (d in 1:length(dows))
    {
      i = i + 1
      nDay = nCustomersPerWeek*dayPeakNorm[d]
      for (h in 1:length(dayHrs))
      {
        arrivalRatePerHour <- nDay*arrivalPeakNorm[h]
        startTime <- 0
        arrivalTime <- cumsum(rexp(arrivalRatePerHour*2, rate = arrivalRatePerHour)*60*60)
        timeStamp <- tod(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        nArrivals <- length(timeStamp)

        day <- rep(i, nArrivals)
        dow <- rep(dows[d], nArrivals)
        hour <- rep(dayHrs[h], nArrivals)
        minute <- tomns(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        second <- tosecs(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        cardLoad <- runif(nArrivals, 0, 1) < pCardLoad[d]
        origin <- sample(stations, size = nArrivals, replace = T)
        hourFrame <- data.frame(day, dow, hour, minute, second, timeStamp, cardLoad, origin)
        mydata <- rbind(mydata, hourFrame)
      }
    }
  }
  return(mydata)
}

gauArrive <- genData()
rm(genData)
rm(tod)
rm(tomns)
rm(tosecs)

nArrivePerMin <- table(gauArrive$day, gauArrive$hour, gauArrive$minute)
```
`@sample_code`
```{undefined}
# The number of customers that arrived within each minute interval has been pre-calculated and is available as the `nArrivePerMin` object.

#1. Determine the minimum number of access-gates required to ensure that the probability of the gates be insufficient is less than 0.05. For this question, use the `qpois` function. Assign your answer to `minGates`



#2. View the above value by printing it to the console output via the `script.R` file.
```
`@solution`
```{undefined}
arrivalRate <- mean(nArrivePerMin)
qInsufficent <- qpois(0.05, arrivalRate, lower.tail = FALSE)
minGates <- ceiling(qInsufficent/2)
minGates
```
`@sct`
```{undefined}
test_object("minGates", undefined_msg = "Make sure to define an object `minGates`.",
            incorrect_msg = "Make sure that you calculated the minimum number of access-gates correctly and assigned your answer to `minGates`")     

test_output_contains("minGates", incorrect_msg = "You did not view the actual value `minGates`.")

success_msg("Correct! We have now successfully used a poisson distribution to calculate the minimum number of access-gates that will ensure that the probability of the gates being insufficient for customer arrivals is less than 0.05.")
```





---
## Using the best distribution for passenger inter-arrival times

```yaml
type: NormalExercise
lang: r
xp: 100
skills: 1
key: 957fe3a04a
```

In this question we are again going to focus on passenger-arrivals at the Gautrain, but this time we will analyse the inter-arrival times of customers. That is the time between the arrivals of two consecutive passengers.

To do so we first need to calculate the inter-arrival time, in minutes, between all consecutive passengers.

Recall that the data frame has the following variables:

* `day`: day when the arrival was captured, ranging from \{1, 2, \ldots, 30\}.
* `dow`: the day of the week when the arrival was captured, and limited to \{`Monday`, `Tuesday`, `Wednesday`, `Thursday`, `Friday`\}.
* `hour`: the hour of the day during which the passenger arrived, from \{16, 17}, where 16 represents 16:00PM--17:00PM and 17 represents 17:00PM--18:00PM,.
* `minute`: the minute of the hour during which the passenger arrived, from \{00, 01, \ldots, 59\}, where 00 represents the time from 16:00PM--16:01PM or 17:00PM--18:01PM. Note that the time is reset at the start of each hour.
* `second`: the second of the minute during which the passenger arrived, from from \{00, 01, \ldots, 59\}.
* `timeStamp`: exact time of the passenger arrival, in the format `hh:mm:ss`.
* `cardLoad`: whether the passenger had to load money on his or her card using the terminal-teller, which is either `TRUE` if the loaded money, or `FALSE` if they did not.
* `origin`: the station where the passenger first got on the train, and limited to \{`Pretoria`, `Centurion`, `Midrand`, `Marlboro`, `Sandton`, `Rosebank`, `Park`, `Rhodesfield`, `OR-Tambo`\}

To calculate the inter-arrival time we first need to calculate the minute since the start of the day at which each customer arrived. This can be done using the following formula:

  ```
minuteArrived <- hour*60 + minute + second/60
```

Thereafter we need to calculate the time between arrivals, taking care _not_ to include the time between the first arrival of a day and the last arrival of the previous day. With the above formula, these inter-arrival times should be ignored since they will give negative inter arrival times. For example, on day 1 the last customer arrived at 1079.167 minutes since the start of that day and on day 2 the first customer arrived at 960.16 minutes since the start of that day. The inter-arrival time between these two consecutive customers will be negative. We can use this information and the `subset` function to remove inter arrival times that are negative. Note that an inter-arrival time of zero is possible and should be included in the data.

To actually calculate the inter-arrival times we can use the `diff` function. Type `?diff()` in the console or visit [this page](http://stackoverflow.com/questions/8404611/how-to-find-the-difference-in-value-in-every-two-consecutive-rows-in-r) to see how it can be used.

Once we have the inter-arrival time between consecutive visits we can use the distribution, $X \sim \text{Exp}(\lambda)$, but we first need to estimate its key parameter, namely the expected number of customers that arrive per minute, $\lambda$, which is $\frac{1}{\text{mean-intertime}}$.

Thereafter we can empirically calculate the probability of the next passenger arriving within 0.5 minutes after the current one arrived. To do so we can use the `pexp` function.

`@instructions`
1. Calculate the minutes since the start of day that each customer arrived at and assign your answer to the vector `minuteArrived`.
2. Calculate the inter-arrival time between consecutive arrivals and assign the result to the vector `interArrive`.
3. Remove all negative inter arrival times from `interArrive` and assign the result to `interArriveClean`.
4. Calculate the arrival rate for the exponential distribution using `interArriveClean` and assign your answer to `arriveRate`.
5. Calculate the probability of a customer arriving within 0.5 minutes (30 seconds) after the current one arrived and assign your answer to `pWithin30sec`.
6. View the above values by printing them to the console output via the `script.R` file.

`@hint`
TBC

`@pre_exercise_code`
```{undefined}
tod <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(todFormatted)
}

tomns <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(mints)
}

tosecs <- function(x, h)
{
  mints = floor(x/60)
  secs = round(x - 60*mints, 2)
  hrs = formatC(h, width=2,format='f',digits=0,flag='0')
  mns = formatC(mints, width=2,format='f',digits=0,flag='0')
  scs = formatC(secs, width=2,format='f',digits=0,flag='0')
  todFormatted = paste(hrs,mns,scs,sep=":")
  return(secs)
}

genData <- function()
{
  nCustomersPerWeek <- 63000/8*5
  dayPeak <- c(2,2,2,1,1)
  dayPeakNorm <- dayPeak/sum(dayPeak)
  arrivalPeak <- c(1, 1, 2, 4, 2, 1, 1, 1, 1, 2, 4, 6, 6, 6, 4, 2, 1, 1)
  arrivalPeakNorm <- arrivalPeak/sum(arrivalPeak)

  pCardLoad <- c(0.05, 0.05, 0.025, 0.025, 0.025)

  dows <- c('Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday')
  dayHrs <- 16:17
  dayMinutes <- 1:60
  stations <- c('Pretoria', 'Centurion', 'Midrand', 'Marlboro', 'Sandton', 'Rosebank',
                'Park', 'Rhodesfield', 'OR-Tambo')

  mydataTemplate <- data.frame(day = numeric(),
                               dow = character(),
                               hour = numeric(),
                               timeStamp = character(),
                               cardLoad = logical(),
                               origin = character())

  mydata <- mydataTemplate

  i = 0
  for (nWeek in 1:(30/length(dows)))
  {
    for (d in 1:length(dows))
    {
      i = i + 1
      nDay = nCustomersPerWeek*dayPeakNorm[d]
      for (h in 1:length(dayHrs))
      {
        arrivalRatePerHour <- nDay*arrivalPeakNorm[h]
        startTime <- 0
        arrivalTime <- cumsum(rexp(arrivalRatePerHour*2, rate = arrivalRatePerHour)*60*60)
        timeStamp <- tod(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        nArrivals <- length(timeStamp)

        day <- rep(i, nArrivals)
        dow <- rep(dows[d], nArrivals)
        hour <- rep(dayHrs[h], nArrivals)
        minute <- tomns(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        second <- tosecs(arrivalTime[arrivalTime < 60*60], dayHrs[h])
        cardLoad <- runif(nArrivals, 0, 1) < pCardLoad[d]
        origin <- sample(stations, size = nArrivals, replace = T)
        hourFrame <- data.frame(day, dow, hour, minute, second, timeStamp, cardLoad, origin)
        mydata <- rbind(mydata, hourFrame)
      }
    }
  }
  return(mydata)
}

gauArrive <- genData()
rm(genData)
rm(tod)
rm(tomns)
rm(tosecs)
```
`@sample_code`
```{undefined}
# The data are available in the gauArrive dataframe.

#1. Calculate the minutes since the start of day that each customer arrived at and assign your answer to the vector `minuteArrived`.



#2. Calculate the inter-arrival time between consecutive arrivals and assign the result to the vector `interArrive`. It may be useful to draw a histogram of `interArrive`.



#3. Remove all negative inter arrival times from `interArrive` and assign the result to `interArriveClean`. It may be useful to draw a histogram of `interArriveClean`.



#4. Calculate the arrival rate for the exponential distribution using `interArriveClean` and assign your answer to `arriveRate`.



#5. Calculate the probability of a customer arriving within 0.5 minutes (30 seconds) after the current one arrived and assign your answer to `pWithin30sec`.



#6. View the above values by printing them to the console output via the `script.R` file.
```
`@solution`
```{undefined}
minuteArrived = gauArrive$hour*60 + gauArrive$minute + gauArrive$second/60
interArrive = diff(minuteArrived)
interArriveClean = subset(interArrive, interArrive >= 0)
arriveRate = 1/(mean(interArriveClean))
pWithin30sec = pexp(0.5, arriveRate)
pWithin30sec
```
`@sct`
```{undefined}
test_object("minuteArrived", undefined_msg = "Make sure to define an object `minuteArrived`.",
            incorrect_msg = "Make sure that you calculated the minute of each customer's arrival correctly, using the formula given in the instructions and assign your answer to the vector `minuteArrived`")     

test_object("interArrive", undefined_msg = "Make sure to define an object `interArrive`.",
            incorrect_msg = "Make sure that you calculated the time between all consecutive arrivals correct, using the function given in the instructions, and assign your answer to `interArrive`")    

test_object("interArriveClean", undefined_msg = "Make sure to define an object `interArriveClean`.",
            incorrect_msg = "Make sure that you removed negative inter arrival times and assigned the resulting vector to `interArriveClean`. You can use the `subset(vector, vector >= value)` command.")

test_object("arriveRate", undefined_msg = "Make sure to define an object `arriveRate`.",
            incorrect_msg = "Make sure that you calculated the arrival rate customers correctly, using the formula given in the notes and in the instructions, and assigned your answer to `arriveRate`.")

test_object("pWithin30sec", undefined_msg = "Make sure to define an object `pWithin30sec`.",
            incorrect_msg = "Make sure that you calculated the probability of a customer arriving within 30 seconds of the current customer, meaning the inter-arrival time is less than 30 seconds, and assigned your answer to `pWithin30sec`.")

test_output_contains("pWithin30sec", incorrect_msg = "You did not view the actual value `pWithin30sec`.")

success_msg("Correct! That's it for this chapter.")
```



