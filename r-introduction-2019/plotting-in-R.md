# Plotting in R

I used different data, but the examples here are built on those provided by
[STHDA][1].

## Base R Plots

### Plot

The ``plot()`` function is the most basic plot and is often used to plot line or point plots. The syntax is ``plot(x,y)`` but also includes arguments for modifying the plot. Examples:

**Basic Plot**

```
ds <- USArrests # load the built-in dataset
head(ds) # look at the top few rows
plot(ds$Murder, ds$UrbanPop, type = "p")
```

Data example 2:

```
library(robustbase)
edex <- education
head(education)
```

**Adding Labels**

Variables include (from ``?education``):

- 'State' State
- 'Region' Region (1=Northeastern, 2=North central, 3=Southern, 4=Western)
- 'X1' Number of residents per thousand residing in urban areas in 1970
- 'X2' Per capita personal income in 1973
- 'X3' Number of residents per thousand under 18 years of age in 1974
- 'Y' Per capita expenditure on public education in a state, projected for 1975

Since the variables do not have descriptive names, we can add labels to the X and Y axes:

```
plot(education$X1, education$Y,
     xlab = "Residents per 1000 in Urban Areas",
     ylab = "Per capita Expenditure on Public Education",
     main = "Relationship Between Expenditure on Public Ed and Urban Pop")
```

**Adding Regression Line**

```
plot(education$X1, education$Y,
     xlab = "Residents per 1000 in Urban Areas",
     ylab = "Per capita Expenditure on Public Education",
     main = "Relationship Between Expenditure on Public Ed and Urban Pop")
abline(lm(education$Y ~ education$X1), col = "blue")
```

**Adding Loess Line**

```
plot(education$X1, education$Y,
     xlab = "Residents per 1000 in Urban Areas",
     ylab = "Per capita Expenditure on Public Education",
     main = "Relationship Between Expenditure on Public Ed and Urban Pop")
lines(lowess(education$Y ~ education$X1), col = "blue")
```

### Scatterplots

**Scatterplots with Groups**

```
library("car")
scatterplot(education$Y ~ education$X1 | education$Region)
```

Making the scatterplot more readable by describing the regions variable:

```
education$RegionName <- factor(education$Region,
    labels = c("Northeastern", "North Central", "Southern", "Western"))
scatterplot(education$Y ~ education$X1 | education$RegionName)
```

**Scatterplot Matrices**

Here's we'll compare the four numerical variables by specifying their column numbers. We can define the kind of points the plot creates with the ``pch`` argument. See ``?points`` for options.

```
pairs(education[,3:6], pch = 20)
pairs(education[,3:6], pch = 18, col = "blue")
pairs(education[,3:6], pch = 18, col = "red", cex = 1.8)
```

### Box Plots

```
boxplot(education$Y ~ education$Region)
boxplot(education$Y ~ education$RegionName)
boxplot(education$Y ~ education$RegionName,
        col = c("red", "blue", "green", "orange"))
```

### Strip Charts

```
stripchart(education$Y ~ education$RegionName)
stripchart(education$Y ~ education$RegionName, vertical = TRUE)
stripchart(education$Y ~ education$RegionName, vertical = TRUE,
           col = c("red", "blue", "green", "orange"))
```

### Bar Plots

Bar plot does not aggregrate by default. Therefore I use the ``table`` command to aggregate the counts:

```
barplot(education$Region)
barplot(education$RegionName)
table(education$RegionName) # to see what's being plotted
barplot(table(education$RegionName))
barplot(table(education$RegionName),
        col = c("red", "blue", "green", "orange"))
```

To create a legend with this data requires some hacking. To compare, in the second call, I use the ``unique`` function to get unique values:

```
barplot(table(education$RegionName),
        col = c("red", "blue", "green", "orange"),
        legend = education$RegionName)
barplot(table(education$RegionName),
        col = c("red", "blue", "green", "orange"),
        legend = unique(education$RegionName))
```

### Multiple Lines

From the ``bigcity`` data set. ``u`` is the population of 49 U.S. cities in 1920. ``x`` is the the population of these cities in 1930. I'm simply sorting these to provide the example.

```
library(boot)
head(bigcity)
plot(sort(bigcity$u), type = "b", col = "blue", pch = 18)
lines(sort(bigcity$x), type = "b", col = "red", pch = 19)
legend("topleft", legend = c("1920", "1930"),
       col = c("blue", "red"), lty = 1:1)
```

### Pie Chart

We need to aggregate the data for a pie chart. For this, I'll use the ``tapply`` function to take the mean of the ``Y`` variable for each ``RegionName``.

```
pie(tapply(education$Y, education$RegionName, FUN = mean))
pie(tapply(education$Y, education$RegionName, FUN = mean),
    col = c("blue", "red", "green", "orange"))
```

### Histograms

```
hist(education$X1)
hist(education$X1, col = "red")
hist(education$X1, col = "#1565c0")
hist(education$X1, col = "#1565c0", breaks = 5)
```

### Density Plots

```
plot(density(education$X1), col = "#1565c0")
```

### Dot Chart

```
dotchart(education$X1, labels = education$State,
         groups = education$RegionName,
         main = "Education Expenditure 1970s")
```

### Plot Group Means

```
library(gplots)
plotmeans(education$X1 ~ education$RegionName)
plotmeans(education$X1 ~ education$RegionName, mean.labels = TRUE)
plotmeans(education$X1 ~ education$RegionName, connect = FALSE)
```

## Saving Plots

By default, plots are displayed and not saved as files. R can save plots in
multiple file formats, and they all generally follow the syntax below that
I use to save a basic plot as a PNG file:

```
png("plot1.png", width = 700, height = 700)
plotmeans(education$X1 ~ education$RegionName, connect = FALSE)
dev.off()
```

## References

See [STHDA][1] for many more fine examples plus some other plotting libraries.

[1]:http://www.sthda.com/english/wiki/r-base-graphs
