---
title       : The magic of `ggplot2`
description : How ggplot turns variables into statistical graphics

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:d599f92ec8
## Thinking about aesthetics
The first thing we are going to is think about how we represent variables in a plot. We need to figure out what 

Take a look at this graph. What variable is mapped to y, and what is mapped to x, and what is mapped to color?

***=pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

#show first columns of dataset
head(gap1992)

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
msg3 = "Correct! We are displaying lifeExp as our y variable and log(gdpPercap) as our x variable"
msg1 = "You have things reversed, and you're taking the log of the wrong variable"
msg2 = "Wrong variables. Go back and look"
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

A ggplot always starts with the `ggplot()` function. In this function, we need two things:

1. `data` - in this case, `gap1992`.
2. `mapping` - An aesthetic mapping, using the `aes()` function. 

In order to map our variables to aesthetic properties, we will need to use `aes()`. Here we map `x` to `log(gdpPercap)` and `y` to `log(pop)`.

Finally, we can superimpose our geometry on the plot using `geom_point()`.

*** =instructions
Based on the graph, map the appropriate variables to the x, y, and color aesthetics.

*** =pre_exercise_code
```{r}
library(dplyr)
library(gapminder)
library(ggplot2)
gap1992 <- gapminder %>% filter(year == 1992)

#show first columns of dataset
head(gap1992)

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
test_output_contains("log(gdpPercap)", incorrect_msg="You need to log transform one of the variables.")
test_output_contains("lifeExp", incorrect_msg="you need to map lifeExp to something")
test_output_contains("continent", incorrect_msg="you need to map continent to something. What could that be?")
```

--- type:NormalExercise lang:r xp:100 skills:1
## Points versus lines

The great thing about `ggplot2` is that it's easy to swap representations.

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
test_output_contains("geom_line()", incorrect_msg="you need to change the geom")
```

--- type:NormalExercise lang:r xp:100 skills:1
## Geoms are layers on a ggplot

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
test_output_contains("geom_line()", incorrect_msg="you need to change the geom")
test_output_contains("geom_point()", incorrect_msg="you need to change the geom")
```

--- type:NormalExercise lang:r xp:300 skills:1
## Final Challenge

Your final challenge is to completely recreate this graph using the `gap1992` data.


