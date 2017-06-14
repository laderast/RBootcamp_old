---
title       : Introduction to dplyr
description : Learn how to manipulate data into a ggplot2 friendly format

--- type:NormalExercise lang:r xp:100 skills:1 key:e0b4f34c19
## Introduction to dplyr

We've been looking at datasets that fit the `ggplot2` paradigm nicely; however, most data we encounter is really messy (missing values), or is a completely different format. In this chapter, we'll look at one of the most powerful tools in the `tidyverse`: `dplyr`, which lets you manipulate data frames.

In particular, we're going to look at six fundamental verbs/actions in dplyr:

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

##length of country category here

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


*** =pre_exercise_code
```{r}
library(fivethirtyeight)
library(dplyr)

data(biopics)
```

--- type:NormalExercise lang:r xp:100 skills:1 key:119cf149a3
## Comparison operators and chaining comparisons

Let's look at the following `filter()` statement:

```{r}
filter(biopics, year_release > 1980 & 
    type_of_subject == "criminal")
```

A couple things to note:
1. We used the comparison operator `>`. The basic comparisons you'll use are `>` (greater than), `<` (less than), `==` (equals to) and `!=` (not equal to) 
2. We also chained on another expression, `type_of_subject == "criminal"` using `&` (and). The other chaining operator that you'll use is `|`, which corresponds to OR. 
3. Chaining expressions is where `filter()` becomes super powerful.

--- type:NormalExercise lang:r xp:100 skills:1 key:c66f0675f2
## The pipe character: `%>%`

We're going to introduce another bit of `dplyr` syntax, the `%>%` operator. 



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




--- type:NormalExercise lang:r xp:100 skills:1 key:6b1d7abb63
## Subsetting Using TRUE/FALSE:

Subsetting using TRUE/FALSE vectors is one of the most important concepts to grasp in R. In this exercise, we'll investigate what happens when we use a Boolean (TRUE/FALSE) vector to subset another vector.

*** =instructions
- Given the vector `weights`, try subsetting using `boolVec`. Assign it to `weightSet`. Find the length of `weightSet` and show the new weightSet variable contents.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#make a vector of weights
weight <- c(10, 20, 50, 30)
boolVec <- c(TRUE, TRUE, FALSE, FALSE)
#subset weight with boolVec here
#assign to weightSet variable

#show length of weightSet

