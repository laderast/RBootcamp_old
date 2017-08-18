---
title       : Simple Stats and Modeling with broom
description : Now we have tidy data, let's start doing some statistics!


--- type:NormalExercise lang:r xp:100 skills:1 key:e91efd94b6
## We've built a foundation. Now to stats!

About 80-90% of any data analysis project is doing the data cleaning. Once you've confirmed that your data is
cleaned properly and is tidy, now you can start trying some simple statistics on your data.

We're going to use `broom`, which is another package available in the `tidyverse`. `broom` make the output
and results of statistical models tidy, and easy to extract. We'll try to leverage all our skills to put together
a simple data story given our data.

Our questions of interest are the following:

+ Is mercury exposure greater within people who are fishermen versus those who are not? (t-test)
+ What are the best predictors of mercury exposure? (multiple linear regression)

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```


--- type:NormalExercise lang:r xp:100 skills:1 key:3e59cb838f
## Let's explore the fishermen mercury dataset

We are going to explore a dataset called the fishermen mercury dataset, which consists of factors 
related to mercury exposure among two groups: fishermen and non-fishermen, who are our control group. 

Take a look at the readme for this dataset first: [README](http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fishermen_mercury_README.txt)

Then use `glimpse` to take a look at the structure of the data and try `table()` on 
`fisherman`. What are the different categories of `fishpart` and `fisherman`?

Now use `table()` as part of a pipe to look at the cross-table of `fisherman` and `fishpart`. Are fishermen more likely to eat more whole fish than non-fishermen?

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
glimpse(fishdata)

table()

fishdata %>% select(___,___) %>% table()
```

*** =solution
```{r}
glimpse(fishdata)

table(fishdata$fisherman)

fishdata %>% select(fisherman,fishpart) %>% table()
```

*** =sct
```{r}
ex() %>% check_function('table',index=1) %>% check_result() %>% check_equal()
ex() %>% check_function('select') %>% check_result() %>% check_equal()

success_msg("Nice tabling!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:80227d57fb
## Visualize Mean of Total Mercury by Fisherman Status

Let's visualize mean `total_mercury` (total mercury) by `fisherman` status to 
see whether there is a difference between them.

*** =instructions

Use `geom_boxplot()` to visualize the median of `total_mercury` conditioned on
`fisherman` status (if you can't remember, 
[here's the exercise](https://campus.datacamp.com/courses/rbootcamp/ggplot2-and-categorical-data?ex=11)). 
Make sure to cast `fisherman` as a factor.

We can add the mean as a point using `stat_summary` to see how the mean differs from the median.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
#plot total_mercury here
ggplot(fishdata,aes(x=___,y=___)) + geom_boxplot() +
  stat_summary(fun.y="mean",geom="point",pch=3,color="red")
```

*** =solution
```{r}
ggplot(fishdata, aes(x=factor(fisherman), y=total_mercury)) + geom_boxplot() +
  stat_summary(fun.y="mean",geom="point",pch=3,color="red")
```

*** =sct
```{r}
test_ggplot()
success_msg("Beautiful plot!")
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:6e3730d99d
## Is there a difference?

Is there a difference between the two groups: fishermen and non-fishermen?

*** =instructions

- No, there isn't. The means are too close.
- Yes, there is. The intervals overlap but there is a clear difference in means

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
ggplot(fishdata, aes(x=factor(fisherman), y=total_mercury)) + geom_boxplot() +
  stat_summary(fun.y="mean",geom="point",pch=3,color="red")
```

*** =sct
```{r}
test_mc(correct=2)
success_msg("That's right, fisherman seem to have higher total mercury")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:556d3ae28b
## T-test of means for fisherman status

A common and very useful test used to compare two means for Normally distributed data is the Student's T Test. The null hypothesis is that two means from two independent groups are indentical, and the alternative hypothesis is that the two means are not identical (two-sided test).

In our case, we want to test whether the mean total mercury in fishermen is the same or different than the mean total mercurcy for non-fishermen. What is the p-value from this test?

*** =instructions

Use the function `t.test` to compare the mean `total_mercury` in fishermen and nonfishermen.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
# Use the formula input option, see ?t.test help page: a formula of the form lhs ~ rhs where lhs is a numeric variable giving the data values and rhs a factor with two levels giving the corresponding groups.
t.test(~,data=fishdata)
```

*** =solution
```{r}
t.test(total_mercury~fisherman,data=fishdata)
```

