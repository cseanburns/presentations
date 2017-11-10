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
  bit of overlap, and other issues are at play, too.
- I will also present a couple of toy information retrieval models. This means
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
  the actual documents or the surrogate records (e.g., bibliographic records).
- For example, Google does not re-search every single web page on the entire internet
  each time someone enters a query in Google; rather, queries are searched against a stored index,
  albeit one that Google is constantly building and modifying based on the
  dynamics of the web and internet and on its own search engineering developments.

Full Text Search
========================================================

# Full text search: the contents of a document

- If we search in a system that indexes the full text, then we are
  searching against all the language in that text
- This is useful to know because when we construct queries, it might be helpful
  to imagine how langauge is used
- It also means that we may retrieve documents that do not exactly match our query
- Example: web pages and sites, full text databases

Surrogate Records Search
========================================================

# Surrogate records search: the bibliographic description of a document

- If we search a system that contains document surrogates, or representations of 
  those documents, then we search against pre-set fields of information about the documents.
  These pre-set fields compose surrogate records, and they contain 
  what is called descriptive and access metadata about the documents of interest.
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
  
Toy Model 1: Full Text Search
========================================================

# Our first toy model

- In the first model, we begin by examining full text search.
- Imagine that we've entered a simple search query into some popular search
  engine using a single term. If it helps, imagine our search term is *librarian*.
- By setting up a toy model (i.e., an oversimplified model), we can start to acquire
  an intuition of how information retrieval works.

Toy Model 1: Aboutness 
========================================================

# Aboutness

- It seems obvious that if I searched for a term, then I would want results to 
  documents (e.g., web pages, etc.) that contain my term.
- As a result, we might assume that how or how often a term is used in a document
  must say something about what that document is about. Therefore, we connect 
  the idea of *aboutness* of a document to the appearance of terms in a document.
- Ex: if a document contains the word *librarian*, then we might guess that the
  document is about librarians and we might become more confident in our guess
  the more that word appears in that document.
- Consequently, if a document has a word count of 500 and the term *librarian* appears
  in that document 10 times, then we might likely say that the document is more *about*
  librarians than a document with also a word count of 500 but where the term *librarian*
  only appears two times.
  
Toy Model 1: Term Frequency - Inverse Document Frequency
========================================================

# tf-idf

- Therefore, one way to measure the importance of a term in a document is to measure the 
  frequency of that term in a document
- Let's imagine that entire web only contains ten documents.
- Let's also imagine that each document contains the exact same number of words
  (n = 100).
- Under this scenario, when all documents are of equal length, then the rank order
  of search results is simply a function of the frequency of the search term 
  in each document: i.e., the more a term appears in the document, the higher its rank,
  or its relevance.

Toy Model 1: Searching a Full Text System
========================================================

## Demonstration 


```
   docid term.freq word.count term.freq.norm inv.doc.freq   rank
2      2         7        100           0.07    0.1053605 0.0074
7      7         7        100           0.07    0.1053605 0.0074
4      4         6        100           0.06    0.1053605 0.0063
5      5         5        100           0.05    0.1053605 0.0053
8      8         5        100           0.05    0.1053605 0.0053
1      1         3        100           0.03    0.1053605 0.0032
9      9         2        100           0.02    0.1053605 0.0021
10    10         2        100           0.02    0.1053605 0.0021
3      3         1        100           0.01    0.1053605 0.0011
6      6         0        100           0.00    0.1053605 0.0000
```

Toy Example 2: Searching a Full Text System
========================================================

## Explanation

- It's not realistic to assume that each document in a set will contain the
  exact word count, even in our toy model.
- This is where the second part of the *tf-idf* algorithm helps 
- The *idf* considers the presence and the lack of presence of a keyword within
  the entire set of or collection of documents so that the keyword within the documents
  will remain effective as the size of the collection changes over time (gets 
  bigger or smaller).
- Imagine, for example, a collection of 100 documents. If only five of those documents
  contains the term *psychology*, then a search amongst those documents for that
  term can be effective. However, if all 100 documents contained that term, then
  the term is no longer effective as a search term. 

Toy Example 2: Searching a Full Text System
========================================================

## Demonstration 

When documents differ in length, then the rank order varies based on tf*idf score


```
   docid term.freq word.count term.freq.norm inv.doc.freq   rank
1      1         3         41    0.073170732    0.1053605 0.0077
7      7         7        138    0.050724638    0.1053605 0.0053
5      5         5        124    0.040322581    0.1053605 0.0042
9      9         2         71    0.028169014    0.1053605 0.0030
8      8         5        183    0.027322404    0.1053605 0.0029
2      2         7        279    0.025089606    0.1053605 0.0026
4      4         6        249    0.024096386    0.1053605 0.0025
10    10         2        256    0.007812500    0.1053605 0.0008
3      3         1        231    0.004329004    0.1053605 0.0005
6      6         0        247    0.000000000    0.1053605 0.0000
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
