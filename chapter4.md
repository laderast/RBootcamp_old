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
Just move on to the next exercise. (CTRL+K)

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

`gather()` is a special function in the `tidyr` package that takes columns and combines them into a single column. 
`gather()` has the following syntax:

```{r}
dem_score %>% gather(key=year, value=score, -country)
```
The first argument, `key`, is the name of our 'gathered' variable. We're smushing all of the columns that have year names and 
calling the column names, or the `key` the name of `year`. The second argument, `value`, is the actual observations. Finally, if we don't want a column to be gathered, we can leave it out with the `-` notation (here, `-country`).

*** =instructions

Try out the above `gather` statement. Use a `mutate` expression to remove the `X` in front of `year`. 
Look up the documentation for `str_replace` to see how to remove the `X` in the mutate statement. Assign the output
to `gatheredData`.

*** =hint
The `mutate` expression to remove the `X` is `mutate(year=str_replace(year, "X", "")`.

*** =pre_exercise_code
```{r}
dem_score <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv")
```

*** =sample_code
```{r}
library(dplyr)
library(tidyr)
library(stringr)

gatheredData <- 
```

*** =solution
```{r}
library(dplyr)
library(tidyr)
library(stringr)

gatheredData <- dem_score %>% gather(key=year, value=score, -country) %>%
        mutate(year=str_replace(year, "X",""))

head(gatheredData)
```

*** =sct
```{r}
test_object("gatheredData", incorrect_msg = "Not quite. Did you run `gather` and `mutate`?")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:0c7200c72b
## tidyr::spread()

`spread()` does the opposite of `gather()`. It "unbundles" a column into multiple columns.
This situation can happen because related measurements that consist of an observation are
collected separately, or someone has `gather`ed the data a little too enthusiastically.

`spread()` has the following syntax:

```{r}
gatheredData %>% spread(key=country, value=score)
```
We see that `spread()` takes two arguments: the first is the *key* column, which is the 
variable name that contains our column names of interest; the second is the *value* 
column, which is the variable that contains the values we want to fill with.

*** =instructions

Let's transform our `gatheredData` into a matrix again, but with each column having a 
`country`. Assign the output to `spreadData`.

*** =hint

*** =pre_exercise_code
```{r}
dem_score <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv")
library(dplyr)
library(tidyr)
library(stringr)

gatheredData <- dem_score %>% gather(key=year, value=score, -country) %>%
        mutate(year=str_replace(year, "X",""))
```

*** =sample_code
```{r}
spreadData <- 
head(spreadData)
```

*** =solution
```{r}
spreadData <- gatheredData %>% spread(key=country, value=score)
head(spreadData)
```

*** =sct
```{r}
success_msg("Great! Now you know how to spread!")
test_object("spreadData", incorrect_msg = "Almost, but not quite")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:fbd075070a
## dplyr::separate()

Have you ever had a table which had a column that actually consisted of bunch of other columns together,
separated by something? Something like this?

```{r}
"value1/value2/value3"
```
`separate()` is made to make this one variable into many other variable. Separate takes the following arguments:

```{r}
health_code_separated <- 
    health_code_example %>% 
        separate(col=HealthCodeEncounterCode, 
        into=c("HealthCode", "EncounterCode"), sep="/")
```
Here we specify the variable name with `col`, the names of the new columns with `into`, and 
the delimiter, or separator in the variable to separate on, with `sep`. Note that `sep` can
be any string.

*** =instructions

Run the above code and `separate` `HealthCodeEncounterCode` into `HealthCode` and `EncounterCode` for  `health_code_example`, assigning the output to `health_code_separated`. Show
those patients that had `HealthCode==410`, assigning them to `patients410`. Show `patients410`.

*** =hint

*** =pre_exercise_code
```{r}
library(tidyr)
library(dplyr)
library(readr)
health_code_example <- 
    read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/healthExample.csv")
```

*** =sample_code
```{r}
health_code_separated <- 
        
patients410 <- health_code_separated %>% filter()

patients410
```

*** =solution
```{r}
health_code_separated <- 
    health_code_example %>% 
        separate(col=HealthCodeEncounterCode, 
        into=c("HealthCode", "EncounterCode"), 
        sep="/")
        
patients410 <- health_code_separated %>% filter(HealthCode==410)

patients410
```