*** =sct
```{r}
ex() %>% check_function('t.test') %>% check_result() %>% check_equal(incorrect_msg = "Not quite. Check your inputs.")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:36a8333264
## Sweep up that output with Broom

That output was pretty ugly, wasn't it? There's an extremely handy package called `broom` that can help us clean it up into a tidy data table. Funnily enough, the handy function is called `tidy`.

*** =instructions

Use `tidy()` to save the output of the t.test to a data.frame called `tidyTtest`. Then `glimpse()` it to see what's in there, and lastly explore how you can easily extract results (like the p-value) in the usual data.frame way.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
fishTtest <- t.test(total_mercury~fisherman,data=fishdata)

#use tidy here
tidyTtest <- tidy()

#glimpse your output
glimpse()

# extract a p-value
tidyTtest$

```

*** =solution
```{r}
fishTtest <- t.test(total_mercury~fisherman,data=fishdata)

# use tidy here
tidyTtest <- tidy(fishTtest)

# glimpse your output
glimpse(tidyTtest)

# extract a p-value
tidyTtest$p.value
```

*** =sct
```{r}
ex() %>% check_object('tidyTtest') %>% check_equal()
ex() %>% check_function('glimpse') %>% check_result() %>% check_equal()
test_output_contains("tidyTtest$p.value",
	                     incorrect_msg = "Have you extracted the p.value column?")
success_msg("So fresh and so clean clean!")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:a8e91afadb
## Let's delve deeper into the data

We have other covariates in our data. We want to see whether we can use these covariates to
predict the level of Mercury Exposure. Could another covariate be confounding the relationship between fisherman and total mercury?

Let's first look at the scatter plots of `weight` and number of fish meals per week `fishmlwk` vs `total_mercury`. Do there seem to be any associations?

*** =instructions

Use `geom_point` to make scatterplots of the two variables.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
# draw a scatterplot of weight (x-axis) vs total_mercury (y axis) and color by fisherman category
ggplot(fishdata,aes(___,___,color=factor(___)))+___

# draw a scatterplot of fishmlwk (x-axis) vs total_mercury (y axis) and color by fisherman category
ggplot(fishdata,aes(___,___,color=factor(___)))+___

```

*** =solution
```{r}
ggplot(fishdata,aes(x=weight,y=total_mercury,color=factor(fisherman)))+geom_point()

ggplot(fishdata,aes(x=fishmlwk,y=total_mercury,color=factor(fisherman)))+geom_point()
```

*** =sct
```{r}
test_ggplot(index=1)
test_ggplot(index=2)
success_msg("Beautiful plots!")
```



--- type:NormalExercise lang:r xp:100 skills:1 key:d5e745c9f5
## Linear Regression

It looks like both weight and number of fish meals per week are associated with total mercurcy. They also appear to be associated with fisherman status. We saw earlier from our cross-table output that fisherman tend to eat more fish (surprised?).

We can use linear regression to adjust for these possible confounders. We first build a univariate linear regression with just fisherman as a predictor. Then we compare it to a multiple linear regression with the three indepedent variables.

Note: the p-value from the linear regression is similar but not exactly the same as the t-test since the t-test assumed equal variances between groups (argument `var.equal=TRUE` by default in `t.test()`).

*** =instructions

Fit a linear regression with `fisherman` as the indepedent variable and `total_mercury` as the dependent variable. Save this as `fit_univariate`. Then, fit a multiple linear regression with `fisherman`, `weight`, `fishmlwk` as dependent variables. Save this as `fit_multiple`.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}

# fit the univariate model
fit_univariate <- lm(~,data=fishdata)

# fit the multiple predictor model with fisherman, weight, fishmlwk
fit_multiple <- lm(~,data=fishdata)

# let's look at the output
summary(fit_univariate)
summary(fit_multiple)
```

*** =solution
```{r}
fit_univariate <- lm(total_mercury~fisherman,data=fishdata)
fit_multiple <- lm(total_mercury~fisherman+weight+fishmlwk,data=fishdata)

