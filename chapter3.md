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



--- type:NormalExercise lang:r xp:100 skills:1 key:0643d8c567
## A Little Bit about assignment

In order to do the following exercises, we need to learn a little bit about how to assign 
the output of a function to a variable.

For example, we can assign the output of the operation `1 + 2` to a variable called `sumOfTwoNumbers`
using the `<-` operator. This is called the `assignment` operator.

You can also use `=` to assign a value to a variable, but I find it makes my code a bit confusing, because
there is also `==`, which tests for equality.
```{r}
sumOfTwoNumbers <- 1 + 2
```
Once we have something assigned to a variable, we can use it in other expressions:

```{r}
sumOfThreeNumbers <- sumOfTwoNumbers + 3
```
This is the bare basics of assignment. We'll use it in the next exercises to evaluate the output
of our `dplyr` cleaning.

*** =instructions
Assign `newValue` the value of `10`. Then use `newValue` to calculate the value of `multValue` by
calculating `newValue * 5`. Show `multValue`.

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
##assign newValue
newValue <-
## use newValue to calculate multValue
multValue <- 
##show multValue
multValue
```

*** =solution
```{r}
##assign newValue
newValue <- 10
## use newValue to calculate multValue
multValue <- newValue * 5
##show multValue
```

*** =sct
```{r}
success_msg("Now you know how to assign variables and use them in expressions. Well done!")
test_object("newValue", incorrect_msg = "Not quite. Did you assign `newValue` to 10?")
test_object("multValue", incorrect_msg = "Not quite. Did you use `newValue` in your expression?")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:cca48c6abd
## Let's look at some data and ways to manipulate it.

We're going to use the `biopics` dataset in the `fivethirtyeight` package to do learn `dplyr`. This is a dataset of 761 different biopic movies.

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
test_function("levels", incorrect_msg = "you need to use the levels() function on biopics$country")

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

+ The first argument to `filter()` is the dataset. We'll see another variation of this in a second.
+ For those who are used to accessing `data.frame` variables by `$`, notice we don't have to use `biopics$country`. Instead, we can just use the variable name `country`.
+ Our filter statement uses `==`. Remember that `==` is an equality test, and `=` is to assign something. (confusing the two will happen to you from time to time.)

