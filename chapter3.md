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

*** =instructions
Just move on to the next exercise!

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
--- type:NormalExercise lang:r xp:100 skills:1 key:cca48c6abd
## Let's look at some data and ways to manipulate it.

We're going to use the `biopics` dataset in `fivethirtyeight` to do learn `dplyr`. This is a dataset of 761 different biopic movies.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
```

*** =instructions
Run a `summary` on the `biopics` dataset. It's already loaded up for you. How many categories are in the `country` variable?

*** =hint
Use the `levels()` function.

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
length(levels(biopics$country))
```

*** =sct
```{r}
success_msg("Nice work!")
test_function("summary", incorrect_msg = "you need to use the summary() function on biopics")
test_function("levels", incorrect_msg = "you need to use the levels() function on biopics")

```
--- type:NormalExercise lang:r xp:100 skills:1 key:1929755973
## dplyr::filter()

`filter()` is a very useful `dplyr` command. It allows you to subset a `data.frame` based on variable criteria.

For example, if we wanted to subset `biopics` to those movies that were made in the `UK` we'd use the following statement:

```{r}
#subset the data using filter
biopicsUK <- filter(biopics, country=="UK")
#confirm that we have subsetted correctly
biopicsUK
```

Three things to note here: 

    1. The first argument to filter is always the dataset. We'll see another variation in a second.
    
    2. For those who are used to accessing `data.frame` variables by `$`, notice we don't have to use `biopics$country`. Instead, we can just use the variable name `country`.

    3. Our filter statement uses `==`. Remember that `==` is an equality test, and `=` is to assign something. (confusing the two will happen to you from time to time.)

