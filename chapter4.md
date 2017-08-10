---
title       : The Whys and Hows of Tidy Data
description : Why we need tidy data and using `tidyr` to make messy data tidy


--- type:NormalExercise lang:r xp:0 skills:1 key:2b27344379
## What you'll learn in this Chapter:

+ How to identify tidy data
+ How to make messy data tidy using `tidyr`
+ Long vs. Wide data
+ The tidy manifesto
+ Joining tables on a common identifier

After this chapter, you will be able to handle about 90% of your data visualization,
data manipulation, and data cleaning problems. Hopefully you'll have gained enough
confidence to start going beyond this Bootcamp and learn some more.

*** =instructions
Just move on to the next exercise.

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





--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:80e373da6a
## What is Tidy Data?

Tidy data is defined as data that has the following properties:

- each row corresponds to an observation
- each variable is a column
- each type of observation is in a different table

![Tidy Data](http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/tidy-1.png)

One thing to keep in mind is when columns are not separate variables. One clue is that the column names could be considered categories of a single variable. This means that the columns themselves could each correspond to a single observation.

Look at the `dem_score` dataset in the console by using either `head()` or `summary()` Would you consider this dataset tidy?

*** =instructions

- Yes, we consider each row to be a separate observation
- No, each column is not a separate observation, but actually multiple observations

*** =hint

*** =pre_exercise_code
```{r}
dem_score <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv")
total_fertility <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/total_fertility.csv")
```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}
test_mc(correct = 2)
```




--- type:NormalExercise lang:r xp:100 skills:1 key:c1c536cd5d
## tidyr::gather()


*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
dem_score <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv")
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
--- type:NormalExercise lang:r xp:100 skills:1 key:0c7200c72b
## Long versus Wide Data


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


--- type:NormalExercise lang:r xp:100 skills:1 key:87aa2a11b4
## Joining two tables together


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

--- type:NormalExercise lang:r xp:100 skills:1 key:24146e724e
## Loading data

We've been hiding some of the complexity of the analysis procedure from you until now. One of the 
most confusing parts about R to learn is how to load data. We'll take it step by step.

There are three main formats we'll have to deal with:

+ Comma Separated Values (`.csv` files) - use the `readr` package
+ Tab Delimited Files (`.tsv` or `.txt` files) - use the `readr` package
+ Excel Spreadsheets (`.xls` and `.xlsx` files) - use the `readxl` package

The functions in these packages will make a guess as to the formatting of the columns, but
sometimes you will have to fix these. We'll show you how to do that in the next section.

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

