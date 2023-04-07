# presentations

Source code for presentations by directory name:

- 600_information_retrieval_pres: introduction to IR for a
  library science course
- r-introduction-2019: introduction to R programming for
  graduate students and faculty
- sig-con-asist-2012: comedic/satirical presentation given
  at ASIST annual conference, Baltimore, 2012, SIG-CON
- web-search-2018: intermediate Google searching
  presentation for an undergraduate class
- ict301: introduction to spatial analysis for an
  undergraduate research methods course
- agu-2021: Presentation given at the American Geophysical
  Untion in December 2021
- asist-km-2021: Presentation given at SIG-KM/ASIST in April
  2021
- cci-research-presentation-2020: MEDLINE research
  presentation given to College in September 2020
- ci-online-learning-pres-2020: Presentation on discussion
  board methods for engagement given to College in July 2020
  to help instructors new to online instruction (because of
  the pandemic).

I'm using the following command to convert a Markdown file
to an HTML5 file that includes a table of contents at the
top of the page:

```
pandoc -s -t html5 --toc file.md -o file.html
```

I use the following command to generate HTML slides:

```
pandoc -s -i -t slidy file.md -o file.html
```
