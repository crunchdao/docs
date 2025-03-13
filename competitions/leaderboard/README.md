# Leaderboard

The only requirement to appear on the leaderboard is to have a successful [Run](../../other/glossary.md#run).

Being on the leaderboard makes you eligible for rewards based on the rewards system of the crunch you are participating in.

## Submission Phase

During the [Submission Phase](../../other/glossary.md#submission-phase), the leaderboard is updated when a [Run](../../other/glossary.md#run) is scored.

If there is no public leaderboard, the leaderboard will be updated when a [Prediction](../../other/glossary.md#prediction) is validated to make sure it is ready for scoring.

## Out-of-Sample Phase

During the [Out-of-Sample Phase](../../other/glossary.md#out-of-sample-phase), the leaderboard is only updated once when all the [Run](../../other/glossary.md#run) that are running on unseen data are scored.

## Columns

Only a subset of columns will be visible, depending on the current state of the crunch.

<table><thead><tr><th width="235">Name</th><th>Description</th></tr></thead><tbody><tr><td>Rank</td><td>Rank of the participant / model</td></tr><tr><td>Participant</td><td>Name of the participant / model</td></tr><tr><td>Participation</td><td>Distinguish if a participant is ready for the private leaderboard<br>(only if there is no public leaderboard)</td></tr><tr><td>Team</td><td>The participant's team name<br>(only if the competition allows teams)</td></tr><tr><td>Best <code>metric</code> (<code>unit</code>)</td><td>The best submission score for the metric <code>metric</code><br>(reset between each rounds)</td></tr><tr><td>Last <code>metric</code> (<code>unit</code>)</td><td>The last submission score for the metric <code>metric</code></td></tr><tr><td>Mean</td><td>The weighted average of the metrics<br>(only if there are multiple)</td></tr><tr><td><code>metric</code> (<code>unit</code>)</td><td>The score value of the metric <code>metric</code></td></tr><tr><td>Run Success</td><td>The number of successful runs / the number of failed runs</td></tr><tr><td>Previous LB Chn</td><td>Rank change from the previous round</td></tr><tr><td>Public LB Chn</td><td>Rank change from the public leaderboard</td></tr><tr><td>Weekly Change</td><td>Rank change from the previous week</td></tr><tr><td>User Hist. Rewards</td><td>The amount of rewards ever received by the participant</td></tr><tr><td>Proj. Rewards</td><td>The amount of rewards the participant is expected to receive at the end of the month</td></tr></tbody></table>

<figure><img src="../../.gitbook/assets/image (144).png" alt=""><figcaption><p>A small view of the leaderboard, including rank changes and a metric.</p></figcaption></figure>

### Badges

<table><thead><tr><th width="214">Icon</th><th>Description</th></tr></thead><tbody><tr><td>Crown <img src="../../.gitbook/assets/image (133).png" alt="" data-size="original"></td><td>The participant has won the crunch</td></tr><tr><td>Certificate <img src="../../.gitbook/assets/image (134).png" alt=""></td><td>The participant has earned a certificate</td></tr><tr><td>Reward <img src="../../.gitbook/assets/image (137).png" alt=""></td><td>The participant has received a prize</td></tr><tr><td><img src="../../.gitbook/assets/image (138).png" alt="" data-size="original"></td><td>The participant only submitted a Quickstarter and is disqualified from receiving any rewards</td></tr><tr><td><img src="../../.gitbook/assets/image (139).png" alt="" data-size="original"></td><td>The <a href="../../other/glossary.md#prediction">prediction</a> is too close to another<br>(<a href="duplicate-predictions.md">grouped by same user or same team, if correlation > 95%</a>)</td></tr><tr><td><img src="../../.gitbook/assets/image (140).png" alt="" data-size="original"></td><td>The code is not deterministic<br>(running the infer function twice give a different result)</td></tr><tr><td><img src="../../.gitbook/assets/image (142).png" alt="" data-size="original"></td><td>The score is not within the allowed range</td></tr></tbody></table>



## Participant

If the crunch allows more than one model, the model name is displayed right after the participant name. Otherwise, only the participant name is displayed.

### Only one Model on the Leaderboard

Some competitions (like Mid+One) allow more than one model, but only ONE can be displayed on the leaderboard at any given time.

The selection can be made during the [Submission Phase](../../other/glossary.md#submission-phase). After that, the <img src="../../.gitbook/assets/image (132).png" alt="" data-size="line"> button will no longer be available.

<figure><img src="../../.gitbook/assets/image (130).png" alt=""><figcaption><p>The model will be displayed on the leaderboard and will be eligible for rewards.</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (131).png" alt=""><figcaption><p>The model will NOT be displayed on the leaderboard and will NOT be eligible for rewards.</p></figcaption></figure>

## Ties

Some competitions (such as Broad #2) allow ties when models achieve the same scores, meaning they receive the same rank.

The rank assigned to all tied models is the first rank of the tie.

The reward is distributed equally among the tied models, calculated as the sum of the rewards for the tied models divided by the number of models in the tie.

#### Example

For a prize pool that rewards the top 10 models:

* If the <mark style="color:blue;">1st</mark>, <mark style="color:purple;">2nd</mark>, and <mark style="color:orange;">3rd</mark> place models are tied, they will all be ranked <mark style="color:blue;">1st</mark>.\
  The next model will be ranked <mark style="color:green;">4th</mark>.\
  Their total reward will be: (<mark style="color:blue;">1st place prize</mark> + <mark style="color:purple;">2nd place prize</mark> + <mark style="color:orange;">3rd place prize</mark>) / 3.
* If the <mark style="color:blue;">8th</mark>, <mark style="color:purple;">9th</mark>, <mark style="color:orange;">10th</mark>, <mark style="color:red;">11th</mark> and <mark style="color:yellow;">12th</mark> place models are tied, they will all be ranked <mark style="color:blue;">8th</mark>.\
  The next model will be ranked <mark style="color:green;">13th</mark>.\
  Their total reward will be: (<mark style="color:blue;">8th place prize</mark> + <mark style="color:purple;">9th place prize</mark> + <mark style="color:orange;">10th place prize</mark>) / 5.