*** =sct
```{r}

```
--- type:NormalExercise lang:r xp:300 skills:1 key:3d8e137017
## Wide Versus Long Data

In addition to tidy data, we can have long data versus wide data. We call a dataset as *long data*
because the format of the data has many more rows than columns, and we call data *wide data*, 
because it has more columns than rows. You have seen how to transform a *wide* dataset (`dem_score`) 
into a *long* one with `gather()` and transform it into a different *wide* format with (`spread`).

In general, I tend to work with long data because this format makes it easeir to aggregate the data for 
plots when I have a lot of covariates. Let's look at what's possible because the data is in a long format.

Let's practice with another dataset in long format, called `fertilityTidy`. You can look at the 
original data as `fertilityData`. We'll summarize it in two different ways.

*** =instructions

Look at `fertilityTidy`. Show the average fertility by country to present day 
by using `dplyr` verbs, calling this variable `meanCountryRate`. Assign the summarized 
data to `fertilityMeanByCountry`. Show `fertilityMeanByCountry`.

Next, show average fertility by `Year`, using `group_by/summarize()` assigning the
summarized data to `fertilityMeanByYear`. Show `fertilityMeanByYear`.

*** =hint
You'll have to first use a `filter()` statement, and then a `group_by/summarize` statement.

*** =pre_exercise_code
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)
fertilityData <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/total_fertility.csv", check.names = FALSE)

colnames(fertilityData)[1] <- "Total fertility rate"

fertilityTidy <- 
  gather(fertilityData, "Year", "fertilityRate", -`Total fertility rate`) %>% 
  select(Country = `Total fertility rate`, Year, fertilityRate) %>% 
  #remove na values (there are countries that have no information)
  filter(!is.na(fertilityRate))

```

*** =sample_code
```{r}
fertilityMeanByCountry <- fertilityTidy %>%

#show fertlityMeanByCountry
fertilityMeanByCountry

fertilityMeanByYear <- fertilityTidy %>%

#show fertilityMeanByYear
fertilityMeanByYear
```

*** =solution
```{r}
fertilityMeanByCountry <- fertilityTidy %>% 
        group_by(Country) %>% summarize(meanCountryRate=mean(fertilityRate))
    
#show fertlityMeanByCountry
fertilityMeanByCountry
        
fertilityMeanByYear <- fertilityTidy %>% 
        group_by(Year) %>% summarize(meanYearRate=mean(fertilityRate))
        
#show fertilityMeanByYear
fertilityMeanByYear
```

*** =sct
```{r}

```
--- type:NormalExercise lang:r xp:500 skills:1 key:651465f93b
## Putting  dplyr, tidyr, and ggplot2 all together

Let's put everything we've learned together on a new data.frame called `MouseBalanceTimeSeries`.

*** =instructions

Look at the `MouseBalanceTimeSeries` `data.frame`. This is a wide `data.frame` where 
each column corresponds to the time (in seconds) a mouse stayed on
a balance beam pre and post treatment. `gather()` the measurements into a single column called 
`time` with a key called `interventionStatus`. 

Use `separate()` to make `measurementStatus` into 
two columns (`intervention` and `replicate`) by specifying `sep="Treat"`. 

Remove any observations 
that are `NA`. Assign the output to `gatheredMouse`. 

Finally, make a boxplot of `gatheredMouse`, plotting 
`time` versus `intervention` using `geom_boxplot`.

*** =hint

*** =pre_exercise_code
```{r}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/module4.RData"))
MouseBalanceTimeSeries <- data.frame(MouseBalanceTimeSeries)
MouseBalanceTimeSeries <- data.frame(mouseID=rownames(MouseBalanceTimeSeries), MouseBalanceTimeSeries)
colnames(MouseBalanceTimeSeries)[3] <- "PreTreat2"
```

*** =sample_code
```{r}
library(tidyr)
library(dplyr)
gatheredMouse <- MouseBalanceTimeSeries %>% 
```

*** =solution
```{r}
library(tidyr)
library(dplyr)
library(ggplot2)

