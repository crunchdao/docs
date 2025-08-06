---
description: Frequently Asked Questions
---

# FAQs

## Can I train a model locally?

Yes, but you should still include the code in case we need to rerun your work.

We understand that training some models requires significant resources and computing time, which cannot easily fit within the weekly quota. Some models even require a network connection that is not available in the cloud environment.

For these models, we recommend training locally but including the training code in a separate file or commenting it out.

If you do not submit your training code, you [might miss the opportunity](#user-content-fn-1)[^1] to retrain with more data during the [Out-of-Sample Phase](../other/glossary.md#out-of-sample-phase), as we have done in past competitions.

## How can I get the output of a Run?

Files generated in the cloud environment (e.g., trained models) cannot be extracted or downloaded locally.

This ensures that data accessible only there cannot be exfiltrated.



[^1]: They can be decided late in the competition.