*** =instructions
Filter `biopics` so that it only shows `Criminal` movies (you'll have to use the `type_of_subject` variable in `biopics`. Show how many rows are left using `nrows(crimeMovies)`.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
#add your filter statement here
crimeMovies <- filter()

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

+ We used the comparison operator `>`. The basic comparisons you'll use are `>` (greater than), `<` (less than), `==` (equals to) and `!=` (not equal to) 
+ We also chained on another expression, `type_of_subject == "Criminal"` using `&` (and). The other chaining operator that you'll use is `|`, which corresponds to OR. 
+ Chaining expressions is where `filter()` becomes super powerful. However, it's also the source of headaches, so you will need to carefully test your chain of expressions.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
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

Which statement should be the larger subset? Try them out in the console if you're not sure.

*** =instructions
- `filter(biopics, year_release > 1980 & type_of_subject == "criminal")`
- `filter(biopics, year_release > 1980 | type_of_subject == "criminal")`

*** =hint
Think about the difference between `|` (or) and `&` (and)

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sct
```{r}
success_msg("Good Job! Let's move on.")
test_mc(correct = 2, feedback_msgs = c("Nope. This should be the smaller subset (because you're applying both criteria)",
"Correct! Because you accept either criteria, this is the larger subset"))
```
--- type:NormalExercise lang:r xp:100 skills:1 key:7d9b5ddfe5
## dplyr::mutate()

`mutate()` is one of the most useful `dplyr` commands. You can use it to transform data 
(variables in your `data.frame`) and  add it as a new variable into the `data.frame`. 
For example, let's calculate the total `box_office` divided by the `number_of_subjects` 
to normalize our comparison as `normalized_box_office`: 

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
options(tibble.width = Inf)
```

*** =sample_code
```{r}
#assign new variable race_and_gender here using mutate()
biopics2 <- mutate()

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
you can use them right away, in the same `mutate` statement. For example, look at this code:

```{r}
biopics %>% mutate(box_office_year = year_release * box_office, box_office_subject = paste0(box_office_year, subject))
```

Notice that we first defined `box_office_year` in the first part of the `mutate()` statement,
and then used it right away to define a new variable, `box_office_subject`. 

*** =instructions
Define another variable called `box_office_year_subject_number` in the same `mutate()` statement by taking 
`box_office_year` and dividing it by `number_of_subjects`. Assign the output to `mutatedBiopics`.

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
mutatedBiopics <- biopics %>% mutate(box_office_year = year_release * box_office, box_office_subject = paste0(box_office_year, subject))
```

*** =solution
```{r}
mutatedBiopics <- biopics %>% mutate(box_office_year = year_release * box_office, box_office_subject = paste0(box_office_year, subject), 
box_office_year_subject_number = box_office_year/number_of_subjects)
```

*** =sct
```{r}

```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:bfd4f10b15
## Another Use for `mutate()`

What is this statement doing? Try it out in the console if you're not sure.

```{r}
mutate(biopics, subject= paste(subject, year_release))
```

*** =instructions
- We are defining a brand-new variable with the same name in our dataset and keeping the old variable as well
- We are processing the variable `subject` and saving it in place

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sct
```{r}
success_msg("Great! Now you know you can also use `mutate()` to process variables in place.")
msg1 = "Try it out. Did we add another variable?"
msg2 = "Correct! We're processing `subject` in place."
test_mc(correct=2, feedback_msgs = c(msg1, msg2))
```
--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:3b247ce89c
## The difference between `filter()` and `mutate()`

What is the difference between these two statements? Try them out in the console if you're not sure.

```{r}
biopics %>% filter(year_release > 1998)
```

```{r}
biopics %>% mutate(isNewer = year_release > 1998)
```

*** =instructions
- The first statement should have a larger number of rows than the second one
- The first statement filters the data, whereas the second statement defines a new boolean variable.
- The second statement is more confusing.

*** =hint
Compare the number of rows and the output of each of these statements.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sct
```{r}
success_msg("Yes! I'm glad you understand the difference.")
msg1 = "Not the case! Try comparing the number of rows."
msg2 = "Yes, this is correct! We're identifying a new variable that we can use to flag the data."
msg3 = "It is a little confusing, but think it out."
test_mc(correct = 2, feedback_msgs = c(msg1, msg2, msg3))
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
As: I took the `biopics` data, THEN 
I filtered it down with the `race_known == "Known"` criteria and THEN 
I defined a new variable called `poc_code`.

Note that `filter()` doesn't have a `data` argument, because the `data` is `piped` into `filter()`. Same thing for `mutate()`.

`%>%` allows you to chain multiple verbs in the `tidyverse`. It's one of the most powerful things about the `tidyverse`. In fact,
having a standardized chain of processing actions is called a **pipeline**. Making pipelines for a data
format is great, because you can apply that pipeline to incoming data that has the same formatting and have it output in a `ggplot2` friendly format.

*** =instructions
Use `%>%` to chain a `filter` command (`country=="US"`) with another `filter` 
command (`grepl("Richard",lead_actor_actress)`). Assign the output to `richardUS`. 

In this second `filter`, we search for any instances of `"Richard"` in 
`lead_actor_actress` using `grepl`.

How many instances of US Richards are there? 

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
richardUS <- 
    biopics %>%
    
#show number of rows in richardUS

```

*** =solution
```{r}
richardUS <- 
    biopics %>% filter(country=="US") %>% 
        filter(grepl("Richard",lead_actor_actress))

#show number of rows in richardUS    
nrow(richardUS)
```

*** =sct
```{r}
success_msg("Great! Now you know how to chain actions into a pipeline. Use `%>%` wisely.")
test_function("filter", incorrect_msg = "did you use `filter()`?")
test_object("richardUS", incorrect_msg = "Not quite. Go back and look at your `dplyr` statement.")
test_function("nrow", incorrect_msg = "Be sure to use `nrow(richardUS)`")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:8c5431911a
## group_by()/summarize()

`group_by()` doesn't do anything by itself. But when combined with `summarize()`, you can 
calculate metrics (such as `mean`, `max` - the maximum, `sd` - the standard deviation) across groups. For example:

```{r}
countryMeans <- biopics %>% 
                    filter(!is.na(box_office)) %>% 
                    group_by(country) %>% 
                    summarize(mean_box_office = mean(box_office))
```

Here we want to calculate the mean `box_office` by `country`. However, in order to do that, we first need to remove
any rows that have `NA` values in `box_office` that may confound our calculation.

*** =instructions
Let's ask a tough question. Is there a difference between mean `box_office` 
between the two `subject_sex` categories? 

First use `filter()` to remove the NA values. Then, use `group_by()` and `summarize()` to 
calculate the mean `box_office` by `subject_sex`, naming the summary
variable as `mean_bo_by_gender`.  Assign the output to `gender_box_office`. 

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
gender_box_office <- biopics %>% 
    filter() %>%
    group_by() %>% 
    summarize(mean_bo_by_gender=)
    
##show gender_box_office
head(gender_box_office)
```

*** =solution
```{r}
gender_box_office <- biopics %>% 
    filter(!is.na(box_office)) %>%
    group_by(subject_sex) %>% 
    summarize(mean_bo_by_gender=mean(box_office))

##show gender_box_office
head(gender_box_office)
```

*** =sct
```{r}
success_msg("Yes! You're summarizing like crazy! Let's move on.")
test_object("gender_box_office", incorrect_msg = "almost, but not quite! Check each dplyr verb to make sure it's correct")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:6586c80668
## arrange()

`arrange()` lets you sort by a variable. If you provide multiple variables, the variables are 
arranged within each other. For example:

```{r}
biopics %>% arrange(country, year_release)
```

This statement will sort the data by `country` first, and then within each `country` category, 
it will sort by `year_release`.

*** =instructions
Sort `biopics` by `year_release` then by `lead_actor_actress`. Assign the output to `biopics_sorted`.

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
biopics_sorted <- biopics %>%
```

*** =solution
```{r}
biopics_sorted <- biopics %>% arrange(year_release, lead_actor_actress)
```

*** =sct
```{r}
success_msg("Now you know how to sort data using dplyr. Nifty, eh?")
test_object("biopics_sorted", incorrect_msg = "Not quite. Make sure you are arranging variables in order")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:86c314b14c
## select()

The final verb we'll learn is `select()`. `select()` allows you to 1) extract columns, 
2) reorder columns or 3) remove columns from your data, as well as 4) rename your data. 
For example, look at the following code:

