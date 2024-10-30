# Whitelisted Libraries

To ensure that the users are not using malicious packages on the competition's infrastructure, a series of Python packages have been whitelisted.

{% hint style="info" %}
You can search the whitelisted library in the Submit section of each competition.
{% endhint %}

## Requesting a package

Packages can be requested to be whitelisted by sending a message to [**@cruncher\_enzo** on Discord](https://discord.com/invite/veAtzsYn3M). To make it easier, please include some of the following in your message:

* The link to the PyPI page.
* Whether it requires a GPU or not.
*   If the package is imported under a name other than the package name

    (e.g. `scikit-learn` is `sklearn`).

However, there are a number of requirements (pun intended) that must be met:

* The package must be available on [PyPI](https://pypi.org/).
* The package must have a good readme/documentation.
* The package must not be new (more than 6 months old).
* The package must have many downloads (e.g. [pandas](https://pypistats.org/packages/pandas)).
* The package must have [verified details](https://docs.pypi.org/project\_metadata/) (by PyPI).

{% hint style="info" %}
We reserve the right to refuse a package if it looks suspicious.
{% endhint %}

### Package not on PyPI

If the package is only available on GitHub, you will need to download it and include it in your submission. Only PyPI packages are allowed.
