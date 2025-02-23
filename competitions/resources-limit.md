# Resources Limit

## Submissions

You can only submit during the [Submission Phase](../other/glossary.md#submission-phase).

You can only submit up to 5 times per day. (some competition may allow more)

A submission cannot exceed 5GB and his `resources/` directory cannot exceed 10GB.

{% hint style="info" %}
If you are debugging, we can **exceptionally** allow you more. [Please contact @cruncher\_enzo on Discord.](https://discord.com/invite/veAtzsYn3M)
{% endhint %}

## Runs

In order for a run to be considered valid, a prediction must be made within the following:

### **Resources**

<table><thead><tr><th width="219">Runner Specs</th><th>CPU</th><th>GPU (g4dn.8xlarge)</th></tr></thead><tbody><tr><td>CPU Core</td><td>16 vCPU</td><td>32 vCPU</td></tr><tr><td>RAM</td><td>64 Gb</td><td>128 Gb</td></tr><tr><td>Disk</td><td>40 Gb</td><td>300 Gb</td></tr><tr><td>Network</td><td>Any <a href="https://en.wikipedia.org/wiki/Network_socket">socket</a> related <a href="https://man7.org/linux/man-pages/man2/socketcall.2.html">operations</a> are not permitted</td><td>No internet</td></tr><tr><td>Shared Memory (SHM)</td><td>10 Gb</td><td>10 Gb</td></tr><tr><td>Graphics Card</td><td></td><td>NVIDIA T4</td></tr></tbody></table>

{% hint style="warning" %}
If you are using a multiprocessing library that relies on socket for local communication, they will not work on CPU runs.

Please use a GPU run instead, even if you do not need a GPU.
{% endhint %}

### Time constraints

* During the [Submission Phase](../other/glossary.md#submission-phase): **the runtime quota reset every week**
* During the [Out-of-Sample Phase](../other/glossary.md#out-of-sample-phase): **the runtime quota does not reset every week**

The time limits are different per competition, you must read the competition page to know the value.

{% hint style="info" %}
The quota are different per competition. Read the overview to know the value.
{% endhint %}

{% hint style="info" %}
You can control your quota under the **my submission** tab.
{% endhint %}
