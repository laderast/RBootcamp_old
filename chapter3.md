---
title       : Introduction to dplyr
description : Learn how to manipulate data into a ggplot2 friendly format

--- type:NormalExercise lang:r xp:0 skills:1 key:e0b4f34c19
## Introduction to dplyr

We've been looking at datasets that fit the `ggplot2` paradigm nicely; however, most data we encounter 
is really messy (missing values), or is a completely different format. In this chapter, we'll look 
at one of the most powerful tools in the `tidyverse`: `dplyr`, which lets you manipulate data frames. 
There is a function/action for most of the annoying tasks you have to use in data cleaning, which
makes it super useful.

In particular, we're going to look at six fundamental verbs/actions in `dplyr`:

- `filter()`
- `mutate()`
- `group_by()`/`summarize()`
- `arrange()`
- `select()`

Along the way, we'll do some data manipulation challenges. You'll be a `dplyr` expert in no time!


--- type:NormalExercise lang:r xp:100 skills:1 key:cca48c6abd
## Let's look at some data and ways to manipulate it.

We're going to use the `biopics` dataset in `fivethirtyeight` to do learn `dplyr`. This is a dataset of 761 different biopic movies.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
```

*** =instructions
Run a `summary` on the `biopics` dataset. It's already loaded up for you. How many categories are in the `country` variable?

*** =sample_code
```{r}
##run summary here

##show length of country categories here

```

*** =solution
```{r}
##run summary here
summary(biopics)
##length of country category here
length(levels(biopics$category))
```

*** =sct
```{r}


```
--- type:NormalExercise lang:r xp:100 skills:1 key:1929755973
## dplyr::filter()

`filter()` is a very useful `dplyr` command. It allows you to subset a `data.frame` based on variable criteria.

For example, if we wanted to subset `biopics` to those movies that were made in the `UK` we'd use the following statement.

```{r}
#subset the data using filter
biopicsUK <- filter(biopics, country=="UK")
#confirm that we have subsetted correctly
biopicsUK
```

Three things to note here: 
1. first argument to filter is always the dataset. We'll see another variation in a second.
2. For those who are used to accessing `data.frame` variables by `$`, notice we don't have to use `biopics$country`. Instead, we can just use the variable name `country`.
3. Our filter statement uses `==`. Remember that `==` is an equality test, and `=` is to assign something. (confusing the two will happen to you from time to time.)

*** =instructions
Filter `biopics` so that it only shows `Criminal` movies (you'll have to use the `type_of_subject` variable in `biopics`. Show how many rows are left.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
```

*** =sample_code
```{r}
#add your filter statement here
crimeMovies <- 

#show number of crime movies
```

*** =solution
```{r}
#add your filter statement here
crimeMovies <- filter(biopics, type_of_subject == "Criminal")

#show number of crime movies
nrow(crimeMovies)
```

*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:119cf149a3
## Comparison operators and chaining comparisons

Let's look at the following `filter()` statement:

```{r}
filter(biopics, year_release > 1980 & 
    type_of_subject == "Criminal")
```

Three things to note:
1. We used the comparison operator `>`. The basic comparisons you'll use are `>` (greater than), `<` (less than), `==` (equals to) and `!=` (not equal to) 
2. We also chained on another expression, `type_of_subject == "Criminal"` using `&` (and). The other chaining operator that you'll use is `|`, which corresponds to OR. 
3. Chaining expressions is where `filter()` becomes super powerful.

*** =instructions
Add another comparison to the chain using `&`. Use `person_of_color == FALSE`. Show how many rows are left from your filter statement.

*** =sample_code
```{r}
#add your comparison to the end of this filter statement
crimeFilms <- filter(biopics, year_release > 1980 & 
    type_of_subject == "Criminal")
    
#show number of rows in crimeFilms

```

*** =solution
```{r}
#add your comparison to the end of this filter statement
crimeFilms <- filter(biopics, year_release > 1980 & 
    type_of_subject == "Criminal" & person_of_color == FALSE)

#show number of rows in crimeFilms
nrows(crimeFilms)
```

*** =sct
```{r}

```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:1c76b8c8f2
## Quick Quiz about Chaining Comparisons

Which statement should be the larger subset?

*** =instructions
- `filter(biopics, year_release > 1980 & type_of_subject == "criminal")`
- `filter(biopics, year_release > 1980 | type_of_subject == "criminal")`

*** =sct

--- type:NormalExercise lang:r xp:100 skills:1 key:c66f0675f2
## The pipe character: `%>%`

We're going to introduce another bit of `dplyr` syntax, the `%>%` operator. `%>%` is called a `pipe` operator. 
What it does is that it takes the output of one statement and makes it the input of the next statement. When 
I'm describing it, I think of it as a "Then". For example, I read the following expression

```{r}
biopics %>% filter(race_known == "Known") %>%
    filter(subject_sex == "Female")
```
As: I took the `biopics` data, THEN I filtered it down with the `race_known == "Known"` criteria and 
THEN I filtered it down even further with `subject_sex == "Female"`.

`%>%` allows you to chain multiple actions in the `tidyverse`. It's one of the most powerful things about it.




--- type:NormalExercise lang:r xp:100 skills:1 key:4df8d2ca44
## dplyr::mutate()


--- type:NormalExercise lang:r xp:100 skills:1 key:68bc51fbf1
## dplyr::group_by()/dplyr::summarize()


--- type:Multiple Choice Exercise lang:r xp:100 skills:1 key:61fad1c7bd
# Chester Ismay's Mantra

What is the difference between `select()` and `filter()?`

*** =instructions
- `select()` works on booleans, whereas `filter()` works on all data types
- `select()` only works after `filter()`
- `select()` works on columns, `filter()` works on rows


