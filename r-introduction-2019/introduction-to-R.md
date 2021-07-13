# Introduction to R 

## Quick tips

### Get help

If you need help with R, then a search engine is your friend. However, since
"R" is a common term, I precede many of my web searches on R with the term **r
stat [query]**, and that's worked well for me.

But R also has builtin in help/documentation/examples. To get help on a command
or function, use the question mark followed by the name of a function or
library, or use two question marks to search for a topic. In the example that
follows, I am pulling up the specific documentation for the `lm` function. In
the second statement, I'm searching the documentation for the term
`chisquare`.

```
?lm
??chisquare
```

The main R website is incredibly useful, too. The site contains links to FAQs,
manuals, books, and an academic journal dedicated to R. See:

[https://www.r-project.org/][6]

### Installing and Updating Packages

R includes base libraries (functions and data) in the default install. These
are called **Base R** or **R Base**. A comprehensive list of these packages are
on the web at the link below, and the links to each package on that site
retrieve the same documentation that you access when you use the `?` commands
above:

[https://stat.ethz.ch/R-manual/R-devel/library/base/html/00Index.html][3]

Eventually you will want to to install additional libraries to add
functionality. To do so, use the following command, and replace `package`
with the name of the package to install:

```
install.packages("package")
```

Once installed, to load a package into the workspace so that you can use it,
you use the `library` command:

```
library("package")
```

Periodically you may need or want to update all packages that are installed:

```
update.packages()
```

Packages and R itself are written by people who work in different industries,
but many work in academia. It's thus appreciated if we cite R and the packages
that we use in our analyses in our publications. To get citation information
for packages (note the version number in the citation info):

```
citation() # to cite R itself
citation("package")
```

### Using a text editor

When we write code, we use a text editor (not a word processor). [RStudio][2]
provides a builtin text editor, but otherwise, which one you choose is based on
personal preferences. [Atom][4] is a popular, cross-platform text editor that
has good integration with [Git][7] / [GitHub][5]. GitHub is a site to manage
and store code. It's also a popular place to find additional R packages.

When writing in a text editor, use the `#` to write comments to document what
you're doing along the way. For example, here is an example comment followed by
a short mathematical statement:

```
# I will add 2 + 2 together:
2 + 2
```

## Data Objects

R uses several types of data structures, typically referred to as **objects**.
The hardest part of R, in my opinion, is learning how to deal with these data
objects and applying them to messy data to organize your data for analysis. It
depends on the research and the researcher, of course, but if I had to estimate
the amount of time I spend on a project that involves R, I'd say that, on
average, I spend:

- 50% of my time organizing and cleaning data
- 35% of my time analyzing the data
- 15% of my time preparing the data and the output (for example, polishing
  plots for publication)

The main data objects follow. Today we'll introduce ourselves to a few of
these:

- Vectors: cf. column of data of the same type
- Lists: "an object consisting of an ordered collection of objects known as its
  components"; or, a mixed bag of data
- Arrays: a multidimensional structure of the same data type
- Matrices: a two dimensional structure of the same data type 
- Factors: "a vector object used to specify a discrete classification"
- Data Frames: "a list with a class 'data.frame'"; or, kind of like
  a spreadsheet

For more details (and attribution for quotes), see: [An Introduction to R][8]

### Vectors

A R vector may include data of different **classes**, such as numeric,
character, and logical. Vectors may also include missing data, which are
indicated by `NA`. 

In the statemenst below, I use the `<-` to make an assignment. It assigns the
values on the right side of the statement to the variable name on the left side
of the statement. Thus, in the first statement below, we declare the variable
`x` to hold the value `1`, and the variable `y` to hold multiple values
from `1` to `4`, inclusive. Sometimes you'll see examples where the equal
sign `=` is used instead of the `<-` for assignment. It's a matter of
preference, but `<-` helps to distinguish assignment operations from other
operations where the equal sign has a different use.


```
x  <- 1  # numeric vector with one element
y  <- c(1,2,3,4) # numeric vector with multiple elements
z  <- c("one", "two", "three") # character vector
x1 <- c(TRUE, FALSE, TRUE, FALSE) # logical vector
y1 <- c(1,2,NA,3) # numeric vector with one missing element
x  <- rnorm(20, 1, 0.5)
y  <- rnorm(20, 2, 0.7)
z  <- x * y
```

All data objects, including vectors, are indexed. To retrieve a specific
element in a vector by its index, use square brackets:

```
x[1] # retrieves the first element in the vector x 
y[4] # retrieves the fourth element in the vector y
```

### Factors

Vectors may include data that belongs to the **factor** class. Factors may be
unordered or ordered. Whether factors need to be ordered depends on the
research and data, of course. For example, we don't order **female** or
**male**, but we may order grade levels.

Data is not necessarily in the desired class when entered into R, and we use
the `class` function to see how data is classed and then change it if needed:

**unordered:**

```
attendance <- c("Yes", "No", "No", "No", "Yes", "Yes", "No")
class(attendance)
attendance <- as.factor(attendance)
```

**ordered:**

```
# enter data
grades <- c("C", "A", "B", "A", "B", "B", "A", "C", "B")
grades
class(grades)
table(grades)
# change data class
grades <- factor(grades, levels = c("A", "B", "C"), ordered = TRUE)
grades
class(grades)
table(grades)
```

### Data Frames

We can import data from spreadsheets, CSV files, etc, but sometimes we create
data by combining separate vectors. For illustration, let's create two vectors:

- `x` will be a numeric vector that contains 20 elements, normally
  distributed with a mean of 83, and a *sd* of 3.0.
- `y` will be a logical vector, TRUE if greater than 83, FALSE if less than
  83

```
x <- rnorm(20, 83, 3)
x
y <- x > 83
y
y <- as.factor(y)
y
z <- data.frame(x, y)
z
plot(z$x, z$y)
# Reversing the order of the variables or changing the syntax may produce
# different plots:
plot(z$x ~ z$y) # written in formula notation
plot(z$y, z$x) # same as above
```

**Note 1:** R often uses formula notation/syntax:

- `response variable ~ predictor variable`, or:
- `dependent variable ~ independent variable`, or:
- `y ~ x`

**Note 2:** The dollar sign is used to refer to a variable in a data frame
(think of a column in a spreadsheet). Above, `x` is a variable (of any
length) in the `z` data frame.

**Note 3:** To retrieve a specific element in a data frame, use the square
brackets and the index for both row and column:

```
z[1,1] # retrieve first element in first column
z[1,2] # retrieve first element in second column
z[3,2] # retrieve third element in second column
z[3,] # retrieve third row
z[,2] # retrieve second column
```

## Manipulate data

We can do mathematical calcuations on all elements of a vector (or variable in
a data frame) at once:

```
z$x + 100
z$x - 100
z$x * 100
z$x / 100
(z$x - 1) / 100
```

A **log2** transformation of a vector:

```
log(z$x)
```

A **log10** transformation of a vector:

```
log10(z$x)
```

When using the logarithm, be careful if there are zeroes in the vector:

```
n <- 0:10
n
log(n)
```

Instead:

```
n <- 0:10
log(n + 1)
log10(n + 1)
```

Basic math on each element in a vector:

```
z$x - 1
z$x * 2
z$x / 3
(z$x + 100) * 3
exp(z$x) # Euler's number raised to x
pi * z$x
```

## Statistics

Some example statistical analyses:

### Descriptive Statistics

```
set.seed(1)  # used to control random generation :)
x <- rnorm(100, 50, 1)
summary(x)
round(x)
min(x)
max(x)
max(x) - min(x)
sum(x)
rank(x)
quantile(x)
var(x)
sd(x)
mean(x)
median(x)
length(x)
hist(x)
plot(density(x))
```

### Correlation

Three types of correlation, but we'll also read in CSV data, from [Kaggle][1]:

```
health <- read.table(file = "weight-height.csv",
                     header = TRUE,
                     sep = ",")
head(health)
cor(health$Height, health$Weight, method = "pearson")
cor(health$Height, health$Weight, method = "spearman")
cor(health$Height, health$Weight, method = "kendal")
boxplot(health$Weight ~ health$Gender)
boxplot(health$Height ~ health$Gender)
```

**Note 1**: When reading data from the file system, it helps if the data is
located in the same part of the file system (i.e., folder) that you used to
start R. Otherwise, you need to specify the path to the data in the
`read.table` (and similar commands). Specifying the path is OS-dependent.

To get the location of your working directory. If you would like see the
current working directory and/or change it:

```
getwd()
setwd('/home/user/workspace/project1') # e.g., Linux
setwd('/Users/user/workspace/project1') # e.g., macOS
setwd('C://file/path') # e.g., Windows
```

### Linear Regression

Linear regression statements take the form of `model <- lm(DV ~ IV)` or
`model <- lm(y ~ x)`, where "model" is simply the name you choose to refer to
the model.

```
head(cars)
plot(cars$dist ~ cars$speed)
abline(lm(cars$dist ~ cars$speed))
cor(cars$speed, cars$dist, method = "pearson")
fit <- lm(cars$dist ~ cars$speed)
plot(fit) # model diagnostics
summary(fit)
confint(fit)
anova(fit)
```

**Note 1:** `cars` is a data set that is part of **base R**.

### Pearson's Chi-squared test for count data

```
M <- as.table(rbind(c(762, 327, 468), c(484, 239, 477)))
dimnames(M) <- list(gender = c("F", "M"),
                    party = c("Democrat","Independent", "Republican"))
Xsq <- chisq.test(M)  # Prints test summary
Xsq$observed   # observed counts (same as M)
Xsq$expected   # expected counts under the null
Xsq$residuals  # Pearson residuals
Xsq$stdres     # standardized residuals
```

### Chi-squared Goodness of Fit

Here we have prior data and so we don't assume the null probabilities are equal
across all categories of observations:

```
count  <- c(399, 193, 63, 82, 13)
null_probs <- c(0.53, 0.32, 0.08, 0.05, 0.02)
chisq.test(count, p = null_probs)
```

If we do test for equal null probabilities:

```
new_count <- c(20, 39, 31)
chisq.test(count)
```

### t.test

**One Sample t-test:**

```
mean(health$Weight)
t.test(health$Weight, mu = 150)
t.test(health$Weight, mu = 150, alternative = "greater")
```

**Two Sample t-test:**

```
L <- health$Weigth - 10
t.test(health$Weight, L) # if variances are unequal
t.test(health$Weight, L, var.equal = TRUE)
```

**Paired t-test:**

```
btemp1  <- beaver1$temp
length(btemp1)
btemp2  <- beaver2$temp
length(btemp)
set.seed(1)
btemp1a <- sample(btemp1, 100, replace = FALSE)
t.test(btemp1a, btemp2, paired = TRUE)
boxplot(btemp1a, btemp2)
```

### ANOVA

**One way ANOVA**

```
x <- c(2,3,7,2,6,10,8,7,5,10,10,13,14,13,15)
g <- c(rep("group1", 5), rep("group2", 5), rep("group3", 5))
xg <- data.frame(x, g)
xg
fit.a <- aov(x ~ g, data = xg)
summary(fit.1)
plot(xg$x ~ xg$g)
fit.tukey <- TukeyHSD(fit.a)
```

## Saving data 

When you quit R, you are given the option to save your workspace. To quit, use
the following command:

```
q()
```

But you may want to save your data to something like a CSV file, especially if
you've organized and cleaned it up for analysis. To do so:

```
write.table(beaver1, file = "beaver1.csv", sep = ",", quote = TRUE)
```

**Note 1**: The `beaver1` and `beaver1` datasets are provided with **base R**. 

## Reading data

Read CSV from the web:

```
boston <- read.table("https://data.boston.gov/dataset/00c015a1-2b62-4072-a71e-79b292ce9670/resource/9fdbdcad-67c8-4b23-b6ec-861e77d56227/download/tmpilkz66jf.csv",
  header = TRUE,
  sep = ",",
  quote = "\"")
```

Read from other sources, such as [Excel, SPSS, SAS, and Stata][9].

Read from Google Sheets, relational databases, such as MySQL, and other
sources.


[1]:https://www.kaggle.com/mustafaali96/weight-height/version/1
[2]:https://rstudio.com/
[3]:https://stat.ethz.ch/R-manual/R-devel/library/base/html/00Index.html
[4]:https://atom.io
[5]:https://github.com
[6]:https://www.r-project.org/
[7]:https://git-scm.com/
[8]:https://cran.r-project.org/doc/manuals/r-release/R-intro.html
[9]:https://www.statmethods.net/input/importingdata.html
