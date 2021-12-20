# ❓ FAQ

## **When is my work checked?**

Every 5 minutes**.**

## I got `null` score, what happened?

There is several common reason:

* Your prediction CSV file does not have exactly the same number of rows as the test set.
* Your prediction CSV columns' name are not `target_r`, `target_g`, `target_b`.
* Your values are not in the range `(0, 1)`.

## **What is the metric for evaluating the models?**

Its the [spearman ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html).



