Short Introduction to Information Retrieval
========================================================
author: Sean Burns, PhD
date: 2017-11-07
autosize: true

Introduction 
========================================================

# Information Retrieval Basics

- What follows is a brief introduction to information retrieval. The concepts
  presented here are presented discretely, but in practice there is quite a 
  bit of overlap, and other issues, some very complicated, are at play, too.
- I will also present a couple of toy models of information retrieval. This means
  that I've removed many important details so that we can direct our full attention
  to some basic IR concepts and mechanisms.

The Document
========================================================

# Our theoretical assumption: Everything is a document.

- Text documents
- Audio documents
- Still / moving image documents

Information Retrieval Systems
========================================================

# Information retrieval systems mirror their documents

- The capabilities of a system are designed to match the nature of the documents
  that are searched in the system.
- This means that if a system contains text documents, then the retrieval system
  will be designed to match the general characteristics of text documents.
- This is the same with audio files, image files, etc., each of which have their own
  bibliographic characteristics.

Information Retrieval Indexes
========================================================

# Searching against an index

- All search engines and database systems search against an index and not against
  the actual documents or surrogate records (e.g., bibliographic records).
- For example, Google does not re-search every single web page on the entire internet
  each time someone enters a query in Google; rather, queries are searched against a stored index,
  albeit one that Google is constantly building and modifying based on the
  dynamics of the web and internet and on its own search engineering developments.

Full Text Search
========================================================

# Full text search: the contents of a document

- If we searching in a system that indexes the full text, then we are
  searching against all the language in that text
- This is useful to know because when we construct queries, it might be helpful
  to imagine how langauge is used
- Example: web pages and sites, full text databases

Surrogate Records Search
========================================================

# Surrogate records search: the bibliographic description of a document

- However, if we are searching a system that contains document surrogates, then
  we are searching against pre-set fields of information about the documents.
  These pre-set fields are displayed in surrogate records, and they contain 
  what's called descriptive and access metadata about the documents of interest.
  This is information about the documents and information about how to access
  the documents.
- Examples: author names, titles, subject headings in an online catalog
- Examples: keywords, abstracts, and more in an Abstract & Index (A&I) database

Searching Against the Index
========================================================

- The indexes that we search against are built differently depending on whether
  the system contains full text, surrogate records, or both.
- Indexes of full text systems, like the web, are based on mathematical, statistical, and
  logical rules surrounding various signals in the text. One basic example is
  term frequency, where the frequency of a term in a document indicates
  something *about* the document; that is, its *aboutness*. 
- Indexes of surrogate records, like an A&I system, are more contolled and
  structured simply because the surrogates are fixed and designed. Oftentimes,
  there will also be rules about how they are written.
  
Toy Example 1: Term Frequency - Inverse Document Frequency
========================================================

Let's take a look at our toy models. In the first model, imagine that we've done
a simple search on some popular search engine using a single term. Let's say our
term is "librarian".

Toy Example 1: Searching a Full Text System
========================================================

When all documents are the same size, the rank order is simply based on the
rank number of keywords per documentdf


```r
docid <- seq(1,10)

set.seed(1000)
tf    <- sample(0:9, replace=TRUE)

wc    <- rep(100, 10)
tfn   <- tf/wc
idf   <- log(length(docid) / length(tf[tf > 0]))
tfidf <- round(tfn * idf, 4)

tfidf.df <- data.frame(cbind(docid, tf, wc, tfn, idf, tfidf))
tfidf.df[order(-tfidf),]
```

```
   docid tf  wc  tfn       idf  tfidf
2      2  7 100 0.07 0.1053605 0.0074
7      7  7 100 0.07 0.1053605 0.0074
4      4  6 100 0.06 0.1053605 0.0063
5      5  5 100 0.05 0.1053605 0.0053
8      8  5 100 0.05 0.1053605 0.0053
1      1  3 100 0.03 0.1053605 0.0032
9      9  2 100 0.02 0.1053605 0.0021
10    10  2 100 0.02 0.1053605 0.0021
3      3  1 100 0.01 0.1053605 0.0011
6      6  0 100 0.00 0.1053605 0.0000
```

