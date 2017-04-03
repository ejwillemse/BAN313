---
title_meta  : Case study 9
title       : Case study 9 - Developing simulation models using probability distributions (advanced)
description : "In this case study we will analyse how a random order process influences inventory levels and stock-outs of our product. We will further analyse the impact of a random production process, identical to the one from the previous chapter, on inventory levels."

--- type:MultipleChoiceExercise lang:r xp:0 skills:1 key:8eaf7b9ebd
## Background

In the previous case study we modelled a random production process.
In this chapter we will move to the supply-side of a business and model a random order process for our product.

**Before continuing with this chapter, please do the following:**

1. Complete [Case study 8 - Developing simulation models using probability distributions (introduction)](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) on Datacamp.
2. Complete [Functions](https://campus.datacamp.com/courses/industrial-analysis-using-r/10161) on Datacamp.
3. Review the lecture slides `Lecture_Week7.pdf`, available on _clickUP_ under `Theme 2: Simulation models/Week 8 Developing simulation models using probability distributions`, or by clicking [on this link](https://clickup.up.ac.za/bbcswebdav/pid-1017449-dt-content-rid-11965449_1/courses/ban313_s1_2017/Lecture_Week7%281%29.pdf).

**After completing the above, read the below instructions carefully.**

Each day, customers place orders for our product.
Each customer is unique and have different requirements, therefore the amount of products ordered differs from day-to-day.

Inventory is used to account for the daily variation.
The company can use the extra products in inventory for high-order days.
For low-order days, the company can store surplus manufactured products as inventory.

As the inventory manager we wish to predict our inventory levels for the coming month and specifically see how likely a stock-out is.

We have a fixed production-run setup, meaning we produce a fixed number of products each day.
Each day's production-run products is moved to inventory on the next day before the start of business.

When customers place orders, it is filled form the available inventory on the same day.
If the day's orders cannot be filled, due to low inventory levels, the customers take the available inventory and purchases the balance elsewhere.

In this lab we do the following:

1. Develop a simulation model of our stock-outs and inventory levels based on random orders.
2. Adapt the simulation model to account for limited inventory space and see how it influences stock-outs and inventory levels.
3. Incorporate the drilling-process simulation model from our previous chapter to account for random production and random orders, and analyse stock-outs and inventory levels.
4. 