gatheredMouse <- MouseBalanceTimeSeries %>% gather(key=measurementStatus, value=time, -mouseID) %>%
    filter(!is.na(time)) %>% separate(measurementStatus, c("intervention","replicate"), sep="Treat")

ggplot(gatheredMouse, aes(x=intervention, y=time)) + geom_boxplot()
```

*** =sct
```{r}

```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:26c910c309
## Was there a difference?

Was there a difference in mean time spent on the balance beam pre and post treatment?

*** =instructions
- No, the times were too close to tell.
- Yes, the intervals overlapped, but the means were clearly different

*** =hint

*** =pre_exercise_code
```{r}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/module4.RData"))
MouseBalanceTimeSeries <- data.frame(MouseBalanceTimeSeries)
MouseBalanceTimeSeries <- data.frame(mouseID=rownames(MouseBalanceTimeSeries), MouseBalanceTimeSeries)
colnames(MouseBalanceTimeSeries)[3] <- "PreTreat2"

library(tidyr)
library(dplyr)
library(ggplot2)

gatheredMouse <- MouseBalanceTimeSeries %>% gather(key=measurementStatus, value=time, -mouseID) %>%
    filter(!is.na(time)) %>% separate(measurementStatus, c("intervention","replicate"), sep="Treat")

ggplot(gatheredMouse, aes(x=intervention, y=time)) + geom_boxplot()
```

*** =sct
```{r}
test_mc(correct=2)
```
--- type:NormalExercise lang:r xp:100 skills:1 key:24146e724e
## Loading data

We've been hiding some of the complexity of the analysis procedure from you until now. One of the most confusing parts about R to learn is how to load data. We'll take it step by step.

There are three main formats we'll have to deal with:

+ Comma Separated Values (`.csv` files) - use the `readr` package
+ Tab Delimited Files (`.tsv` or `.txt` files) - use the `readr` package
+ Excel Spreadsheets (`.xls` and `.xlsx` files) - use the `readxl` package

The functions in these packages will make a guess as to the formatting of the columns, but sometimes you will have to fix these. We'll show you how to do that in the next section.

Let's try the `readr` package. For `.csv` (Comma Separated Value) files, we can use the `read_csv` function. For this exercise, we have to use files that are available online. But if you have data on your disk, you can load it into R if you know the path (location) of the data file.

If you have a tab-separated file, you can use the general file reader, `read_delim`, supplying the argument `delim="\t"`. `"\t"` is the programmatic way of specifying a tab.

```{r}
library(readr)
dem_score <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv", header=TRUE)
```

*** =instructions
Take a look at the the `dem_score.csv` file here: http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv

Load this file as `dem_score`. Show the number of rows in `dem_score`.

*** =hint

*** =pre_exercise_code
```{r}
library(tidyverse)

dem_score <- read.csv()
nrow(dem_score)
```

*** =sample_code
```{r}
library(readr)
```

*** =solution
```{r}
library(readr)
dem_score <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv", header=TRUE)
nrow(dem_score)
```

*** =sct
```{r}

```
--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:d458cbbf05
## header=TRUE

What is the difference between `header=TRUE` and `header=FALSE`? Try it out in the console.

```{r}
library(tidyverse)
dem_score <- read_csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/dem_score.csv", header=TRUE)

head(dem_score)
```

*** =instructions

- There is no difference.
- `header=TRUE` is faster
- `header=TRUE` assumes the first row defines the column names, whereas `header=FALSE` assumes there are no column names

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
test_mc(correct=3)
```


--- type:NormalExercise lang:r xp:100 skills:1 key:1683bd8844
## readxl



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

A very common operation that we have to do is when we have two tables with common ids. Often times, we
want to merge them on these common ids. In order to do that, we have to be aware of the column name
in each table that contains the 

Then, we can do what's called a join.

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/module4.RData"))
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

--- type:NormalExercise lang:r xp:100 skills:1 key:7cdbd2ac43
## Inner Joins versus Outer Joins

What's the difference between these two joins?

```{r}
inner_join(Mouse
```

*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
load(url("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/module4.RData"))
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
--- type:NormalExercise lang:r xp:100 skills:1 key:54f7a84f13
## who_tidy


*** =instructions

*** =hint

*** =pre_exercise_code
```{r}
load("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/who_tidy.rda")
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
