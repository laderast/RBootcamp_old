---
title       : The Magic of `ggplot2`
description : Learn how ggplot turns variables into statistical graphics

--- type:NormalExercise lang:r xp:100 skills:1 key:8323fcbca1
## Quick Data Frame Review

Remember that a `data.frame` is basically a table-like format which has the following properties: 

- Columns can each have multiple types (`numeric`, `character`, `boolean`, `factor`)
- Columns are called "variables"
- Rows correspond to a single observation (ideally)
- Can be subset or filtered based on criteria

Individual variables within a `data.frame` can be accessed with the `$` operator. We won't use this very often, as the `tidyverse` lets us access the names directly, as you'll see.

*** =instructions
Run `colnames()` and `head()` on the `gap1992` data to see what's in each column. Confirm that the `length()` of the `pop` column is equal to the `lifeExp` column.

*** =pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)
```

*** =sample_code
```{r}
head(gap1992)

##check if pop column is same length as lifeExp column
##show the length of the pop column
length() 
##show the length of the lifeExp column
length()
```

*** =solution
```{r}
head(gap1992)

##check if pop column is same length as lifeExp column
##show the length of the pop column
length(gap1992$pop) 
##show the length of the lifeExp column
length(gap1992$lifeExp)
```

*** =sct
```{r}
test_function("length", args="x", index=1, incorrect_msg="use gap1992$pop")
test_function("length", args="x", index=2, incorrect_msg-"use gap1992$lifeExp")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:d599f92ec8
## Thinking about aesthetics
Now that we've reviewed data frames, we can get to the fun part: making graphs.

The first thing we are going to is think about how we represent variables in a plot. 

We need to figure out how to visually represent a variable in our dataset to some sort of aesthetic property.

Take a look at this graph. What variable is mapped to `y`, and what is mapped to `x`, and what is mapped to color?

***=pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, size=pop, color=continent)) +
  geom_point() + ggtitle("Gapminder for 1992")
```

*** =instructions
- x = gdpPercap, y = log(lifeExp), color = continent
- x = continent, y = year, color = pop
- y = lifeExp, x = log(gdpPercap), color = continent

*** =hint
Look at the y-axis.

*** =sct
```{r}
msg1 = "You have things reversed, and you're taking the log of the wrong variable"
msg2 = "Wrong variables. Go back and look"
msg3 = "Correct! We are displaying lifeExp as our y variable and log(gdpPercap) as our x variable"

test_mc(correct = 3, feedback_msgs=c(msg1, msg2, msg3))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ef91841efd
## Mapping variables to produce geometric plots

A statistical graphic consists of:

+ A `mapping` of variables in `data` to
+ `aes()`thetic attributes of
+ `geom_`etric objects.

In code, this is translated as:

```{r}
ggplot(data = gap1992, mapping = aes(x = log(gdpPercap), y=log(pop))) +
  geom_point()
```

Let's take the code apart. A `ggplot2` call always starts with the `ggplot()` function. In this function, we need two things:

1. `data` - in this case, `gap1992`.
2. `mapping` - An aesthetic mapping, using the `aes()` function. 

In order to map our variables to aesthetic properties, we will need to use `aes()`. Here we map `x` to `log(gdpPercap)` and `y` to `log(pop)`.

Finally, we can superimpose our geometry on the plot using `geom_point()`.

*** =instructions
Based on the graph, map the appropriate variables to the x, y, and color aesthetics. Run your plot.

*** =pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, size=pop, color=continent)) +
  geom_point() + ggtitle("Gapminder for 1992")
```

*** =sample_code
```{r}
ggplot(data = gap1992, 
    mapping = aes(
      x = , 
      y = , 
      color = )) + 
geom_point()
```
*** =hint
Look at the graph. If you need the variable names, you can always 
use `head()` on the gap1992 dataset.

*** =solution
```{r}
ggplot(data=gap1992, 
    mapping = aes(
      x = log(gdpPercap), 
      y = lifeExp, 
      color = continent)) + 
geom_point()
```

*** =sct
```{r}
test_output_contains("log(gdpPercap)", 
        incorrect_msg="You need to log transform one of the variables.")
test_output_contains("lifeExp", 
        incorrect_msg="you need to map lifeExp to something")
test_output_contains("continent", 
        incorrect_msg="you need to map continent to something. What could that be?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:f0a09d682e
## Points versus lines

The great thing about `ggplot2` is that it's easy to swap representations. 
Instead of x-y points, we can plot the data as a line graph by swapping `geom_line()`
for `geom_point()`.

*** =instructions
Change the `geom_point()` in the following graph to `geom_line()`. What happened?

*** =pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)
```

*** =sample_code
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
  geom_point() 
```

*** =solution
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
  geom_line() 
```

*** =sct
```{r}
test_function("geom_line", incorrect_msg="you need to change the geom")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:13ea4fbf3d
## Geoms are layers on a ggplot

We are not restricted to a single geom on a graph! You can think of geoms
as layers on a graph. Thus, we can use the `+` symbol to add geoms to our
base `ggplot()` statement.

*** =instructions
Add both `geom_line()` and `geom_point()` to the following ggplot. 

*** =pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)
```

*** =sample_code
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +

```

*** =solution
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
  geom_line() + geom_point()
```

*** =sct
```{r}
test_function("geom_line", incorrect_msg="you need to change the geom")
test_function("geom_point", incorrect_msg="you need to change the geom")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:349a622cb7
## Quick review about ggplot2

What does the `+` in a `ggplot` statement do?

```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
  geom_line() + geom_point()
```

*** =instructions
- adds one `data.frame` to another `data.frame` 
- allows you to chain data and geoms together
- allows you to add variables together in a `data.frame`

*** =hint
`+` combines things, but doesn't add them together

*** =sct
```{r}
msg1 = "This is not the case. Go back and look at the ggplot code."
msg2 = "Correct! This is how we can add data and layer geoms together"
msg3 = "Look at the ggplot code and see if we are manipulating data or not"
test_mc(correct = 2, feedback_msgs=c(msg1, msg2, msg3))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:6e0ba88ae9
## "gg" is for *G*rammar of *G*raphics

When Hadley Wickham built `ggplot2`, he had Wilkinson's ["Grammar of Graphics"](http://www.springer.com/us/book/9780387245447) in mind. In this book, Wilkinson et. al. decomposed all statistical graphics of having a number of graphical elements.

`ggplot2` directly maps these concepts of a [grammar of graphics to plotting R Data](http://vita.had.co.nz/papers/layered-grammar.pdf). 


--- type:NormalExercise lang:r xp:300 skills:1 key:01ef5c54c5
## Final Challenge: Recreate this Gapminder Plot

Your final challenge is to completely recreate this graph using the `gap1992` data.

***=pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, size=pop, color=continent)) +
  geom_point() + ggtitle("Gapminder for 1992")
```

*** =instructions
- If you need to remember variable names, you can always call `head(gap1992)` in the console.
- Recreate the above graphic by mapping the right variables to the right aesthetic elements. Remember, you can try plots out in the console.

*** =sample_code
```{r}
ggplot(gap1992, aes(x = , 
    y = , 
    color = ,
    size =
    )) + ggtitle("Gapminder for 1992") +
```

*** =solution
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), 
    y = lifeExp, 
    color = continent,
    size = pop
    )) + ggtitle("Gapminder for 1992") + 
    geom_point()
```

