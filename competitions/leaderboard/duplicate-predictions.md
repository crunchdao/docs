---
description: How is the duplicate badge triggered.
---

# Duplicate Predictions

Some competitions have a duplicate detection feature that flags all models that have a prediction correlation above 95%[^1] and makes them ineligible for prizes.

The correlation is computed using [`pandas.DataFrame.corr(method="spearman")`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.corr.html).

#### Grouping

The prediction are first grouped between:

* user, to avoid duplication between multiple models
* team member, to avoid duplicate between all models of every team member

The correlation function is then called for each prediction against each other.

#### Keeping

Predictions are then grouped into correlated pairs to isolate them.

The first model created in a pair is always retained. This makes the choice deterministic. Other models are treated as copies of the first.

e.g.:

* If <mark style="color:blue;">A</mark> & <mark style="color:purple;">B</mark> are considered duplicates, and <mark style="color:orange;">C</mark> & <mark style="color:yellow;">D</mark> are also considered duplicates, then <mark style="color:blue;">A</mark> and <mark style="color:orange;">C</mark> will be retained and <mark style="color:purple;">B</mark> and <mark style="color:yellow;">D</mark> will have the duplicate badge.
* However, if <mark style="color:purple;">B</mark> & <mark style="color:orange;">C</mark> are also considered duplicates, then only <mark style="color:blue;">A</mark> will be retained. <mark style="color:purple;">B</mark>, <mark style="color:orange;">C</mark>, <mark style="color:yellow;">D</mark> will have the duplicate badge.

[^1]: Some competitions may explicitly state a different threshold.
