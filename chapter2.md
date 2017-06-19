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

Here's the simple dataset we'll use to investigate how to work with factors in `ggplot2`.

*** =instructions
Using the console, look at the `pets` dataset. Show the categories for `shotsCurrent` and `ageCategory` using `levels()`. 
Remember, you'll need to use the `$` operator to access the variables in the `data.frame` (example: `pets$animal`).

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
test_function("levels", index=1, incorrect_msg = "did you call levels() on pets$shotsCurrent?")
test_function("levels", index=2, incorrect_msg = "did you call levels() on pets$ageCategory?")

```

--- type:NormalExercise lang:r xp:100 skills:1 key:8102f92bcd
## A Basic Barplot using `geom_bar()`

Now that we understand what categories exist in our dataset, we can begin to visualize them using barplots generated with the `geom_bar()` geom.

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
##Show a barplot and count by name and fill by animal
##theme() allows us to angle the text labels so that we can read them
ggplot(pets, aes(
    x=name, 
    fill=  
  )) + geom_bar() + 
    ## We make the x axis text angled 
    ## for better legibility
    theme(axis.text.x = element_text(angle=45))
```

*** =solution
```{r}
##show a barplot and count by name and fill by animal
ggplot(pets, aes(x=name, fill=animal)) + geom_bar() + 
    ## we make the x axis text angled for better legibility
    theme(axis.text.x = element_text(angle=45))

```
*** =sct
```{r}
#test_ggplot(check_aes = )
```


--- type:NormalExercise lang:r xp:100 skills:1 key:b2586607f1
##  Stacked Bars

Let's see how many of each animal got shots. We can do this by mapping `shotsCurrent` to `fill`.

```{r}
#we map color (the outline of the plot) to black to make it look prettier
ggplot(pets, aes(x=animal, fill=shotsCurrent)) + 
  geom_bar(color="black")
```

*** =instructions
Map `shotsCurrent` to the `fill` aesthetic.

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")

```

*** =sample_code
```{r}
#map the right variable in pets to fill
ggplot(pets, aes(x=animal, fill= )) + 
  geom_bar(color="black")
```

*** =solution
```{r}
ggplot(pets, aes(x=animal, fill=shotsCurrent)) + 
  geom_bar(color="black")

```

*** =sct
```{r}
test_ggplot(check_aes = TRUE)
```


--- type:MultipleChoiceExercise lang:r xp:50 skills:1 key:2723146f59
## Quick Quiz

What does mapping `color` to `"black"` in `geom_bar()` do? 

*** =instructions
- Makes the default bar fill color black
- Specifies the text in the graph to be black
- Outlines the bars in black

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
ggplot(pets, aes(x=animal, fill=shotsCurrent)) + 
  geom_bar(color="black")
```

*** =sct
```{r}
success_msg("Great! That's one tip for improving the appearance of your bar graph.")
msg1 = "Not quite. Look at the graph. What is being mapped to `black`?"
msg3 = "Good! Yes, we are outlining the bars in `black`."
test_mc(correct = 3, feedback_msgs = c(msg1, msg1, msg3))
```
--- type:NormalExercise lang:r xp:100 skills:1 key:942f00b085
## Proportional Barchart

We may only be interested in the relative proportions between the different categories. 
Visualizing this is useful for various 2 x 2 tests on proportions. By mapping `position = "fill"`, 
we can show proportions rather than counts. 

What percent of dogs did not receive shots?

