# FAQ

## **When is my work checked?**

Every 5 minutes.

## I got `null` score, what happened?

There is several common reason:

* Your prediction CSV file does not have exactly the same number of rows as the test set.
* Your prediction CSV columns' name are not `target_r`, `target_g`, `target_b`.
* Your values are not in the range `(0, 1)`.

## **What is the metric for evaluating the models?**

Its the [spearman ratio](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.spearmanr.html) X 100.

## I am restricted, what does it mean?

When you are not respecting the rules or our system has detected a strange behaviour from you, you will be 'restricted' until the issue has been resolved!

You will be able to browse and use the site normally, but you will not be able to submit any work.

## What does mean \`_This round is an inception_\`

It's a legacy term. Before we were not able to update the strategy as fast as it was played. So it happened that we were playing the exact same data without updating for couple of weeks. Dataset are now always inceptions.
