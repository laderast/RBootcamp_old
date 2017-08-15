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

Take a look at the readme for this dataset first: http://www.stat.ufl.edu/~winner/data/fishermen_mercury.txt

Then use `glimpse` to take a look at the structure of the data and try `table()` on both `fishpart` and 
`fisherman`. What are the different categories of `fishpart` and `fisherman`? 

Are fishermen more likely to eat more whole fish than non-fishermen?

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fishermen_mercury.csv")
```

*** =sample_code
```{r}
glimpse(fishdata)

table(fishdata$fishpart)
table(fishdata$fisherman)

fishdata %>% select(fishpart, fisherman) %>% table()
```

*** =solution
```{r}
glimpse(fishdata)

table(fishdata$fishpart)
table(fishdata$fisherman)
```

*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:80227d57fb
## Visualize Mean of Total Mercury by Fisherman Status

Let's visualize both mean `TotHg` (total mercury) and `MeHg` by `fisherman` status to 
see whether there is a difference between them.

*** =instructions

Use `geom_boxplot()` to visualize the means of of both `TotHg` and `MeHg` by
`fisherman` status. Make sure to cast `fisherman` as a factor.

*** =hint

*** =pre_exercise_code
```{r}
library(readr)
library(dplyr)
library(ggplot2)
library(broom)

fishdata <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/fishermen_mercury.csv")
```

*** =sample_code
```{r}
#plot totHg here
ggplot(fishdata,aes()) + geom_boxplot()
```

*** =solution
```{r}
ggplot(fishdata, aes(x=factor(fisherman), y=TotHg)) + geom_boxplot()
```

*** =sct
```{r}
test_ggplot()
```



--- type:NormalExercise lang:r xp:100 skills:1 key:556d3ae28b
## T-test of means for fisherman status


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


```
