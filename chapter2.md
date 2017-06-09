---
title       : ggplot2 and categorical data
description : More on plotting using ggplot2

--- type:NormalExercise lang:r xp:100 skills:1 key:6b1d7abb63
## Review of factors

Factors are how R represents categorical data.

There are two kinds of factors: 

+ `factor` - used for *nominal* data ("Ducks","Cats","Dogs")
+ `ordered`- used for *ordinal* data ("10-30","31-40","41-60")

We'll manipulate our barplots and add more information using factors.

Here's the simple dataset we'll use to investigate how to work with factors in `ggplot`.

*** =instructions
Using the console, look at the `pets` dataset. Show the categories for `shotsCurrent` and `ageCategory` using `levels()`. Remember, you'll need to use the `$` operator to access the variables in the `data.frame` (example: `pets$animal`).

*** =pre_exercise_code
```{r}
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
##show categories for shotsCurrent
levels()

##show categories for ageCategory
levels()

```

*** =solution
```{r}
##show categories for shotsCurrent
levels(pets$shotsCurrent)

##show categories for ageCategory
levels(pets$ageCategory)
```
*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:8102f92bcd
## A Basic Barplot using `geom_bar()`

Now that we understand our the categories in our data, we can begin to visualize them using barplots generated with the `geom_bar()` geom.

```{r}
##show a barplot and count by name and fill by animal
ggplot(pets, aes(x=name)) + geom_bar()
```

The `geom_bar()` default is to count the number of values with each factor level. Note that you don't map to a y-aesthetic here, because the y values are the counts.

Given this dataset, we might want to ask how many pets have the same name. What is the most popular name?

*** =instructions
Map the `animal` variable to `fill`. Are the results what you expected?

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
ggplot(pets, aes(x=name)) + geom_bar() + theme(axis.text.x = element_text(angle=45))
```

*** =sample_code
```{r}
##show a barplot and count by name and fill by animal
##theme() allows us to angle the text labels so that we can read them
ggplot(pets, aes(x=name, fill=  )) + geom_bar() + 
    ## we make the x axis x angled for better legibility
    theme(axis.text.x = element_text(angle=45))
```

*** =solution
```{r}
##show a barplot and count by name and fill by animal
ggplot(pets, aes(x=name, fill=animal)) + geom_bar() + 
    ## we make the x axis x angled for better legibility
    theme(axis.text.x = element_text(angle=45))

```
*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:3a6ee29fbc
## Faceting a graph

Say you have another `factor` variable and you want to stratify the plots based on that. 
You can do that by supplying the name of that variable as a facet. Here, we facet our barplot by `shotsCurrent`.

```{r}
ggplot(data=pets, mapping=aes(x=name)) + geom_bar() + 
  ## have to specify facets using notation
  ## try out facets=~ageCategory!
  facet_wrap(facets=~shotsCurrent) + 
  ## we make the x axis x angled for better legibility
  theme(axis.text.x = element_text(angle=45))
```

You might notice that there are blank spots for the categories in each facet. We can restrict the factors in each by using `scale="free_x"` argument in `facet_wrap()`.

*** =instructions
How many animals named "Morris" did not receive shots? Map 

*** =sample_code
```{r}
ggplot(pets, aes(x=name)) + geom_bar() + 
  facet_wrap(facets=~shotsCurrent, scale=    ) +
  theme(axis.text.x = element_text(angle=45))
```

*** =solution
```{r}
ggplot(pets, aes(x=name)) + geom_bar() + 
  facet_wrap(facets=~shotsCurrent, scale="free_x") +
  theme(axis.text.x = element_text(angle=45))
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:fa1117cdfb
## Super Quick Review

Faceting a graph allows us to

```{r}
ggplot(pets, aes(x=name)) + geom_bar() + 
  facet_wrap(facets=~shotsCurrent, scale="free_x") +
  theme(axis.text.x = element_text(angle=45))
```
*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")

ggplot(pets, aes(x=name)) + geom_bar() + 
  facet_wrap(facets=~shotsCurrent, scale="free_x") +
  theme(axis.text.x = element_text(angle=45))
```

*** =instructions
- see ourselves in the graph
- stratify our graph by another category
- add another set of categories to the x-axis






--- type:NormalExercise lang:r xp:100 skills:1 key:5a83c50f55
## Stacked Bars

Let's see how many of each animal got shots. We can do this by mapping `shotsCurrent` to `fill`.

```{r}
#we map color (the outline of the plot) to black to make it look prettier
ggplot(pets, aes(x=animal, fill=shotsCurrent)) + 
  geom_bar(color="black")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:942f00b085
## Proportional Barchart

We may only be interested in the relative proportions between the different categories. Visualizing this is useful for various 2 x 2 tests on proportions.

What percent of dogs did not receive shots?

```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position = "fill", color = "black")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:3784f57df1
## Dodge those bars!

Instead of stacking, we can also dodge the bars (move the bars so they're beside each other).

```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position="dodge", color="black")
```

--- type:NormalExercise lang:r xp:300 skills:1 key:2a9c3e204e
## Your Task: Bar Charts

Given the `pets` `data.frame`, plot a stacked proportional barchart that shows the age category counts by animal type. Is the proportion of animals receiving shots the same across each age category?

Hints: think about what to map to `x`, and what to map to `fill`.

Intermediate Folks: facet this plot by `shotsCurrent`. 

```{r}
#Space for your answer here.

```

```{r}
#Space for your answer here.

```

--- type:NormalExercise lang:r xp:100 skills:1 key:a1efb868f1
## Boxplots

Boxplots allow us to assess distributions of a continuous variable conditioned on categorical variables.

What does this tell us? 

```{r}
ggplot(pets, aes(x=shotsCurrent, y=weight)) + geom_boxplot()
```

--- type:NormalExercise lang:r xp:100 skills:1 key:76115fda96
## Violin Plots

Violin plots are another useful way to visualize the data. They provide a more nuanced look at the data. They're a density plot that's mirrored around the y-axis.

```{r}
ggplot(pets, aes(x=ageCategory, y=weight, fill=ageCategory)) + geom_violin()
```

--- type:NormalExercise lang:r xp:100 skills:1 key:374d642704
## Your task: How heavy are our pets?

Visualize `weight` by `animal` type as both a boxplot and a violin plot. What do you conclude? Which kind of animal weighs more on average than the other?

Intermediate challenge: How would we plot both boxplots and a violin plot on the same graph?

```{r}
##Space for your answer here

```
