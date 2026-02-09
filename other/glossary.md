---
description: Technical definitions of platform concepts.
---

# Glossary

## Competition

A structured, (often) time-bound, incentive-driven challenge where participants (typically data scientists) compete to solve a specific problem or achieve a defined objective using data, algorithms, or machine learning models.

## Crunch

A period during which the leaderboard can be updated and compute quotas can be refreshed.

This period usually lasts for a week, but it can be arbitrary.

{% hint style="info" %}
Crunch is the technical term. It is also used by marketers to refer to a [Competition](glossary.md#competition).
{% endhint %}

## Submission Phase

Part of the competition while submission is allowed. The data does not change and the quota is regularly reset.

It is also known as the public leaderboard.

## Out-of-Sample Phase

Part of the competition while scoring is happening on an out-of-sample phase. The data does change regularly, and no modification is allowed to your code or selection.

It is also known as the private leaderboard.

## Submission

Similar to a Git commit, a submission represents a frozen version of a user's code. Once on the platform, no file can be updated and the message cannot be changed.

## Run

Similar to GitHub Actions, a run is when a user's code is executed in the cloud environment. The environment is heavily restricted. The user's code must follow the [code-interface.md](../competitions/participate/code-interface.md "mention") in order to run properly.

## Prediction

A prediction is the output of a [Run](glossary.md#run).
