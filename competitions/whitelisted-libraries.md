# Whitelisted Libraries

To prevent users from using malicious packages on the competition's infrastructure, a series of Python packages has been whitelisted.

The whitelist is also used when converting notebooks because some libraries have different names when imported than on PyPI. For example, `scikit-learn` is `sklearn`.

You can search the whitelisted library in the **Resources > Whitelisted Libraries** section of each competition.

## Requesting a package

You can request to whitelist packages via the "**Request whitelisting**" button in the "**Resources > Whitelisted Libraries**" section of each competition.

<figure><img src="../.gitbook/assets/image.png" alt=""><figcaption><p>Request to whitelist a library form</p></figcaption></figure>

Administrators need some information to find the package on PyPI and approve it.

* The **Name** is the package name on PyPI.\
  e.g.: `scikit-learn`
* The **Alias** is the name used in the Python code to access the package.\
  e.g.: `sklearn`
* The **GPU Requirement** is the hardware requirement to use the package:
  * If you are unsure, select **I don't know**.
  * If the package does not require a GPU at all, select **Useless for this library**.
  * If a GPU can be used but is not required, select **Can be useful for this library**.
  * If the package requires a GPU, select **Required for this library**.
* You can leave **Additional Details** if the package requires special attention, so the administrators can better understand why.

### Requirements

However, there are a number of requirements (pun intended) that must be met:

* The package must be available on [PyPI](https://pypi.org/).
* The package must have a good readme/documentation.
* The package must not be new (more than 6 months old).
* The package must have many downloads (e.g. [pandas](https://pypistats.org/packages/pandas)).
* The package must have [verified details](https://docs.pypi.org/project_metadata/) (by PyPI).

{% hint style="info" %}
We reserve the right to refuse a package if it looks suspicious.
{% endhint %}

### Package not on PyPI

If the package is only available on GitHub, you will need to download it and include it in your submission. Only PyPI packages are allowed.
