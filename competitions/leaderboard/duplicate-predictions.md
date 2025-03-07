---
description: How is the duplicate badge triggered.
---

# Duplicate Predictions

Some competitions have a duplicate detection feature that flags all models that have a prediction correlation above 95%[^1] and makes them ineligible for prizes.

The correlation is computed using [`pandas.DataFrame.corr(method="spearman")`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.corr.html).

### Grouping

The prediction are first grouped between:

* user, to avoid duplication between multiple models
* team, to avoid duplicate between all models of every team member

The correlation function is then called for each prediction against each other.

### Keeping

Predictions are then grouped into correlated pairs to isolate them.

The best[^2] model in a pair is always kept. Other models are treated as copies.

e.g.:

* If <mark style="color:blue;">A</mark> & <mark style="color:purple;">B</mark> are considered duplicates, and <mark style="color:orange;">C</mark> & <mark style="color:yellow;">D</mark> are also considered duplicates, then <mark style="color:blue;">A</mark> and <mark style="color:orange;">C</mark> will be retained and <mark style="color:purple;">B</mark> and <mark style="color:yellow;">D</mark> will have the duplicate badge.
* However, if <mark style="color:purple;">B</mark> & <mark style="color:orange;">C</mark> are also considered duplicates, then only <mark style="color:blue;">A</mark> will be retained. <mark style="color:purple;">B</mark>, <mark style="color:orange;">C</mark>, <mark style="color:yellow;">D</mark> will have the duplicate badge.
* In some cases, even if <mark style="color:blue;">A</mark> and <mark style="color:purple;">D</mark> are not considered to be duplicates, because of the link from <mark style="color:blue;">A</mark> to <mark style="color:purple;">B</mark> to <mark style="color:orange;">C</mark> to <mark style="color:yellow;">D</mark>, the duplicate badge is still displayed.

[^1]: This threshold can be changed at any time during the competition and at the discretion of the organizers.

[^2]: Old competition used the first model created.
