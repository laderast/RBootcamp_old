---
title       : The magic of `ggplot2`
description : How ggplot turns variables into graphs

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:d599f92ec8
## Thinking about aesthetics
The first thing we are going to is think about how we represent variables in a plot. We need to figure out what 

Take a look at this graph. What variable is mapped to y, and what is mapped to x?

=pre_exercise_code
```{r}
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

#Space for your answer here.
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, size=pop, color=continent)) +
  geom_point()
```

*** =instructions
- y = lifeExp, x = log(gdpPercap)
- x = gdpPercap, y = log(lifeExp)
- x= continent, y = year

*** =hint
Look at the y-axis.

*** =sct
```{r}
msg1 = "Correct! We are displaying lifeExp as our y variable and log(gdpPercap) as our x variable"
msg2 = "You have things reversed, and you're taking the wrong log"
msg3 = "Wrong variables. Go back and look"
test_mc(correct = 1, feedback_msgs=c(msg1, msg2, msg3))
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
**QUESTION 1-1**: How would we get the last 5 weights?
```{r}
# Space for your answer here
```

--- type:NormalExercise lang:r xp:100 skills:1,3 key:7aaf63aa0b
**QUESTION 1-2**: How would we get the first 5 weights in reverse order?
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
**QUESTION 1-3**: What happened here? Why can we subtract a scalar from a vector?
```{r}
# Space for your answer here
```
--- type:NormalExercise lang:r xp:100 skills:1,3 key:20d81ff8ed
**QUESTION 1-4**: What happens when you try to subtract two vectors of unequal
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
**QUESTION 1-5**: What happens when you try to mix characters and numbers? What
does this tell you about vectors? I.e. `c("This is character data", 1:5)`
```{r}
# Space for your answer here
```

--- type:NormalExercise lang:r xp:100 skills:1,3 key:c00b5cb852
**QUESTION 1-6**: How might you combine `c()` and a series of names to pull out
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