# let's look at the output
summary(fit_univariate)
summary(fit_multiple)
```

*** =sct
```{r}
ex() %>% check_object('fit_univariate') %>% check_equal()
ex() %>% check_object('fit_multiple') %>% check_equal()
success_msg("Now we're regressing!")
```



--- type:NormalExercise lang:r xp:100 skills:1 key:7e98c03571
## Broom with linear regression

Did you notice we have more ugly output? It's not really so bad to look at (the stars are pretty afterall) but when you try to grab model information out of it things get messy and complicated quickly.

For illustration, let's try to get the p-value for fisherman from the multiple regression model the normal way, then lets try the broom way. We can get covariate information from `tidy()`.

*** =instructions

Use the output from `summary` to obtain a p-value for fisherman from `fit_multiple`. Then, use `broom::tidy`.


*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
# here is our model
fit_multiple <- lm(total_mercury~fisherman+weight+fishmlwk,data=fishdata)

# what is in summary(fit_multiple)?
str(summary(___))
# yikes, right? well at least it's a list

# summary()$coef gives you a numerical matrix, extract fisherman's p-value
summary(fit_multiple)$coefficients[___,___]

# or use broom's tidy to see the covariate estimates and p-values--look, a tibble!
tidy()

# we can use dplyr on this to obtain our p.value
tidy() %>% filter() %>% select()
```

*** =solution
```{r}
# here is our model
fit_multiple <- lm(total_mercury~fisherman+weight+fishmlwk,data=fishdata)

# what is in summary(fit_multiple)?
str(summary(fit_multiple))
# yikes, right? well at least it's a list

# summary()$coef gives you a numerical matrix, extract fisherman's p-value
summary(fit_multiple)$coefficients[2,4]

# or use broom's tidy to see the covariate estimates and p-values--look, a tibble!
tidy(fit_multiple)

# we can use dplyr on this to obtain our p.value
tidy(fit_multiple) %>% filter(term=="fisherman") %>% select(p.value)

```

*** =sct
```{r}
test_output_contains("summary(fit_multiple)$coefficients[2,4]",
	                     incorrect_msg = "Have you extracted the correct p.value row and column using summary(fit_multiple)$coefficients?")
ex() %>% check_function('tidy') %>% check_result() %>% check_equal()
ex() %>% check_function('filter') %>% check_result() %>% check_equal()
ex() %>% check_function('select') %>% check_result() %>% check_equal()
success_msg("Tidy all the things!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:395ea60755
## Broom with linear regression: glance

Now let's do something similar with the $R^2 $ summary measure which is a measure of model fit in that it quantifies the amount of variance explained in the outcome (total mercury) explained by the predictors. Using broom, we get model summary level information from the function `glance()`. While `tidy()` returned a tibble/data_frame of covariate information with one row for each model term, `glance()` will return a tibble with just one row with all the pertinent single value model information.

*** =instructions

Use the output from `summary` to obtain an $R^2 $ for fisherman from `fit_multiple`. Then, use `broom::glance`.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}
# here is our model
fit_multiple <- lm(total_mercury~fisherman+weight+fishmlwk,data=fishdata)

# ok where is that R^2? look at the names of the summary list again
names(summary(fit_multiple))

# call the name we need
___$___

# or we can use glance
glance()

# and select that R^2
glance() %>% select()

```

*** =solution
```{r}
# here is our model
fit_multiple <- lm(total_mercury~fisherman+weight+fishmlwk,data=fishdata)

# ok where is that R^2? look at the names of the summary list again
names(summary(fit_multiple))

# call the name we need
summary(fit_multiple)$r.squared

# or we can use glance
glance(fit_multiple)

# and select that R^2
glance(fit_multiple)%>%select(r.squared)
```

*** =sct
```{r}
test_output_contains("summary(fit_multiple)$r.squared",
	                     incorrect_msg = "Have you extracted the R^2 using summary()$?")
ex() %>% check_function('glance') %>% check_result() %>% check_equal()
ex() %>% check_function('select') %>% check_result() %>% check_equal()
success_msg("I see you did a great job!")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:abcb19fe03
## Compare our models

We've built two models: `fit_univariate` and `fit_multiple`. The first only contains fisherman as a predictor, and the second contains fisherman as well as weight and number of fish meals per week. Which fits the data better, and how does the association of fisherman with total mercury change when we adjust for weight and number of fish meals per week?

*** =instructions

Extract the covariate information using `tidy` from the two models and bind them together into one data frame with `bind_rows` in the dplyr package. Do the same with the model summary information using `glance`.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fisherman_mercury_modified.csv")
```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:b9ac8f4738
## Visualizing proportions of fishpart by fisherman status

Let's try and answer the question of fishpart eating by fisherman status. 

Produce a proportional barplot ([Here's how if you forgot](https://campus.datacamp.com/courses/rbootcamp/ggplot2-and-categorical-data?ex=5))
of `fishpart` versus `fisherman`. 

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}
```

*** =solution
```{r}
```

*** =sct
```{r}

```

