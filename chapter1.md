---
title       : The Magic of `ggplot2`
description : Learn how ggplot turns variables into statistical graphics

--- type:NormalExercise lang:r xp:100 skills:1 key:8323fcbca1
## Quick Data Frame Review

Remember that a `data.frame` is basically a table-like format which has the following properties: 

- Columns can each have a different type (`numeric`, `character`, `boolean`, `factor`)
- Columns are called "variables"
- Rows correspond to a single observation (ideally)
- Can be subset or filtered based on criteria

Individual variables within a `data.frame` can be accessed with the `$` operator (such as `gap1992$pop`). We won't use this very often, 
as the `tidyverse` lets us access the variables without it, as you'll see.

*** =instructions
Run `colnames()` and `head()` on the `gap1992` data to see what's in each column. Confirm that the 
`length()` of the `pop` column is equal to the `lifeExp` column.

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
##run colnames here on gap1992


##check if pop column is same length as lifeExp column
##show the length of the pop column
length() 
##show the length of the lifeExp column
length()
```

*** =solution
```{r}
head(gap1992)
##run colnames here on gap1992
colnames(gap1992)

##check if pop column is same length as lifeExp column
##show the length of the pop column
length(gap1992$pop) 
##show the length of the lifeExp column
length(gap1992$lifeExp)
```

*** =sct
```{r}
success_msg("Great! You remembered some basics about `data.frame`s! Let's move on.")
test_function("colnames", incorrect_msg = "did you use colnames(gap1992)?")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:d599f92ec8
## Thinking about aesthetics
Now that we've reviewed data frames, we can get to the fun part: making graphs.

The first thing we are going to is think about how we represent variables in a plot. 

How do we visually represent a variable in our dataset? Take a look at this graph. What variable is mapped to `y`, and what is mapped to `x`, and what is mapped to `color`?

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
msg2 = "Wrong variables. Go back and look at what's being mapped"
msg3 = "Correct! We are displaying lifeExp as our y variable and log(gdpPercap) as our x variable"

test_mc(correct = 3, feedback_msgs=c(msg1, msg2, msg3))
```





--- type:NormalExercise lang:r xp:100 skills:1 key:bfe1375688
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

Let's take the above code apart. A `ggplot2` call always starts with the `ggplot()` function. In this function, we need two things:

1. `data` - in this case, `gap1992`.
2. `mapping` - An aesthetic mapping, using the `aes()` function. 

In order to map our variables to aesthetic properties, we will need to use `aes()`, which is our `aes()`thetic mapping function. In our example, we map `x` to `log(gdpPercap)` and `y` to `log(pop)`.

Finally, we can superimpose our geometry on the plot using `geom_point()`.


*** =instructions
Based on the graph, map the appropriate variables to the x, y, and color aesthetics. Run your plot.

*** =hint
Look at the graph. If you need the variable names, you can always 
use `head()` on the gap1992 dataset.


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
      color = 
      )) + 
geom_point()
```

*** =solution
```{r}
ggplot(data=gap1992, 
    mapping = aes(
      x = log(gdpPercap), 
      y = lifeExp, 
      color = continent
      )) + 
geom_point()

```

*** =sct
```{r}
#test_output_contains("log(gdpPercap)", 
#        incorrect_msg="You need to log transform one of the variables.")
#test_output_contains("lifeExp", 
#        incorrect_msg="you need to map lifeExp to something")
#test_output_contains("continent", 
#        incorrect_msg="you need to map continent to something. What could that be?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:f0a09d682e
## Points versus lines

The great thing about `ggplot2` is that it's easy to swap representations. 
Instead of x-y points, we can plot the data as a line graph by swapping `geom_line()`
for `geom_point()`.

*** =instructions
Change the `geom_point()` in the following graph to `geom_line()`. What happened? 
How did the visual presentation of the data change?

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
success_msg("Great! Now you know how to swap representations in ggplot2. Let's move on.")
test_function("geom_line", incorrect_msg="you need to change the geom")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:13ea4fbf3d
## Geoms are layers on a ggplot

We are not restricted to a single geom on a graph! You can think of geoms
as layers on a graph. Thus, we can use the `+` symbol to add geoms to our
base `ggplot()` statement. 

*** =instructions
Add both `geom_line()` and `geom_point()` to the following ggplot. Are the results what you expected?

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
## add code here

```

*** =solution
```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
## add code here
  geom_line() + geom_point()
```

*** =sct
```{r}
test_function("geom_line", incorrect_msg="you need to add geom_line()")
test_function("geom_point", incorrect_msg="you need to add geom_point()")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:349a622cb7
## Quick review about ggplot2

What does the `+` in a `ggplot` statement do? 

For example:

```{r}
ggplot(gap1992, aes(x = log(gdpPercap), y = lifeExp, color=continent)) +
  geom_line() + geom_point()
```

*** =instructions
- adds one `data.frame` to another `data.frame` 
- allows you to chain data and geoms together into a single statistical graphic
- allows you to add variables together in a `data.frame`

*** =hint
`+` combines things, but doesn't add them together

*** =sct
```{r}
msg1 = "This is not the case. Go back and look at the ggplot code."
msg2 = "Correct! This is how we can add data and layer geoms together"
msg3 = "Look at the ggplot code and see if we are manipulating data or not. Are we?"
test_mc(correct = 2, feedback_msgs=c(msg1, msg2, msg3))
```

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
- If you need to remember variable names, you can always call `head(gap1992)` or `colnames(gap1992)` in the console.
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

*** =sct
```{r}
success_msg("Now you know the basics of ggplot and aesthetics. Congrats!")
#test_output_contains("continent", 
#        incorrect_msg="you need to map `continent` to something. What could that be?")
#test_output_contains("log(gdpPercap)", 
#        incorrect_msg="you need to map `log(gdpPercap)` to something. What could that be?")
#test_output_contains("lifeExp", 
#        incorrect_msg="you need to map lifeExp to something")
#test_output_contains("pop", incorrect_msg = "You need to map `pop` to something.")
```

--- type:NormalExercise lang:r xp:0 skills:1 key:fe7e851b1f
## What you learned in this chapter

- Basic `ggplot2` syntax.
- Plotting x-y data using `ggplot2` using both `geom_point()` and `geom_bar()`.
- Mapping variables in a dataset to visual properties using `aes()`
- `geom`s correspond to layers in a graph.
- That `ggplot2` can make some pretty cool graphs
- That you can do this!

*** =instructions
Just move on to the next chapter!

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