```{r}
biopics %>% select(movieTitle=title, box_office)
```
Here, we're just extracting two columns (`title_of_movie`, `box_office`). Notice we also renamed
`title` to `movieTitle`.

*** =instructions
Use `select` to extract the following variables: `title` (rename it `movieTitle`), 
`box_office` and `subject_sex` and assign them to a new table called `threeVarTable`

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
threeVarTable <- biopics %>% select()

```

*** =solution
```{r}
threeVarTable <- biopics %>% select(movieTitle=title, box_office, subject_sex)
```

*** =sct
```{r}
success_msg("Great! Now you know how to select columns from your tables.")
test_object("threeVarTable", incorrect_msg = "Not quite. Remember names and order matters.")
```

--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:f9e951b711
## Chester Ismay's Mantra

What is the difference between `select()` and `filter()?`

*** =instructions
- `select()` works on booleans, whereas `filter()` works on all data types
- `select()` only works after `filter()`
- `select()` works on columns, `filter()` works on rows

*** =pre_exercise_code
```{r}

```

*** =sct
```{r}
success_msg("Welcome to the cult of dplyr! Your secret decoder ring is in the mail.")
msg1 <- "Nope. Both of these verbs don't care what data type you use."
msg2 <- "Not true. You can use `filter()` and `select()` in any order!"
msg3 <- "Yup. Repeat this 10 times every day so you know the difference."
test_mc(correct = 3, feedback_msgs = c(msg1, msg2, msg3))
```

--- type:NormalExercise lang:r xp:300 skills:1 key:5bbc97ed1b
## Putting it all together: Challenge 1

*** =instructions
For the `biopics` data, `filter()` the data so that we only cover movies from 2000 to 2014. Then 
use `mutate()` to code a new variable, `box_office_per_subject`. `filter` to remove the NA values in
`box_office_per_subject` and `group_by()` `country` and `summarize()` the mean `box_office_per_subject`
by `country` as `bops_by_country`. Use `arrange()` to sort by `bops_by_country`.  
Assign the output of this chain to `biopics_by_country`.

*** =hint
Remember you can debug a `dplyr` chain by running each step incrementally.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
biopics_by_country <- biopics %>%
    filter() %>%
    mutate() %>%
    filter() %>%
    group_by() %>%
    summarize() %>%
    arrange()
```

