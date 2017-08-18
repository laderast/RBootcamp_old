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
predict the level of Mercury Exposure.

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


--- type:NormalExercise lang:r xp:100 skills:1 key:abcb19fe03
## Compare our models

We've built two models: 

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

