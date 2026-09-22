---
description: Medals and Titles on Crunch.
---

# Medals & Titles

Similarely to Kaggle, Crunch has its own title system to ensure that our best Crunchers are properly recognized.

The actual implementation is [available on GitHub](https://github.com/crunchdao/crunch-titles).

## How does it work?

Crunch has a lot of different types of competitions, each of which must be interpreted differently.

### Scoring Unit

A leaderboard is made of a sequence of scoring units over time. What a unit represents depends on the competition type:

* **Regular**: the single round's Out-of-Sample.
* **Continuous**: a round, one for each Out-of-Sample.
* **Real-Time**: a weekly payout.

A Cruncher's score for a leaderboard is the average of their rank across every unit they participated in.

#### What is the “best” rank within a unit?

* If the user is on a team, either the team leader's rank (if enabled) or the rank of the best team member is shared with teammates.
* If the user has multiple models, the rank of their best model is used.

### Regular Competitions

Also known as one-shot competitions, those are our big prizes yearly competitions where participants submit for a long period and only get a final Out-of-Sample.

Since a one-shot competitions has a single round, the rank is used directly.

#### What about disqualified models?

Models may become ineligible for rewards for several reasons, such as non-determinism or failing to complete [mandatory steps](#user-content-fn-1)[^1]. An ineligible model does not have an average rank and is ignored as a result.

### Continuous Competitions

Some competitions never end, or at least have multiple Out-of-Sample periods throughout their duration. This is why we call them continuous.

Those competitions are divided by year (e.g. DataCrunch (v1) 2024 and DataCrunch (v1) 2025), which results in two unique rankings. Competitions are only considered at the beginning of the following year: for example, DataCrunch (v1) 2025's medals will be calculated in January 2026.

A Cruncher's must appear on 25% of the leaderboard.

#### Why a minimum participation requirement?

A model can have one lucky break and then disappear forever. To be fair to those with good rankings but lower ranks than those lucky events, the average ranking only counts if they have participated in at least 25% of the leaderboard's total units. Otherwise, it is ignored.

Example for a competition that ran for 6 months (26 weeks of leaderboards):

* Enzo ranked 3 (12%) weeks. Enzo will be ignored.
* Jean ranked 20 (77%) weeks. Jean will be taken into account.

### Real-Time Competitions

Real-Time competitions are a special case of Continuous competitions where the leaderboard positions are too dynamic to be used directly; instead, weekly payouts are used, in the same way as the [Global Leaderboard](global-leaderboard.md).

### Follow-up Competitions

There is one exception: the best ranking of the Structural Break Open Benchmark has been merged with the original Structural Break competition.

This means that participants in either competition will receive the highest ranking from the two competitions, as we do not believe that distributing medals for a reopened competition based on the same problem and data is worthwhile.

## Medals

These averaged ranks are then used to distribute medals:

* <mark style="color:yellow;">🥇Gold</mark>: ended up 1st
* <mark style="color:blue;">🥈</mark><mark style="color:$info;">Silver</mark>: ended up 2nd
* <mark style="color:orange;">🥉Bronze</mark>: ended up 3rd
* <mark style="color:blue;">⭐10%</mark>: ended up in the top 10%

A Cruncher can only receive one medal per competition. A Cruncher on a team will receive the same medal as their leader (if enabled) or their best-performing member.

## Titles

Titles are awarded based on the number of medals received:

* Grandmaster: at least two <mark style="color:green;">🏅Podium</mark>
* Master: <mark style="color:green;">🏅Podium</mark> AND <mark style="color:blue;">⭐10%</mark>
* Expert: <mark style="color:green;">🏅Podium</mark>
* Ranked: <mark style="color:blue;">⭐10%</mark>
* Builder: submitted to three different competitions
* Contributor: submitted to any competition
* Novice: default title for new users

A Cruncher is awarded only the highest title they qualify for, and cannot be downgraded, except in the case of cheating or a critical bug in the implementation.

{% hint style="info" %}
The <mark style="color:green;">🏅Podium</mark> refers to either one <mark style="color:yellow;">🥇Gold</mark>, one <mark style="color:blue;">🥈</mark><mark style="color:$info;">Silver</mark> or one <mark style="color:orange;">🥉Bronze</mark> medal.
{% endhint %}

[^1]: Broad competitions required participating to the Peer Review phase.
