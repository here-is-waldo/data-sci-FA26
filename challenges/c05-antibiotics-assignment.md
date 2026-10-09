Antibiotics
================
Luca Odio
2026-10-5

*Purpose*: Creating effective data visualizations is an *iterative*
process; very rarely will the first graph you make be the most
effective. The most effective thing you can do to be successful in this
iterative process is to *try multiple graphs* of the same data.

Furthermore, judging the effectiveness of a visual is completely
dependent on *the question you are trying to answer*. A visual that is
totally ineffective for one question may be perfect for answering a
different question.

In this challenge, you will practice *iterating* on data visualization,
and will anchor the *assessment* of your visuals using two different
questions.

*Note*: Please complete your initial visual design **alone**. Work on
both of your graphs alone, and save a version to your repo *before*
coming together with your team. This way you can all bring a diversity
of ideas to the table!

<!-- include-rubric -->

# Grading Rubric

<!-- -------------------------------------------------- -->

Unlike exercises, **challenges will be graded**. The following rubrics
define how you will be graded, both on an individual and team basis.

## Individual

<!-- ------------------------- -->

| Category | Needs Improvement | Satisfactory |
|----|----|----|
| Effort | Some task **q**’s left unattempted | All task **q**’s attempted |
| Observed | Did not document observations, or observations incorrect | Documented correct observations based on analysis |
| Supported | Some observations not clearly supported by analysis | All observations clearly supported by analysis (table, graph, etc.) |
| Assessed | Observations include claims not supported by the data, or reflect a level of certainty not warranted by the data | Observations are appropriately qualified by the quality & relevance of the data and (in)conclusiveness of the support |
| Specified | Uses the phrase “more data are necessary” without clarification | Any statement that “more data are necessary” specifies which *specific* data are needed to answer what *specific* question |
| Code Styled | Violations of the [style guide](https://style.tidyverse.org/) hinder readability | Code sufficiently close to the [style guide](https://style.tidyverse.org/) |

## Submission

<!-- ------------------------- -->

Make sure to commit both the challenge report (`report.md` file) and
supporting files (`report_files/` folder) when you are done! Then submit
a link to Canvas. **Your Challenge submission is not complete without
all files uploaded to GitHub.**

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.3     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(ggrepel)
```

*Background*: The data\[1\] we study in this challenge report the
[*minimum inhibitory
concentration*](https://en.wikipedia.org/wiki/Minimum_inhibitory_concentration)
(MIC) of three drugs for different bacteria. The smaller the MIC for a
given drug and bacteria pair, the more practical the drug is for
treating that particular bacteria. An MIC value of *at most* 0.1 is
considered necessary for treating human patients.

These data report MIC values for three antibiotics—penicillin,
streptomycin, and neomycin—on 16 bacteria. Bacteria are categorized into
a genus based on a number of features, including their resistance to
antibiotics.

``` r
## NOTE: If you extracted all challenges to the same location,
## you shouldn't have to change this filename
filename <- "./data/antibiotics.csv"

## Load the data
df_antibiotics <- read_csv(filename)
```

    ## Rows: 16 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): bacteria, gram
    ## dbl (3): penicillin, streptomycin, neomycin
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
df_antibiotics %>% knitr::kable()
```

| bacteria                        | penicillin | streptomycin | neomycin | gram     |
|:--------------------------------|-----------:|-------------:|---------:|:---------|
| Aerobacter aerogenes            |    870.000 |         1.00 |    1.600 | negative |
| Brucella abortus                |      1.000 |         2.00 |    0.020 | negative |
| Bacillus anthracis              |      0.001 |         0.01 |    0.007 | positive |
| Diplococcus pneumonia           |      0.005 |        11.00 |   10.000 | positive |
| Escherichia coli                |    100.000 |         0.40 |    0.100 | negative |
| Klebsiella pneumoniae           |    850.000 |         1.20 |    1.000 | negative |
| Mycobacterium tuberculosis      |    800.000 |         5.00 |    2.000 | negative |
| Proteus vulgaris                |      3.000 |         0.10 |    0.100 | negative |
| Pseudomonas aeruginosa          |    850.000 |         2.00 |    0.400 | negative |
| Salmonella (Eberthella) typhosa |      1.000 |         0.40 |    0.008 | negative |
| Salmonella schottmuelleri       |     10.000 |         0.80 |    0.090 | negative |
| Staphylococcus albus            |      0.007 |         0.10 |    0.001 | positive |
| Staphylococcus aureus           |      0.030 |         0.03 |    0.001 | positive |
| Streptococcus fecalis           |      1.000 |         1.00 |    0.100 | positive |
| Streptococcus hemolyticus       |      0.001 |        14.00 |   10.000 | positive |
| Streptococcus viridans          |      0.005 |        10.00 |   40.000 | positive |

# Visualization

<!-- -------------------------------------------------- -->

### **q1** Prototype 5 visuals

To start, construct **5 qualitatively different visualizations of the
data** `df_antibiotics`. These **cannot** be simple variations on the
same graph; for instance, if two of your visuals could be made identical
by calling `coord_flip()`, then these are *not* qualitatively different.

For all five of the visuals, you must show information on *all 16
bacteria*. For the first two visuals, you must *show all variables*.

*Hint 1*: Try working quickly on this part; come up with a bunch of
ideas, and don’t fixate on any one idea for too long. You will have a
chance to refine later in this challenge.

*Hint 2*: The data `df_antibiotics` are in a *wide* format; it may be
helpful to `pivot_longer()` the data to make certain visuals easier to
construct.

#### Visual 1 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

``` r
# modify dataframe (consolidate MIC, extract genus)
df_antibiotics_mod <-
  df_antibiotics %>%
  pivot_longer(
    cols = !bacteria & !gram,
    names_to = "antibiotic",
    values_to = "MIC"
  ) %>%
  mutate(genus = word(bacteria, 1))

df_antibiotics_mod
```

    ## # A tibble: 48 × 5
    ##    bacteria              gram     antibiotic       MIC genus      
    ##    <chr>                 <chr>    <chr>          <dbl> <chr>      
    ##  1 Aerobacter aerogenes  negative penicillin   870     Aerobacter 
    ##  2 Aerobacter aerogenes  negative streptomycin   1     Aerobacter 
    ##  3 Aerobacter aerogenes  negative neomycin       1.6   Aerobacter 
    ##  4 Brucella abortus      negative penicillin     1     Brucella   
    ##  5 Brucella abortus      negative streptomycin   2     Brucella   
    ##  6 Brucella abortus      negative neomycin       0.02  Brucella   
    ##  7 Bacillus anthracis    positive penicillin     0.001 Bacillus   
    ##  8 Bacillus anthracis    positive streptomycin   0.01  Bacillus   
    ##  9 Bacillus anthracis    positive neomycin       0.007 Bacillus   
    ## 10 Diplococcus pneumonia positive penicillin     0.005 Diplococcus
    ## # ℹ 38 more rows

``` r
  # this is how ZDR would do it:
  # mutate(genus = str_extract(bacteria, "^\\w+"))
```

``` r
# WRITE YOUR CODE HERE
max_MIC <- 0.1

df_antibiotics_mod %>%
  ggplot(aes(
    x = MIC, 
    y = bacteria, 
    color = gram
  )) +
  scale_x_log10() +
  facet_grid(cols = vars(antibiotic)) +
  # facet_grid(antibiotic ~ .) +
  geom_point(size = 2) +
  geom_vline(
    xintercept = max_MIC,
    linetype = "dotted"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.1-1.png)<!-- -->

#### Visual 2 (All variables)

In this visual you must show *all three* effectiveness values for *all
16 bacteria*. This means **it must be possible to identify each of the
16 bacteria by name.** You must also show whether or not each bacterium
is Gram positive or negative.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
set.seed(135)

df_antibiotics_mod %>%
  ggplot(aes(
    x = MIC,
    y = antibiotic,
    color = bacteria,
    shape = gram
  )) +
  scale_x_log10() +
  geom_jitter(width = 0, height = 0.08) +
  geom_vline(
    xintercept = max_MIC,
    linetype = "dashed"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.2-1.png)<!-- -->

#### Visual 3 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics_mod %>%
  filter(antibiotic == "penicillin") %>%
  ggplot(aes(
    x = MIC,
    y = reorder(bacteria, MIC),
    color = genus,
    shape = gram
  )) +
  scale_x_log10() +
  geom_point(size = 2) +
  geom_vline(
    xintercept = max_MIC,
    linetype = "dashed"
  ) +
  labs(
    title = "MIC by Bacteria for Penicillin",
    x = "MIC",
    y = "Bacteria name"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.3-1.png)<!-- -->

#### Visual 4 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics %>%
  ggplot(aes(
    x = penicillin,
    y = neomycin,
    color = bacteria,
    shape = gram
  )) +
  scale_x_log10() +
  scale_y_log10() +
  geom_vline(
    xintercept = max_MIC,
    linetype = "dashed"
  ) +
  geom_hline(
    yintercept = max_MIC,
    linetype = "dashed"
  ) +
  geom_point(size = 2) +
  labs(
    x = "Penicillin MIC",
    y = "Neomycin MIC"
  )
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.4-1.png)<!-- -->

#### Visual 5 (Some variables)

In this visual you may show a *subset* of the variables (`penicillin`,
`streptomycin`, `neomycin`, `gram`), but you must still show *all 16
bacteria*.

Note that your visual must be *qualitatively different* from *all* of
your other visuals.

``` r
# WRITE YOUR CODE HERE
df_antibiotics %>%
  ggplot(aes(
    x = streptomycin,
    y = neomycin,
    color = bacteria,
    shape = gram
  )) +
  scale_x_log10() +
  scale_y_log10() +
  geom_point(size = 2) 
```

![](c05-antibiotics-assignment_files/figure-gfm/q1.5-1.png)<!-- -->

### **q2** Assess your visuals

There are **two questions** below; use your five visuals to help answer
both Guiding Questions. Note that you must also identify which of your
five visuals were most helpful in answering the questions.

*Hint 1*: It’s possible that *none* of your visuals is effective in
answering the questions below. You may need to revise one or more of
your visuals to answer the questions below!

*Hint 2*: It’s **highly unlikely** that the same visual is the most
effective at helping answer both guiding questions. **Use this as an
opportunity to think about why this is.**

#### Guiding Question 1

> How do the three antibiotics vary in their effectiveness against
> bacteria of different genera and Gram stain?

*Observations* - What is your response to the question above?

Penicillin is effective (MIC \<0.1) against almost all (all but one) of
the gram stain positive bacteria, and none of the gram stain negative
bacteria. Neomycin and Streptomycin don’t have such clearly delineated
results. The fewest bacteria strains are susceptible to Streptomycin,
and gram stain positive bacteria have a range of MICs. Gram stain
negative bacteria are a little more grouped around 0.1 MIC, but the log
scale makes it difficult to evaluate.

Regarding genera, it is difficult to discern patterns because there are
only three genera with multiple bacteria strains in this data set. The
genera that do have data on multiple bacteria have few data points (2-3)
so it is hard to tell what’s a pattern and what’s coincidence. It
appears that the bacterial responses to Penicillin may be more
consistent across genera than for Neomycin or Streptomycin, but more
data (within few genera) should be used to better understand genus-level
similarities in MIC.

Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? Why?

My visual 1. It shows the distribution of MIC for each bacteria and each
antibiotic, and uses color to indicate the gram stain status. It is easy
to see the distribution of values for any given antibiotic and visually
discern patterns relating to gram stain. The bacteria are listed by
name, so bacteria of the same genus are sequential, which is helpful for
analysis, but not overly evident information at first glance.

#### Guiding Question 2

In 1974 *Diplococcus pneumoniae* was renamed *Streptococcus pneumoniae*,
and in 1984 *Streptococcus fecalis* was renamed *Enterococcus fecalis*
\[2\].

> Why was *Diplococcus pneumoniae* was renamed *Streptococcus
> pneumoniae*?

*Observations* - What is your response to the question above?

Diplococcus pneumoniae was renamed Streptococcus pneumoniae because
further research found that the bacteria displayed chain-forming
behavior (a characteristic of the Streptococcus genus) in liquid media.

Which of your visuals above (1 through 5) is **most effective** at
helping to answer this question? Why?

None of my visuals would conclusively suggest this outcome. The most
effective visual was visual 4, where Diplococcus pneumoniae is clustered
with two Streptococcus bacteria on a Neomycin MIC vs Pencillin MIC
log-log plot. This indicates that Diplococcus pneumoniae shared similar
sensitivities to both antibiotics as two of the three Streptococcus
bacteria. The third Streptococcus bacteria that was not included in the
cluster was Streptococcus fecalis, which was later reclassified as well.
The antibiotics operate differently, and target different aspects of a
bacteria’s form (cell wall, ribosome functionality). The fact that
Diplococcus pneumoniae shared such similar responses to the true
Streptococcus bacteria is a clue that there may be greater similarities
in biological form than suggested by the initial genus classification.

# References

<!-- -------------------------------------------------- -->

\[1\] Neomycin in skin infections: A new topical antibiotic with wide
antibacterial range and rarely sensitizing. Scope. 1951;3(5):4-7.

\[2\] Wainer and Lysen, “That’s Funny…” *American Scientist* (2009)
[link](https://www.americanscientist.org/article/thats-funny)
