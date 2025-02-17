[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/34zqszlc)
# tidy-model
Assignment by Caleb Trujillo

For this assignment, we will learn how to represent data with a statistical model and take advantage of some of the tools in the tidyverse and base R that will make working with data easier. 

## Learning objectives 

In this assignment, you will demonstrate your ability to:

* Be capable of running, modifying, and sharing scripts to accomplish analyze data and visualize in a scripting language (R)
* Manage project development to store, organize, and track code using digital collaboration tools for reproducibility (GitHub)
* Build a quantitative linear model to accompany a data visualization for statistical analysis

## Before you get started

Familiarize yourself with linear regression. 

If you aren't familiar with linear models or need  a refresher about linear models and how to interpret them, start here by watching these two YouTube videos:
* [Linear Regression: Very Basics](https://www.youtube.com/watch?v=ZkjP5RJLQF4)
* [Linear Regression, Algebra, Equations, and Patterns](https://www.youtube.com/watch?v=iAgYLRy7e20&list=PLIeGtxpvyG-LoKUpV0fSY8BGKIMIdmfCi&index=2). 

Additionally, you may want to read about [Software for modeling in *Tidy Modeling with R*](https://www.tmwr.org/software-modeling.html) by Max Kuhn and Julia Silge.

To supplement this assignment, use resources from [A review of R modeling fundamentals in *Tidy Modeling with R*](https://www.tmwr.org/base-r.html) and 
The sections on modeling form [*R for Data Science*](https://r4ds.had.co.nz) by Hadley and Grolemund: 

22.  [Introduction](https://r4ds.had.co.nz/model-intro.html)
23.  [Model basics](https://r4ds.had.co.nz/model-basics.html) 
24.  [Model building](https://r4ds.had.co.nz/model-building.html)

## Instructions

Write out the code to get in the habit of understanding its grammar. Do not copy directly. The power of learning code is the creative avenues it unlocks. If you need help, remember there are lots of resources.

## Creating an Rmd file

We will begin this assignment by creating a new R Markdown document. Select **New File** and **R Markdown...**, give the document the title "tidy-model" and add your name as the author. Save the new `.Rmd` file as `tidy-model.Rmd`.

Open the file and then use the **Knit** button to load the file as an html document.

At this point, save the file, write a commit message, and ***commit***.

### Setup chunk

We will update the library. Update the *setup* chunk to load packages as shown below. Here, we use `include=FALSE`, so the chunk is not included in the final knit document. 

````
```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
library('tidyverse') ; library('modeldata')
``` 
````

You may not have these packages installed, so you will want to use the following code in the **console** to get the packages needed for this assignment. 

`install.packages(c('tidyverse', 'modeldata'))`

Save the file, write a commit message, and ***commit***.

### Introduction

Underneath the setup chunk, replace the text with the following header and text.

`## Introduction`

`This R Markdown document demonstrates my abilities to use models for data analysis using a data set collected on crickets.`

Save the file, write a commit message, and ***commit***.

### Load data chunk

Create a new chunk under your replaced text called `{r load data, include = FALSE}`.
 
Inside this chunk, load the data collected on different species of crickets.

````
```{r load data, include = FALSE}
data(crickets, package = "modeldata")
names(crickets)
```
````

Save the file, write a commit message, and ***commit***.

### What factors affect the chirp rate?

#### Graphing chirps

Remove the remaining text, header, and code created by the template so we can code our information.

Let's create a header `## What is that sound?`  and under this header, we will write the text:
`In this report, we examine what factors predict a cricket's chirp rate.`

Next, we will add a chunk named *summary* that summarizes the data in the data set. Using the `summary ()` function, we can examine the data at a glance. 

````
```{r summary, echo = FALSE}
summary(crickets)
``` 
````

Save the file, knit, write a commit message, and ***commit***.

Next, write in the text to describe the general statistics by reporting the number of observations in the data set, the number of species, the temperature range (minimum and maximum), and the mean rate of chirping.

Add to the chunk `summary` to make a histogram of the chirp rate based on ggplot.

````
```{r summary, echo = FALSE}
summary(crickets)
ggplot(crickets, aes(x = rate)) +
  geom_histogram(bins = 8) + 
  ggtitle("Distribution of the chirp rate of crickets") +
  xlab('Chirp rate (per min.)')
```
````

Save the file, knit, write a commit message, and ***commit***.

If you get an error related to the duplicate label 'summary', it is because the summary chunk should be the only chunk that we wrote the above code in. 

#### Graphing temperature and chirps

Let's create a new header, `## Temperature affects chirp rate`.

Next, we will add a chunk named *temp* that plots a scatter plot of temperature and chirp rate.

````
```{r temp, echo= FALSE}
ggplot(crickets, aes(x = temp, y = rate)) +
  geom_point() + 
  geom_smooth(method = 'lm') +
  ggtitle("Plot of temperature and chirp rate") +
  ylab('Chirp rate (per min.)') +
  xlab('Temperature (Celsius)')
```
````

After making a graph, add some text below such as `Based on a scatter plot of temperature and chirping, it seems that as temperature increases, the rate of chirping also increases.`

Save the file, knit, write a commit message, and ***commit***.

#### Modeling temperature and chirps

In the `temp` chunk, create a new line below the ggplot commands to add a linear model using the functions `lm()` and `summary.lm()`. Notice that this line uses the formula format to calculate the model. 

Instead of ggplot's aesthetics function  `aes(x = temp, y = rate)`,  we can represent our variables as a formula `rate ~ temp`, which states that the linear formula will try to predict chirp rate (the y-value) based on a linear relationship with temperature (the x-value). Typically, these models assume that the dependent variable comes before your list of independent variables, `y ~ x`. Often, the syntax will ask for a formula before you enter  the argument for data.  

Store the output of the `lm()` function as a new object `temp_lm`. Then, call up the `summary.lm()` for a complete linear model output.

Update the chunk so it looks like the one below.

````
```{r temp, echo= FALSE}
ggplot(crickets, aes(x = temp, y = rate)) +
  geom_point() + 
  geom_smooth(method = 'lm') +
  ggtitle("Plot of temperature and chirp rate") +
  ylab('Chirp rate (per min.)') +
  xlab('Temperature (Celsius)')

temp_lm <- lm(rate ~ temp, crickets)

summary.lm(temp_lm)
```
````

Notice that the linear model provides two important values. The first is the intercept, the point at which the line of best fit intersects the y-axis. Additionally, we see a second value representing the curve's slope—--the relationship to rate. In this case, we can see that as the temperature rises 1 degree C, the crickets will chirp an additional four chirps per minute. 

In addition to the linear model, which gives us a sense of the relationship, we can also see the residual values (the distance between each point and the curve), the standard error of the measures, and even calculated probability values (p-values) for hypothesis testing. This summary gives us great information if we are interested in t-values, R-squared values, or F-statistics!

For now, let's revise our earlier sentence in the text to add more information, such as the following text:  

`Based on a scatter plot of temperature and chirping and a correlation test, it seems that as temperature increases one degree, the rate of chirping increases about 4.2 chirps per minute.`

Save the file, knit, write a commit message, and ***commit***.

Again, if you get an error related to the duplicate label 'temp', it is because the summary chunk should be the only chunk in which we wrote the above code.

#### Temperature across species

Next, we can see if an account of the different species of crickets affects our model. Up to this point, we have grouped all crickets, which may be an inappropriate assumption if the species are distinct. Many animals use signals like chirping to communicate, and it's important to distinguish your call from other similar species for mating behaviors. For instance, species of fireflies are known to signal at different times of night with unique light patterns and colors even when occupying similar areas.

Now, let's make a new header and chunk for graphing and separating these two values. 

`## Species-specific effects of temperature on chirping`

````
```{r species, echo= FALSE}
ggplot(crickets, aes(x = temp, y = rate, color = species)) +
  geom_point() + 
  geom_smooth(method = 'lm') +
  ggtitle("Plot of temperature and chirp rate for two species of crickets") +
  ylab('Chirp rate (per min.)') +
  xlab('Temperature (Celsius)')
```
````

Wow! It looks like instead of one line of best fit, now we have two! The plot has added colors to separate the data by species. We can see that both crickets have a positive relationship between temperature and chirping but start at different places. So, let's return to our linear model to see how values have changed. Add the `lm()` and `summary.lm()` functions to the chunk. We will extend our formula to account for the new variable this time. 

````
```{r species, echo= FALSE}
ggplot(crickets, aes(x = temp, y = rate, color = species)) +
  geom_point() + 
  geom_smooth(method = 'lm') +
  ggtitle("Plot of temperature and chirp rate for two species of crickets") +
  ylab('Chirp rate (per min.)') +
  xlab('Temperature (Celsius)')
  
  
species_lm <- lm(rate ~ temp + species, crickets)

summary.lm(species_lm)
```
````

Let's check out the data of this model to see how it stacks up against the first model. Now, we have three important values in the coefficients table. It helps us think about the impact of different variables on the line. The intercept again states where the line would cross the y-axis. The value gives us the temperature relationship to chirping like before. Notice that this has dropped to 3.6 as the slope, so as the temperature rises one degree (C), the chirps increase by 3.6 per minute. Finally, the third variable is -10. This value represents the difference between species. Since we have only two species, we can treat this as the difference in the intercept between the two values. So the cricket species *O. exclamationis* has a chirp rate of 10 chirps per minute, higher than its counterpart *O. niveus*. So, we have a linear model that predicts the relationship between temperature, species, and chirps. 

We can use any of the statistical methods collected from `summary.lm()` to compare the quality of the species-specific model to the general model we made earlier. For instance, let's compare the R-squared values, which measure the amount of variance the model explains. In the `temp_lm` model, the R-squared value was 0.9199, which means that the model accounts for about 92 percent of the variance in the data. In contrast, in the `species_lm` model, the R-squared value was 0.9896, which means about 99 percent of the variance in the data is explained by this model. Even though the first model might be good enough, the second one is much more reliable for making predictions. 
 
Add text to explain in your own words what the second graph and model show. 

Save the file, knit, write a commit message, and ***commit***.

#### Interactions between temperature and species

In addition to seeing the variables as contributing to the slope or intercept, we can also test for interactions. The effects of species and temperature might be interlinked because one species tends to live in a colder temperature than another. 

For instance, let's examine our original histogram with a new sensitivity to the species variable. 

Create a new header `## Interactions`, new chunk called `species histogram` and then modify the code from the `summary` chunk to add fill as a variable for species. This latest graph should show that the species occupy different temperature zones, which we may want to account for in the model. Add text below that states that these species occupy different temperature ranges.

````
```{r species historgram, echo = FALSE}
ggplot(crickets, aes(x = rate, fill = species)) +
  geom_histogram(position = 'identity', alpha = 0.7, bins = 8) + 
  ggtitle("Distribution of the chirp rate of crickets") +
  xlab('Chirp rate (per min.)')
```
````

This time we will use our code from the `species` chunk to make a new chunk `interactions`, but we will update it to include an addition to the formula so it accounts for interactions `rate ~ temp + species + temp:species`. Alternatively, you could represent interactions with `rate ~ (temp + species)^2`. We will store the new model as an object `species_x_temp_lm`. To compare this with our previous species-specific models, we will add an ANOVA (Analysis of Variance) code to compare the two linear models. 

````
```{r interactions, echo= FALSE}
ggplot(crickets, aes(x = temp, y = rate, color = species)) +
  geom_point() + 
  geom_smooth(method = 'lm') +
  ggtitle("Plot of temperature and chirp rate for two species of crickets") +
  ylab('Chirp rate (per min.)') +
  xlab('Temperature (Celsius)')
  
species_x_temp_lm <- lm(rate ~ temp + species + temp:species, crickets) 
summary.lm(species_x_temp_lm)

anova(species_lm, species_x_temp_lm)
```
````

This comparison produces a p-value far above 0.05 and suggests that the species-temperature interaction and species-species models are not statistically significant in their difference. Therefore, we can rely on the simpler `species_lm` results. Adding more variables and interactions can often improve the  performance of R-squared, but then it becomes less meaningful to make predictions and practical decisions.
Add text that states that you checked for interactions but decided to stay with the species model. 

Save the file, knit, write a commit message, and ***commit***.

### Visualize and model diamond prices

Now, apply these skills to a different data set.

Without detailed instruction, try to build a linear model of at least three variables to predict the worth of a diamond.

You can find the data in the diamonds object:

```load(ggplot2::diamonds)```

Write commits, knit, and push all your work when you finish.

## Going deeper

Congratulations! You wrote your first code to perform a linear regression analysis on data. These methods can be easily adapted for modeling nearly any data set. The basic ideas and formulas used here can be adapted to run logistic regression models, multiple linear models, hierarchical models, and many other approaches.

Consider additional resources to deepen your knowledge of your particular area of interest. 

* [*Tidy Modeling with R*](https://www.tmwr.org/software-modeling.html) by Max Kuhn and Julia Silge.
* [*R for Data Science*](https://r4ds.had.co.nz) by Hadley and Grolemund:
  + [Introduction](https://r4ds.had.co.nz/model-intro.html)
  + [Model basics](https://r4ds.had.co.nz/model-basics.html) 
  + [Model building](https://r4ds.had.co.nz/model-building.html)