#show weightSet itself
weightSet
```

*** =solution
```{r}
#make a vector of weights
weight <- c(10, 20, 50, 30)
boolVec <- c(TRUE, TRUE, FALSE, FALSE)
#subset weight with boolVec here
weightSet <- weight[boolVec]
#show length of weightSet
length(weightSet)
#show weightSet itself
weightSet
```
*** =sct
```{r}
test_output_contains("weight[boolVec]", incorrect_msg="Use boolVec as an index to weights.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ef91841efd
## Generating Boolean Vectors using <, ==, >:

Now that we know how to use boolean vectors, how can we generate them? We can generate them using the comparison operators: `<`, `>`, `==`, and `!=`.

*** =instructions
- Given the vector `weights`, generate a boolean vector to pick those values that are less than 30. Assign this vector to `smallAnimals`.
- Use `smallAnimals` to subset weights. Assign to `smallAnimalWeights`
- Print out `smallAnimals`.

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
#make a vector of weights
weight <- c(10, 20, 50, 30)

#define smallAnimals as a boolean vector
smallAnimals <- 

#subset weight vector, assign to smallAnimalWeights
smallAnimalWeights <- 

#show length of smallAnimalWeights

#show smallAnimalWeights itself


```

*** =solution
```{r}
#make a vector of weights
weight <- c(10, 20, 50, 30)

#define smallAnimals as a boolean vector
smallAnimals <- weight < 30

#subset weight vector, assign to smallAnimalWeights
smallAnimalWeights <- weight[smallAnimals]

#show length of smallAnimalWeights
length(smallAnimalWeights)

#show smallAnimalWeights itself
smallAnimalWeights
```
*** =sct
```{r}
test_output_contains("weight[smallAnimals]", incorrect_msg="Use smallAnimals to subset the weight vector.")
test_output_contains("length(smallAnimalWeights)", incorrect_msg="show the length of smallAnimalWeights")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:25bf06fe6e
## Quick Review of Boolean Subsetting:
Given the following code, how many values do we expect?

```{r}
heights <- c(100, 104, 106, 96, 85, 103)
heights[heights > 100]
```

*** =instructions
- 2
- 3
- 4

*** =hint
Remember that `>` is non-inclusive. 

*** =sct
```{r}
msg1 = "Try again!"
msg2 = "Correct! We don't include 100 in our returned set, so there are three values: 104, 106, and 103"
test_mc(correct = 2, feedback_msgs=c(msg1, msg2, msg1))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d992e61f9f
## Using Criteria on One Variable to Subset the Other

Since the boolean vector is just a vector of TRUE/FALSE values, we can 

*** =pre_exercise_code
```{r}
# The pre exercise code runs code to initialize the user's workspace.
# You can use it to load packages, initialize datasets and draw a plot in the viewer
load("datasets/module2.RData")
```

## dplyr::filter()

Now that we understand how basic subsetting works, we can understand how `filter` works. 


## Part 1: Vectors

Let's look at a vector in the workspace.

```{r}
weights
```

Note that each slot in a vector can have a unique name.

```{r}
names(weights)
```

Length is one of the properties a vector has.

```{r}
# A vector has a length
length(weights)
```

The `weights` vector can be subsetted. Let's get the very first weight.

```{r}
weights[1]
```

If we pass a sequence to the vector, we can subset. Here we want to grab the
first 5 weights, so we pass a sequence from 1 to 5.

```{r}
# A sequence is actually a vector
1:5

# Get the first 5 weights
weights[1:5]
```

Values in a vector can also be retrieved by name:

```{r}
weights["M15"]
```
--- type:NormalExercise lang:r xp:100 skills:1,3 key:ec51833009
## **QUESTION 1-1**: How would we get the last 5 weights?
```{r}
# Space for your answer here
```

--- type:NormalExercise lang:r xp:100 skills:1,3 key:7aaf63aa0b
## **QUESTION 1-2**: How would we get the first 5 weights in reverse order?
```{r}
# Space for your answer here
```

### Assignment in Vectors

In general, assignment is done by using the `<-` operator. Here we assign the
first 5 elements of the weights vector to `b`.

```{r}
b <- weights[1:5]
b
```

### Operations on Vectors

In general, if you can do an operation on a vector all at once, try to do it (as R is optimized for this).

```{r}
# Run the mean on a vector
meanWeight <- mean(weights)
```

Missing numeric values tend to be coded as `NA`s. Not all functions will handle
`NA`s the same. In module 3, we will talk about the `na.omit()` function.

### Vector Operations
Some operations output a vector of the same length. Let's get the residual sum
of squares.

```{r}
resids <- weights - meanWeight
```
--- type:MultipleChoiceExercise lang:r xp:100 skills:1,3 key:fc3efc69a6
## **QUESTION 1-3**: What happened here? Why can we subtract a scalar from a vector?
```{r}
# Space for your answer here
```
--- type:NormalExercise lang:r xp:100 skills:1,3 key:20d81ff8ed
## **QUESTION 1-4**: What happens when you try to subtract two vectors of unequal
lengths? I.e. `weights[1:5] - weights[1:2]`
```{r}
# Space for your answer here
```

Here's another example of vector operations:
```{r}
# Look at resids
resids

# Square the residuals
sqResids <- resids^2

# Produce residual sum of squares
sum(sqResids)
```

#### Making vectors
In general, you can use the `c()` (concatenate) command to make vectors.

```{r}
vec <- c(1,2,3:5)
vec
```

You can also concatenate a vector to another vector.

```{r}
vec2 <- c(vec,5,5)
vec2
```

--- type:NormalExercise lang:r xp:100 skills:1,3 key:a37cc5436e
## **QUESTION 1-5**: What happens when you try to mix characters and numbers? What
does this tell you about vectors? I.e. `c("This is character data", 1:5)`
```{r}
# Space for your answer here
```

--- type:NormalExercise lang:r xp:100 skills:1,3 key:c00b5cb852
## **QUESTION 1-6**: How might you combine `c()` and a series of names to pull out
only the "M1", "M5", and "M10" weights? This is another kind of subsetting
operation, and we'll cover it more in detail later.
```{r}
# Space for your answer here
```

### Descriptive Statistics and Exploratory Data Analysis

You can run operations on a single vector.
```{r}
# Show mean and quantiles
summary(weights)

# Show a histogram
hist(weights)
```

**QUESTION 1-7**: What can you say about the distribution of weights? i.e., are
they distributed normally?
```{r}
# Space for your answer here
```

### Vector Datatypes and Casting

There are four useful vector datatypes to keep in mind: 

- numeric, 
- character,
- boolean, and
- factor.

#### Numeric

We've already seen numeric vectors:
```{r}
c(1,2,3,5)
```

Another useful way to initialize a vector is by using a sequence:
```{r}
c(1:10)
```

#### Character

Characters can also be put into vectors:
```{r}
c("foo", "bar", "baz")
```

#### Boolean

Boolean vectors are simply `TRUE`/`FALSE`:
```{r}
c(TRUE,TRUE,TRUE,FALSE)
```

To some extent, we can convert between vector types using `as.numeric()` and
`as.character()`.
```{r}
as.character(c(1,2,3,5))
```

#### Factors

Factor vectors can be ordered (by specifying the level argument) or unordered
(by not specifying the level argument). The ordering impacts properties such as
the order the factors are plotted, the order in which factors are reported in
table(), and the way other functions treat them. 

Note that levels have to contain all factors.
```{r}
testVecFac <- factor(c("D", "D", "E"), levels=c("D", "E"))
```

Character and factor vectors are for the most part interchangable for tables.
```{r}
testVecChar <- c("A", "B", "C","C", "D")
testVecFac <- factor(c("A", "B", "C","C", "D"))

table(testVecChar)
table(testVecFac)
```

However, when we use numbers as factors, be aware that `as.numeric()` doesn't
work. This is due to the internal representation of factors as integers.
```{r}
testVecFac <- factor(c(1,5,5,56), levels=c(1,5,56))
as.numeric(testVecFac)
```

Instead, we must first cast the vector to character, and then numeric:
```{r}
as.numeric(as.character(testVecFac))
```

A final note: you may notice that `read.table()` will treat any strings as
factors when loading. You can override this behavior by setting the argument
`stringsAsFactors` to `FALSE` (refer to `?read.table` for more information.)
whether you will want your strings represented as characters or factors is
dependent on the application.  

```{r}
testTable <- read.table("mouseData.txt", header=TRUE)
summary(testTable)

# You can always cast a factor as a string, remember
testTable$Strain <- as.character(testTable$Strain)
summary(testTable)
```

Let's try to read the table as characters.
```{r}
testTable2 <- read.table("mouseData.txt", stringsAsFactors=FALSE, header=TRUE)

# Note the numeric property of `Weight` is preserved.
summary(testTable2)
```


## Some Basics about Data Frames
First things first. Let's look at a dataset in R. 


