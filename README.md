---
title: "Test"
output: html_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```
Create a data set that may look like what you have.  You don't need to worry about extra variables, because you are assuming that there will be no difference between the extra variables. So your outcome variable can just be standard normal, because we will add the actual effect later on.  So with 120/3 there are 40 people.  Extending n to 15 means we are adding 15 participants   

Here is a model with random intercepts only
```{r}
library(simr)
set.seed(12345)
dat = data.frame(cbind(y = rnorm(120), time = rep(1:3, 40), id = rep(1:40, each = 3))); dat
model1 = lmer(y ~ time + (1 | id), data = dat)
fixef(model)["time"] = .2
set.seed(12345)
powerCurve(model, along = "id", nsim = 10)

model2 = extend(model, along = "id", n = 90)

powerCurve(model2, nsim = 10, along = "id")
```
Now let's run the model with random intercepts and slopes.  Remember that you can use a standard effect size, because the data is standardized and which is the same has a standard deviation increase which is the same has a cohen D.
```{r}
set.seed(12345)
dat = data.frame(cbind(y = rnorm(120), time = rep(1:3, 40), id = rep(1:40, each = 3))); dat
model1 = lmer(y ~ time + (time | id), data = dat)
fixef(model)["time"] = .2
set.seed(12345)
powerCurve(model, along = "id", nsim = 10)

model2 = extend(model, along = "id", n = 90)

powerCurve(model2, nsim = 10, along = "id")
```
Here is the simple random intercepts and slopes model for looking at time 

$$ Level~1:~~~{y_{ij} = \beta_{0j} + \beta_{1j}(time_{ij}) + e_{ij}}~~~ (1.1)$$

$$ Level~2~Intercept:~~~{\beta_{0j} = \gamma_{00} + u_{0j}} ~~~ (1.2)$$

$$ Level~2~Slope:~~~{\beta_{1j} = \gamma_{10} + u_{1j}} ~~~ (1.2)$$
$$Mixed~model: ~~~{y_{ij} = \gamma_{00}+\gamma_{10}(Time_{ij}) + u_{1j} + u_{0j} + e_{ij}} ~~~(1.3)$$
