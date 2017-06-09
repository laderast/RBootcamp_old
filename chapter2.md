---
title       : ggplot2 and categorical data
description : More on plotting using ggplot2

--- type:NormalExercise lang:r xp:100 skills:1 key:6b1d7abb63
## Review of factors

Factors are how R represents categorical data.

There are two kinds of factors: 

+ `factor` - used for *nominal* data ("Ducks","Cats","Dogs")
+ `ordered`- used for *ordinal* data ("10-30","31-40","41-60")

We'll manipulate our barplots and add more information using factors.

Here's the simple dataset we'll use to investigate how to work with factors in `ggplot`.

*** =instructions
Using the console, look at the `pets` dataset. For the `factor` and `ordered` variables in `pets`, show the categories for each using `levels()`.

*** =pre_exercise_code
```{r}
read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
##show pets data
pets
```

*** =solution
```{r}
##show pets data
pets
```
*** =sct
```{r}
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8102f92bcd
## A Basic Barplot using `geom_bar()`

Now that we understand our the categories in our data, we can begin to visualize them using barplots generated with the `geom_bar()` geom.

The `geom_bar()` default is to count the number of values with each factor level. Note that you don't map to a y-aesthetic here, because the y values are the counts.

Given this dataset, we might want to ask how many pets have the same name. What is the most popular name?

*** =instructions
Try mapping another variable to `fill` (try both `weight` and `animal`). Are the results what you expected?

*** =pre_exercise_code
```{r}
read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
##show pets data
pets
```

*** =solution
```{r}
##show pets data
pets
```
*** =sct
```{r}
```

