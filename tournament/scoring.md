# Scoring

### Spearman correlation score

Once you have submitted, your prediction will be confronted to what is happening on the market. It will be scored weekly using the spearman rank metric.&#x20;



### Post processing ranking (scaled leaderboard)

* The last two rounds on each dataset are taken into account
* Non-submissions in any round get a score of -5. (Incentivises long-term participation.)
* Scores are normalized (between range \[-1,1]) per round. Then both rounds are averaged.
* Once averaged, users scoring above the 90th percentile get the same score of +1. (This is to disincentivize overfit models as anyone above a threshold gets the same score).
* Finally, the scores for all rounds are averaged.



This post processing ranking has been voted :

First proposal :&#x20;

{% embed url="https://snapshot.org/#/datacrunch.eth/proposal/0xdd240592ae82a405b975e7a9d5fa4701b1cc3ccf660eb7b9c69deec8b78bbd75" %}
\[Ranking] Rounds Equal Weightage proposal
{% endembed %}

* Second proposal :&#x20;

{% embed url="https://snapshot.org/#/datacrunch.eth/proposal/0x96719d7b67f0000a2b50c50d6b6797c9c774e10e98c0da440465812151cd73d3" %}
\[Ranking] Normalization, Top percentile, Non-submitters penalty
{% endembed %}
