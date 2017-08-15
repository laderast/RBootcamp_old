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
## Let's explore the fisherman mercury dataset

We are going to explore a dataset called the fisherman mercury dataset, which consists of factors 
related to mercury exposure among two groups of fishermen. 

Take a look at the readme for this dataset first: http://www.stat.ufl.edu/~winner/data/fishermen_mercury.txt

Then use `glimpse` to take a look at the structure of the data and try `table()` on both `fishpart` and 
`fisherman`. 

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


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:b9ac8f4738
## <<<New Exercise>>>


*** =instructions

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}

```
