---
title_meta  : Case study 10
title       : Case study 10 - Sensitivity analysis on queueing models
description : "In this case study we will first develop a basic queueing model
that models how customers arrive at an ATM and perform transactions.
Thereafter we will test the sensitivity of our model to model parameters."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

**Read the below instructions carefully.**

In this case study we will model a simple queuing system that represent customers using an ATM.
Customers arrive at an ATM and perform a number of ATM transactions.
After completing their transactions, the customer takes their card and leaves.
The next customer can then use the ATM.
When the ATM is in use, the next customers have to wait in-line until it is their time to use the ATM.

A key measurement of such a queueing system is how long customers wait in-line, before it is their turn to use the ATM.
If customers wait too long, the system should be improved. Either be reducing the time that customers spend completing transactions or by increasing the number ATMs.

Is this chapter we will develop a simulation model for this simply process and measure the average waiting time of customers.
When developing the model, we need to make certain assumptions, such as the arrival time and time required by customers to complete the transactions.

Critical to model development is to test the impact of the assumptions on the model output and to see how sensitive the model is to a change in any of the assumptions.
After developing the queueing model we will conduct informal sensitivity analysis.
We will sequentially change the model input parameters and see how it influences the customer queuing times.
By doing so we can see which input parameters are critical for our analysis, and spend extra effort to ensure that they are accurate, such as obtaining more data for the random variable.

In this lab we do the following:

1. Develop a simulation model of queueing model.
2. Run the simulation model, measure and analyse the average customer waiting time.
3. Perform sensitivity to determine which model parameters, if changed, will have the biggest influence on customer waiting time.

The specific R functions that are applicable to this chapter are:

```
rnorm(), rexp(), sample(), function(), for, if
```

To find out more about the functions, type `?` followed by the R-function in the R console.

Take note that the data used for this chapter is randomly generated and will change:

- each time the chapter is attempted; and
- when moving from one exercise to the next.

When completing the chapter, read all the available information and instructions _carefully_, and if necessary, review the applicable engineering statistics methods.

**DO NOT** jump straight to the instructions and skip the each questions' background information. The question backgrounds contain valuable information to assist you in completing the questions.

To continue with this chapter confirm the following:

*** =instructions

* I have read **ALL** the instructions on this page carefully.

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
test_mc(correct = 1, feedback_msgs = c(msg_success))
```