Toy Example 2: Searching a Full Text System
========================================================

When documents differ in length, then the rank order varies based on tf*idf score


```r
docid <- seq(1,10)

set.seed(1000)
tf    <- sample(0:9, replace=TRUE)

set.seed(1000)
wc    <- sample.int(sample(100:1000), replace=FALSE, 10)
tfn   <- tf/wc
idf   <- log(length(docid) / length(tf[tf > 0]))
tfidf <- round(tfn * idf, 4)

tfidf.df <- data.frame(cbind(docid, tf, wc, tfn, idf, tfidf))
tfidf.df[order(-tfidf),]
```

```
   docid tf  wc         tfn       idf  tfidf
1      1  3  41 0.073170732 0.1053605 0.0077
7      7  7 138 0.050724638 0.1053605 0.0053
5      5  5 124 0.040322581 0.1053605 0.0042
9      9  2  71 0.028169014 0.1053605 0.0030
8      8  5 183 0.027322404 0.1053605 0.0029
2      2  7 279 0.025089606 0.1053605 0.0026
4      4  6 249 0.024096386 0.1053605 0.0025
10    10  2 256 0.007812500 0.1053605 0.0008
3      3  1 231 0.004329004 0.1053605 0.0005
6      6  0 247 0.000000000 0.1053605 0.0000
```

Toy Example 3: Searching a Surrogate Record System
========================================================

Boolean searching




```r
cats   <- rep("cat", 10)
dogs   <- rep("dog", 10)
snakes <- rep("snake", 10)

set.seed(1)
one <- sample(c(cats, dogs, snakes), 10)

set.seed(2)
two <- sample(c(cats, dogs, snakes), 10)

set.seed(3)
three <- sample(c(cats, dogs, snakes), 10)

docid <- seq(1,10)
cds <- data.frame(cbind(docid, one, two, three))
cds
```

```
   docid   one   two three
1      1   cat   cat   cat
2      2   dog snake snake
3      3   dog   dog   dog
4      4 snake   cat   cat
5      5   cat snake   dog
6      6 snake snake snake
7      7 snake   cat   cat
8      8   dog   dog   cat
9      9   dog   dog   dog
10    10   cat   dog   dog
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Retrieve all records that contain cat in first column


```r
cds
```

```
   docid   one   two three
1      1   cat   cat   cat
2      2   dog snake snake
3      3   dog   dog   dog
4      4 snake   cat   cat
5      5   cat snake   dog
6      6 snake snake snake
7      7 snake   cat   cat
8      8   dog   dog   cat
9      9   dog   dog   dog
10    10   cat   dog   dog
```

```r
filter(cds, one == "cat")
```

```
  docid one   two three
1     1 cat   cat   cat
2     5 cat snake   dog
3    10 cat   dog   dog
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Retrieve all records that do not contain cat in the first column


```r
cds
```

```
   docid   one   two three
1      1   cat   cat   cat
2      2   dog snake snake
3      3   dog   dog   dog
4      4 snake   cat   cat
5      5   cat snake   dog
6      6 snake snake snake
7      7 snake   cat   cat
8      8   dog   dog   cat
9      9   dog   dog   dog
10    10   cat   dog   dog
```

```r
filter(cds, one != "cat")
```

```
  docid   one   two three
1     2   dog snake snake
2     3   dog   dog   dog
3     4 snake   cat   cat
4     6 snake snake snake
5     7 snake   cat   cat
6     8   dog   dog   cat
7     9   dog   dog   dog
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Boolean AND: More terms reduces the results


```r
cds
```

```
   docid   one   two three