*** =instructions
Change the `position` argument in `geom_bar()` to `"fill"`.

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position= , color="black")
```

*** =solution
```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position= "fill", color="black")
```

*** =sct
```{r}
success_msg("Great! Now you know how to make a proportional stacked bar plot! Let's move on")
test_function("geom_bar", args = "position", incorrect_msg = "Did you add a position argument to `geom_bar()`?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5a8192a589
## Dodge those bars!

Instead of stacking, we can also dodge the bars (move the bars so they're beside each other).

*** =instructions
Change the `position` argument in `geom_bar()` to `"dodge"`.

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position= , color="black")
```

*** =solution
```{r}
ggplot(pets, aes(x=animal,fill=shotsCurrent)) + 
  geom_bar(position= "dodge", color="black")
```

*** =sct
```{r}
success_msg("Great! Now you know how to make a dodged bar plot! Let's move on")
test_function("geom_bar", args = "position", incorrect_msg = "Did you add a position argument to `geom_bar()`?")
```
--- type:NormalExercise lang:r xp:100 skills:1 key:3a6ee29fbc
## Faceting a graph

Say you have another `factor` variable and you want to stratify the plots based on that. 
You can do that by supplying the name of that variable as a facet. Here, we facet our barplot by `shotsCurrent`.

```{r}
ggplot(data=pets, mapping=aes(x=name)) + geom_bar() + 
  ## have to specify facets using "~" notation
  facet_wrap(facets=~shotsCurrent) + 
  ## we make the x axis x angled for better legibility
  theme(axis.text.x = element_text(angle=45))
```

You might notice that there are blank spots for the categories in each facet. We can remove these in each facet by using `scale="free_x"` argument in `facet_wrap()`.

*** =instructions
Add `free_x` to the scale argument. How many animals named "Morris" did not receive shots? 

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

*** =sct
```{r}

```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:fa1117cdfb
## Super Quick Review

Faceting a graph allows us to:

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
- plot points as pretty gems
- stratify our graph by another category
- add another set of categories to the x-axis


*** =hint
Look at the y-axis.

*** =sct
```{r}
msg1 = "Not that kind of facet!"
msg2 = "Yes! Faceting is a way of visualizing information by stratifying on an additional variable."
msg3 = "Not right."

test_mc(correct = 2, feedback_msgs=c(msg1, msg2, msg3))
```


--- type:NormalExercise lang:r xp:300 skills:1 key:2a9c3e204e
## Your Task: Bar Charts

Now you can put everything you've learned together into a single barplot.

*** =instructions
Given the `pets` `data.frame`, plot a stacked proportional barchart that shows the `ageCategory` 
counts by `animal` type. Facet this plot by `shotsCurrent`. 

Is the proportion of animals receiving shots the same across each age category?

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =hint
Think about what to map to `x`, and what to map to `fill`, and what `position` argument 
you need for `geom_bar()`. Finally, think about how to facet the variable.

*** =sample_code
```{r}
ggplot(pets, aes(x=ageCategory, fill=)) + 
    #what argument goes here?
    geom_bar() +
    facet_wrap()
```

*** =solution
```{r}
#Space for your answer here.
ggplot(pets, aes(x=ageCategory, fill=animal)) + geom_bar(position="fill") +
    facet_wrap(facets=~shotsCurrent, scale="free_x")
```

*** =sct
```{r}
success_msg("Good job! You managed to integrate everything you learned about barplots. Let's move on!")
test_ggplot(check_facet = TRUE, check_aes = TRUE)
test_function("geom_bar", incorrect_msg = "Did you use `geom_bar()`?")
```

--- type:MultipleChoiceExercise lang:r xp:100 skills:1 key:a1efb868f1
## Boxplots

Boxplots allow us to assess distributions of a continuous variable (`weight`) conditioned on categorical variables (`shotsCurrent`).

What does this tell us? Is there a difference in weight between those animals who recieved shots or not?

*** =instructions
- there is a small difference in means, but the difference doesn't look significant
- there is no difference in means
- there is a large difference in means and the difference is probably statistically significant

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")

ggplot(pets, aes(x=shotsCurrent, y=weight)) + geom_boxplot()
```

*** =sct
```{r}
msg1 = "Yes, this appears to be the case. The interquartile ranges are overlapping, so it's probably not significant."
msg2 = "This is a judgement call, but there is a small difference in means between the groups"
msg3 = "Probably not true. The interquartile ranges are overlapping, so it's probably not a significant difference in means."

test_mc(correct = 1, feedback_msgs=c(msg1, msg2, msg3))
```

--- type:NormalExercise lang:r xp:100 skills:1 key:46aedf28cb
## Try out geom_boxplot() yourself

*** =instructions
Plot a boxplot of `weight` conditioned on `animal`. Is there a difference in weight between animal types?

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =hint
Think about what variables map to what aesthetics. 

*** =sample_code
```{r}
ggplot(pets, aes(x= , y=) + geom_boxplot()
```

*** =solution
```{r}
ggplot(pets, aes(x=animal, y=weight) + geom_boxplot()
```

*** =sct
```{r}
success_msg("Great! Now you know how to set up boxplots!")
test_ggplot(check_aes = TRUE, aes_fail_msg = "You didn't quite get the variable mapping right. Try again.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:76115fda96
## Violin Plots

Violin plots are another useful way to visualize the data. They provide a more nuanced look at the data. They're a density plot that's mirrored around the y-axis.

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =instructions
Add `geom_violin()` to the ggplot statement. How does the violin plot compare to the boxplot? Does it show less or more information?

*** =sample_code
```{r}
ggplot(pets, aes(x=animal, y=weight, fill=animal)) + 
```

*** =solution
```{r}
ggplot(pets, aes(x=animal, y=weight, fill=animal)) + geom_violin()
```

--- type:NormalExercise lang:r xp:300 skills:1 key:4b380fad6f
## Your task: How heavy are our pets?


*** =instructions
Visualize `weight` by `animal` type. Plot both boxplots and a violin plot on the same graph.
What do you conclude? Which kind of animal weighs more on average than the other? 

*** =hint

*** =pre_exercise_code
```{r}
library(ggplot2)
pets <- read.csv("http://s3.amazonaws.com/assets.datacamp.com/production/course_3864/datasets/pets.csv")
```

*** =sample_code
```{r}
ggplot(pets, aes()) + 
## add boxplots and violin plots as separate layers.
```

*** =solution
```{r}

```

*** =sct
```{r}

```
--- type:NormalExercise lang:r xp:100 skills:1 key:87317d094e
## What you learned in this chapter

- How to visualize categorical data
- Three more types of plots: `geom_bar()`, `geom_boxplot()` and `geom_violin()`
- Aesthetics that can be mapped to these geoms (`fill`, `x`, `y`)
- Options for `geom_bar()`: `position = "fill"` (proportional bars) and `position = "dodge"` (dodged bars)
- How to stratify your graphs using `facet_wrap()`
- More about how to put together a ggplot

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
