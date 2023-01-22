# Live Score Computation Process

Computing the live score of each day is a very computationally intensive process.

### Active datasets lookup

![](<../../.gitbook/assets/image (23).png>)

This board allows you to visualize which dataset is currently active and scored against live data.

### Live Score computation

For each primary submission, the scorer will compute the spearman correlation score between your prediction for that target (30, 60, 90 days) and what happened live.

### Mean by dataset

For each dataset, your live score for this dataset is the mean of your last two submissions for this dataset.

### Mean of mean

The global mean score is the mean of all your live scores over the universe of strategy. If you miss a round you get a score of -5%.

### Normalization

Since December '21, all round's scores are normalized to avoid some rounds having to much impact on the overall score: [https://discord.com/channels/770586873260933120/770586873260933124/905097810502910033](https://discord.com/channels/770586873260933120/770586873260933124/905097810502910033)

### Computing global leaderboard

Crunchers are ranked by their mean live spearman correlation score normalized overall strategies.

### Internal Schema

Here is a schema on how the process is working internally:

![internal schema](../../.gitbook/assets/live-computer.drawio.svg)
