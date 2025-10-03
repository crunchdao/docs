# Competitions

## What are Competitions?

The competitions are the real fuel of the Crunch Foundation.&#x20;

The data quality is top tier and clients offer big cash prize in order to crowdsource predictions.

There are for now different format of collaboration:

* Limited in time competitions: The client buy the models to the rewarded winners.
* Continuous competitions: last in time. The client pays to query the model through our API to receive the community's prediction each hour/day/week/months.
* Rallies: Limited in time. It allows clients to test a dataset and the performance of the community models before launching a Continuous Competition.
* Code Competitions: Participant must submit the code of their model in order for the customer to query inferences from the submission.
* Predictions Competitions: Participant submit a simple prediction not the model or the code of the model.

## Competition Stages

A competition is structured in multiple stages:

### 1. The [Submission Phase](../../other/glossary.md#submission-phase)

Participants will have access to the training data and can submit their solutions to the challenge.

A public leaderboard is usually available for you to use to compare yourself with other participants.

### 2. The Selection Phase

The Selection Phase comes after the Submission Phase, which gives you extra time to make your selection for the Out-of-Sample phase.

If you start a run at the very end of the Submission Phase, it gives you time to wait for it to finish before making your final selection. However, the submission period has now closed and it is no longer possible to create new runs.

If you don't select anything, **the last run will be selected automatically**.

### 3. The [Out-of-Sample Phase](../../other/glossary.md#out-of-sample-phase)

The private leaderboard will be computed by running your model(s) on unseen data.

Thanks to the Submission Phase, your submission is considered valid, and you should not encounter any issues when running it on the new dataset.

However, the same Submission Phase rules apply:

* If your code exceed the quota, your run will end.
* If your code crashes, your run will end.

A run that isn't successful means the end of the competition for you.

#### Data update

In rare cases where we decide to update the training data, we will ensure that it is sufficient to cover the new dataset size.

This change may be unannounced, so your code must be able to handle it. This is why we ask for the training code in advance.\
\
Failure to provide this will result in you losing access to the new training data.

However, most of the time, only the new data will be used for inference.\
This means that training will be skipped (as the training data will be the same), and you can use your entire quota for inference.

#### Early contact

If you are among the [top N](#user-content-fn-1)[^1] on the private leaderboard, we will contact you to complete the KYC process. This is necessary in order to be eligible to receive the prize.

This ensures that we have everything ready to distribute the prize on time.

Of course, you must not tell anyone that you were contacted.\
This is also why we are contacting N participants:

* To not reveal the leaderboard too early.
* To make sure that we have enough winners in case someone refuses their prize.

### 4. The Reveal

The reveal will happen at the very end, after the private leaderboard is published.

The rankings will then be final.

The prizes will be distributed in the coming days. If you are one of winner, we suggest reading the [Prizes Winners documentation](../faqs/prize-winners.md) to know what to do next.

[^1]: e.g. top 30 if the prize reward the top 10
