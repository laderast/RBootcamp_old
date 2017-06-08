---
title       : Introduction to Visualization using `ggplot2`
description : How to get started
attachments :
  slides_link : https://s3.amazonaws.com/assets.datacamp.com/course/teach/slides_example.pdf

## Simple Example
```{r}
library(ggplot2)
data(iris)
ggplot(iris) + geom_point(aes(x= Sepal.Width, y=Sepal.Length)) + theme_minimal()
```

## aes() - the Aesthetic Function

aes() lets you map a variable to a visual property of the geom. Aesthetics can include:

- x or y (almost any geom)
- size
- color (point or line)
- shape (point)
- linetype (line)
- fill (rect, bar, histogram)

Documentation for aes() is remarkably terrible, as it's different for each geom. Look at the documentation for each geom for a list of mappable aesthetics.

## Let's get fancier and add coloring
```{r fig.height=3}
ggplot(iris) + geom_point(aes(x= Sepal.Width, y=Sepal.Length, color=Species, size=Petal.Length))
```

## Using a continuous variable
```{r}
ggplot(iris) + geom_point(aes(x= Sepal.Width, y=Sepal.Length, color=Petal.Length)) +
  scale_color_continuous(low = "white", high = "green")
```

## Other aesthetic attributes: Size
```{r,warning=FALSE}
ggplot(iris) + geom_point(aes(x= Sepal.Width, y=Sepal.Length, size=Petal.Length, color=Species))
```