1      1   cat   cat   cat
2      2   dog snake snake
3      3   dog   dog   dog
4      4 snake   cat   cat
5      5   cat snake   dog
6      6 snake snake snake
7      7 snake   cat   cat
8      8   dog   dog   cat
9      9   dog   dog   dog
10    10   cat   dog   dog
```

```r
filter(cds, two == "dog")
```

```
  docid one two three
1     3 dog dog   dog
2     8 dog dog   cat
3     9 dog dog   dog
4    10 cat dog   dog
```

```r
filter(cds, two == "dog" & three == "cat")
```

```
  docid one two three
1     8 dog dog   cat
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Boolean NOT: retrieve all records that contain dog in first and second columns 
but not the third column


```r
cds
```

```
   docid   one   two three
1      1   cat   cat   cat
2      2   dog snake snake
3      3   dog   dog   dog
4      4 snake   cat   cat
5      5   cat snake   dog
6      6 snake snake snake
7      7 snake   cat   cat
8      8   dog   dog   cat
9      9   dog   dog   dog
10    10   cat   dog   dog
```

```r
filter(cds, one == "dog" & two == "dog" & three != "dog")
```

```
  docid one two three
1     8 dog dog   cat
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Boolean OR: captures more


```r
filter(cds, two == "dog" & three == "cat")
```

```
  docid one two three
1     8 dog dog   cat
```

```r
filter(cds, two == "dog" | three == "cat")
```

```
  docid   one two three
1     1   cat cat   cat
2     3   dog dog   dog
3     4 snake cat   cat
4     7 snake cat   cat
5     8   dog dog   cat
6     9   dog dog   dog
7    10   cat dog   dog
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Why is *snake* appearing in the first list? Because order of operation matters.
Note what happens when I add parentheses -- we have changed the order of
operation and so it stands out.


```r
filter(cds, one != "snake" & two == "dog" | three == "cat")
```

```
  docid   one two three
1     1   cat cat   cat
2     3   dog dog   dog
3     4 snake cat   cat
4     7 snake cat   cat
5     8   dog dog   cat
6     9   dog dog   dog
7    10   cat dog   dog
```

```r
filter(cds, one != "snake" & (two == "dog" | three == "cat"))
```

```
  docid one two three
1     1 cat cat   cat
2     3 dog dog   dog
3     8 dog dog   cat
4     9 dog dog   dog
5    10 cat dog   dog
```

Toy Example 3: Boolean Queries on Surrogate Records
========================================================

These two are the same search, because order of operation matters


```r
filter(cds, one != "snake" & two == "dog" | three == "cat")
```

```
  docid   one two three
1     1   cat cat   cat
2     3   dog dog   dog
3     4 snake cat   cat
4     7 snake cat   cat
5     8   dog dog   cat
6     9   dog dog   dog
7    10   cat dog   dog
```

```r
filter(cds, (one != "snake" & two == "dog") | three == "cat")
```

```
  docid   one two three
1     1   cat cat   cat
2     3   dog dog   dog
3     4 snake cat   cat
4     7 snake cat   cat
5     8   dog dog   cat
6     9   dog dog   dog
7    10   cat dog   dog
```


Toy Example 3: Boolean Queries on Surrogate Records
========================================================

Final thoughts:

- Instead of *cats*, how would these systems resolve synonymous, related, broader,
or narrower terms, such as *felines*, *tabbies*, or even *lions*; and relatedly,
instead of *dogs*, how would these systems resolve *schnauzers*, *German Shepherds*, 
or *mutts*.

Depending on the system, full text or surrogate, it'll be resolved differently. 
In a full text system, it will be resolved by perhaps how documents link to each
other, how words are used (semantics), and how terms are connected to each other.
In a surrogate system, it might be resolved through controlled vocabularly,
thesuari, and other such methods.

- Notice that the *docid* order is always the same in the above toy models. Why?

Because Boolean search simply subsets listwise, based on the default order
of the system, and doesn't rank.