*** =instructions
Filter `biopics` so that it only shows `Criminal` movies (you'll have to use the `type_of_subject` variable in `biopics`. Show how many rows are left.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
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
success_msg("Good job! That's the right number.")
test_function("filter", incorrect_msg = "Did you try using the `filter()` function?")
test_object("crimeMovies", incorrect_msg = "Your filter statement is incorrect. Try again.")
test_function("nrow", incorrect_msg = "You should use `nrow()` to show the number of rows in `crimeMovies`")

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

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
```

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
nrow(crimeFilms)
```

*** =sct
```{r}
success_msg("Good job! Now you know how to chain comparisons!")
test_object("crimeFilms", incorrect_msg = "Not quite. Did you add your comparison correctly?")
test_function("nrow", incorrect_msg = "Show the number of rows in the crimeFilms using `nrow()`")
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:e6843338b8
## Quick Quiz about Chaining Comparisons

Which statement should be the larger subset?

*** =instructions
- `filter(biopics, year_release > 1980 & type_of_subject == "criminal")`
- `filter(biopics, year_release > 1980 | type_of_subject == "criminal")`

*** =hint
Think about the difference between `|` (or) and `&` (and)

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
success_msg("Good Job! Let's move on.")
test_mc(correct = 2, feedback_msgs = c("Nope. This should be the smaller subset (because you're applying both criteria)",
"Correct! Because you accept either criteria, this is the larger subset"))
```
--- type:NormalExercise lang:r xp:100 skills:1 key:7d9b5ddfe5
## dplyr::mutate()

`mutate()` is one of the most useful `dplyr` commands. You can use it to transform data and 
add it as a new variable into the `data.frame`. For example, let's calculate the total `box_office`
divided by the `number_of_subjects` to normalize our comparison as `normalized_box_office`: 

```{r}
biopics2 <- mutate(biopics, normalized_box_office = box_office/number_of_subjects)
```
What did we do here? First, we used the `mutate()` function to add a new column into our 
`data.frame` called `normalized_box_office`. This new variable is calculated per row by dividing
`box_office` by `number_of_subjects`.

*** =instructions
Try defining a new variable `race_and_gender` by pasting together `subject_race` and `subject_sex`
into a new `data_frame` called `biopics2`. Show the first few rows using `head()` so you can
confirm that you added this new variable correctly.

Remember, you can use the `paste()` function to paste two strings together.

*** =hint
`paste(subject_race, subject_sex)`

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
```

*** =sample_code
```{r}
#assign new variable race_and_gender here using mutate()
biopics2 <- 

#show first rows of biopics2 using head()

```

*** =solution
```{r}
#assign new variable race_and_gender here using mutate()
biopics2 <- mutate(biopics, race_and_gender = paste(subject_race, subject_sex))

#show first rows using head
head(biopics2)
```

*** =sct
```{r}
success_msg("Great! Now you know how to use `mutate()`!")
test_object("biopics2", incorrect_msg = "Not quite. Did you use `paste()`?")
test_function("head", incorrect_msg = "Almost there. Did you use `head()`")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:dfc4eaade1
## You can use mutated variables right away!

The nifty thing about `mutate()` is that once you define the variables in the statement,
you can use them right away. For example, look at this code:

```{r}

```


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
--- type:NormalExercise lang:r xp:100 skills:1 key:3bba8ddfc5
## Another Use for mutate(): Changing a column in place


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
--- type:NormalExercise lang:r xp:100 skills:1 key:c66f0675f2
## The pipe character: `%>%`

We're going to introduce another bit of `dplyr` syntax, the `%>%` operator. `%>%` is called a `pipe` operator. 
You can think of it as being similar to the `+` in a `ggplot2` statement.

What `%>%` does is that it takes the output of one statement and makes it the input of the next statement. When 
I'm describing it, I think of it as a "THEN". For example, I read the following expression

```{r}
biopics %>% filter(race_known == "Known") %>%
    mutate(poc_code = as.numeric(person_of_color))
```
As: I took the `biopics` data, THEN I filtered it down with the `race_known == "Known"` criteria and 
THEN I defined a new variable called `poc_code`.

`%>%` allows you to chain multiple actions in the `tidyverse`. It's one of the most powerful things about it. In fact,
having a standardized chain of processing actions is called a **pipeline**. Having established a pipeline for a data
format is great, because you can apply that pipeline to incoming data and have it output as a standardized format.

*** =instructions
Use `%>%` to chain a `filter` command (`country=="Canada"`) with a `mutate` 
command (`subject_is_richard=grepl("Richard",lead_actor_actress)`). 

How many instances of Canadian Richards are there?

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
```

*** =sample_code
```{r}
richardCanada <- 
    biopics %>%
```

*** =solution
```{r}
richardCanada <- 
    biopics %>% filter(country=="Canada") %>% 
        mutate(subject_is_richard=grepl("Richard",lead_actor_actress))
    
nrow(richardCanada)
```

*** =sct
```{r}

```


--- type:NormalExercise lang:r xp:100 skills:1 key:8c5431911a
## dplyr::group_by()/dplyr::summarize()

`group_by()` doesn't do anything by itself. But when combined with `summarize()`, you can 
calculate metrics (such as `mean`, `max`, `sd`) across groups. For example:

```{r}
countryMeans <- biopics %>% filter(!is.na(box_office)) %>% 
    group_by(country) %>% summarize(mean_box_office = mean(box_office))
```

Here we want to calculate the mean box office 

*** =instructions
Let's ask a tough question. Is there a difference between mean `box_office` between `subject_sex`?

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
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
--- type:NormalExercise lang:r xp:100 skills:1 key:86c314b14c
## dplyr::select()


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

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:f9e951b711
## Chester Ismay's Mantra

What is the difference between `select()` and `filter()?`

*** =instructions
- `select()` works on booleans, whereas `filter()` works on all data types
- `select()` only works after `filter()`
- `select()` works on columns, `filter()` works on rows

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}

```
--- type:NormalExercise lang:r xp:0 skills:1 key:749b2485e7
## What you learned in this chapter

- How to use six of the main `dplyr` verbs
- Chester's Mantra
- 

*** =instructions
Just move on to the next chapter. Good job for making it through this chapter! You're well on your way
to becoming a tidyverse ninja!

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