*** =solution
```{r}
biopics_by_country <- biopics %>%
    filter(year_release >= 2000 & year_release <= 2014) %>%
    mutate(box_office_per_subject = box_office / number_of_subjects) %>%
    filter(!is.na(box_office_per_subject)) %>%
    group_by(country) %>%
    summarize(bops_by_country = mean(box_office_per_subject)) %>%
    arrange(bops_by_country)
```

*** =sct
```{r}
success_msg("Wow! You've really come really far. I bestow upon you the title of `dplyr` ninja!")
test_object("biopics_by_country", incorrect_msg = "Not quite. Check your code.")
```


--- type:NormalExercise lang:r xp:300 skills:1 key:a4c8d46d41
## Challenge 2: Show your stuff

Answer the question: Do movies where we know the race is known (`race_known` == TRUE) make more 
money than movies where the race is not known (`race_known`== FALSE) grouped by country? 
Which `race_known`/`country` combination made the highest amount of money?

*** =instructions
You'll need to do a `filter` step first to remove `NA` values from `box_office` before you do 
anything. Then think of what variables you need to `group_by`. Finally, what do you need to
`summarize` (assign the value to `mean_box_office`) and `arrange` on (don't forget to use `desc`!)? 

Assign the output to `race_country_box_office`.

*** =hint

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
race_country_box_office <- biopics %>%
        
```

*** =solution
```{r}
race_country_box_office <- biopics %>%
    filter(!is.na(box_office)) %>%
    group_by(race_known, country) %>%
    summarize(mean_box_office=mean(box_office)) %>%
    arrange(desc(mean_box_office))
```

*** =sct
```{r}
success_msg("Your `dplyr` skills are ever improving!")
test_object("race_country_box_office", incorrect_msg = 
    "Not quite. Think about the order in which you need to do these operations.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:b7b6b449de
## Challenge 3: Putting together what we know about ggplot2 and dplyr

Now we're cooking with fire. You can directly pipe the output of a `dplyr` pipeline
into a `ggplot2` statement. For example:

```{r}
biopics %>%
    filter(year_release >= 2000 & year_release <= 2014) %>%
    mutate(box_office_per_subject = box_office / number_of_subjects) %>%
    ggplot(aes(x = year_release, y=box_office_per_subject)) +
    geom_point()
```

Note that we use `%>%` to pipe our statement into the `ggplot()` function. The
tricky thing to remember is that everything after the `ggplot()` is connected with
`+`, and not `%>%`. 

Also note: we don't assign a `data` variable in the `ggplot()` statement. We are piping
in the data. 

*** =instructions
Are you sick of `biopics` yet? I promise this is the last time we use this dataset.

First filter `biopics` to have `year_release` < 1990 and remove `NA` values. 
Then pipe that into a `ggplot()` statement that plots an x-y plot of `box_office` 
(use `geom_point()`) where `x=year_release` and 
`y=log(box_office)`. Color the points by `person_of_color`.

*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(ggplot2)
library(dplyr)

data(biopics)
biopics$country <- factor(biopics$country)
options(tibble.width = Inf)
```

*** =sample_code
```{r}
biopics %>%
```

*** =solution
```{r}
biopics %>% filter(year_release < 1990) %>%
    ggplot(aes(x=year_release, y=log(box_office), color=person_of_color)) +
    geom_point()
```

*** =sct
```{r}
success_msg("Now you know how to chain dplyr statements straight into a ggplot!")
test_ggplot(check_aes = TRUE, aes_fail_msg = "Make sure you're mapping the right variables")
test_ggplot(check_data = TRUE, data_fail_msg = "Did you do the filter step correctly?")
```
--- type:NormalExercise lang:r xp:0 skills:1 key:749b2485e7
## What you learned in this chapter

- How to use `%>%` (the pipe)
- `dplyr::filter()`
- `dplyr::mutate()`
- `dplyr::group_by()/dplyr::summarize()`
- `dplyr::arrange()`
- `dplyr::select()`
- How to put it all together!
- Chester's Mantra

*** =instructions
Just move on to the next chapter. Good job for making it through this chapter! You're well on your way
to becoming a `tidyverse` ninja!

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
