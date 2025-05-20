---
description: >-
  To get started and submit your first model, you will need to pass through the
  following steps.
---

# Participate

## Register

Creating an account on the CrunchDAO platform will allow you to be identified and get access to the competition dataset. Follow the link below to join the competition.

{% embed url="https://account.crunchdao.com/auth/register?ref=doc" %}

## Submit

Two distinct formats of submission are accepted for the competitions:

* **Jupyter Notebook** (`.ipynb`), which is a self-contained version of the code
* **Python Script** (`.py`), which allows more flexibility and to split the code into multiple files

{% hint style="info" %}
All the work you submit remains your exclusive property. The Crunch Foundation guarantees the privacy of both client data and competitors' code.
{% endhint %}

### Jupyter Notebook

Notebook users can use the Quickstarters provided by CrunchDAO to quickly experiment with a working solution that users can tinker with.

#### Setting the Environment

Before trying to execute any cell, users must set up their environment by copying the command available on the competition page:

<figure><img src="../.gitbook/assets/Jupyter Notebook - 1.png" alt=""><figcaption><p>The "Submit a Notebook" tab from the "Submit" page of a competition</p></figcaption></figure>

Run the commands to set up your environment and download the data to be ready to go:

{% code title="Python Notebook Cell" %}
```python
# Upgrade the Crunch-CLI to the latest version
%pip install crunch-cli --upgrade

# Authenticates yourself, it will downloads your last submission and the data
!crunch setup <competition name> <model name> --notebook --token <token>
```
{% endcode %}

{% hint style="info" %}
[Learn more about how setup tokens work and if it is safe to leak them.](participate.md#setup-tokens)
{% endhint %}

Users can now load the data locally:

{% code title="Python Notebook Cell" %}
```python
# Load the notebook, run me once
import crunch
crunch = crunch.load_notebook()

# Load the data, re-run me if you corrupt the dataframes
X_train, y_train, X_test = crunch.load_data()
```
{% endcode %}

#### Local Testing

When users are satisfied with their work, they can easily test their implementation:

{% code title="Python Notebook Cell" %}
```python
# Run a local test
crunch.test()
```
{% endcode %}

#### Submitting your Notebook

After testing the code, users need to have access to the `.ipynb` file.

* If you are on Google Colab: `File` > `Download` > `Download .ipynb`
* If you are on Kaggle: `File` > `Download Notebook`
* If you are on Jupyter Lab: `File` > `Download`

Then submit on the **Submit a Notebook** page:

<figure><img src="../.gitbook/assets/Jupyter Notebook - 2.png" alt=""><figcaption></figcaption></figure>

Some model files can also be uploaded along with the notebook, which will be stored in the `resources/` directory. [Read more about the file selection dialog.](participate.md#file-selection-dialog)

#### Global variables

If you want to use global variables in your notebook, put them in a class, this will improves the readability of your code:

{% code title="Python Notebook Cell" %}
```python
class Constants:

    TRAIN_DEPTH = 42
    IMPORTANT_FEATURES = [ "a", "b", "c" ]

def infer():
    print(Constants.TRAIN_DEPTH)
    # 42
```
{% endcode %}

#### Automatic line commenting

The notebook is automatically converted into a Python script that only includes the functions, imports, and classes.

Everything else is commented out to prevent side effects when your code is loaded into the cloud environment. (e.g. when you're exploring the data, debugging your algorithm, or doing visualizating using Matplotlib, etc.)

You can prevent this behavior by using special comments to tell the system to keep part of your code:

* To start a section that you want to keep, write: `@crunch/keep:on`
* To end the section, write: `@crunch/keep:off`

{% code title="Python Notebook Cell (before)" %}
```python
# @crunch/keep:on

# keep global initialization
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# keep constants
TRAIN_DEPTH = 42
IMPORTANT_FEATURES = [ "a", "b", "c" ]

# @crunch/keep:off

# this will be ignored
x, y = crunch.load_data()

def train(...):
    ...
```
{% endcode %}

The result will be:

{% code title="Python Notebook Cell (after)" %}
```python
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

TRAIN_DEPTH = 42
IMPORTANT_FEATURES = [ "a", "b", "c" ]

#x, y = crunch.load_data()

def train(...):
    ...
```
{% endcode %}

The command does not affect comments, functions, classes, or imports.

{% hint style="info" %}
You can put a `@crunch/keep:on` at the top of the cell and never close it to keep everything.
{% endhint %}

#### Specifying package versions

Since submitting a notebook does not include a `requirements.txt`, users can instead specify the version of a package using import-level [requirement specifiers](https://pip.pypa.io/en/stable/reference/requirement-specifiers/#examples) in a comment on the same line.

{% code title="Python Notebook Cell" %}
```python
# Valid statements
import pandas # == 1.3
import sklearn # >= 1.2, < 2.0
import tqdm # [foo, bar]
import scikit # ~= 1.4.2
from requests import Session # == 1.5
```
{% endcode %}

Specifying multiple times will cause the submission to be rejected if they are different.

{% code title="Python Notebook Cell" %}
```python
# Inconsistant versions will be rejected
import pandas # == 1.3
import pandas # == 1.5
```
{% endcode %}

Specifying versions on standard libraries does nothing (but they will still be rejected if there is an inconsistent version).

{% code title="Python Notebook Cell" %}
```python
# Will be ignored
import os # == 1.3
import sys # == 1.5
```
{% endcode %}

If an optional dependency is required for the code to work properly, an import statement must be added, even if the code does not use it directly.

{% code title="Python Notebook Cell" %}
```python
import castle.algorithms

# Keep me, I am needed by castle
import torch
```
{% endcode %}

#### Embed Files

Additional files can be embedded in cells to be submitted with the Notebook. In order for the system to recognize a cell as an Embed File, the following syntax must be followed:

{% code title="Markdown Notebook Cell" %}
```markdown
---
file: <file_name>.md
---

<!-- File content goes here -->
Lorem ipsum dolor sit amet, consectetur adipiscing elit.
Aenean rutrum condimentum ornare.
```
{% endcode %}

Submitting multiple cells with the same file name will be rejected.

While the focus is on Markdown files, any text file will be accepted. Including but not limited to: `.txt`, `.yaml`, `.json`, ...

### Python Script

Script users can use the Quickstarters provided by CrunchDAO to know what the structure should be.

A mandatory main.py is required to have both functions (`train` and `infer`) in order for your code to run properly.

#### Setting the Environment

Before starting to work, users must setup their environment which will be similar to a git repository.

<figure><img src="../.gitbook/assets/Python Script.png" alt=""><figcaption><p>The "Submit via CLI" tab from the "Submit" page of a competition</p></figcaption></figure>

Run the commands to set up your environment and download the data to be ready to go:

{% code title="Terminal" %}
```bash
# Upgrade the Crunch-CLI to the latest version
$ pip install crunch-cli --upgrade

# Authenticates yourself, it will downloads your last submission and the data
$ crunch setup <competition name> <model name> --token <token> [directory]

# Change the directory to the configured environment
$ cd <directory>
```
{% endcode %}

[Read more about how setup tokens work and why it is safe to (accidentally) "leak" them.](participate.md#setup-tokens)

#### Directory Layouts

{% code title="File Explorer" %}
```bash
# Example of a folder structure.
# The data files may change depending on the competition.
.
├── data/
│   ├── X_test.parquet
│   ├── X_train.parquet
│   └── y_train.parquet
├── main.py
├── requirements.txt
└── resources/
    └── model.joblib
```
{% endcode %}

<table><thead><tr><th width="215">File / Directory</th><th>Reason</th></tr></thead><tbody><tr><td><code>data/</code></td><td>Directory containing the data of the competition, should never be modified by the user. Always kept up to date by the CLI.</td></tr><tr><td><code>main.py</code></td><td>Code entry point. Must contain the <code>train()</code> and <code>infer()</code> function. Can import other files if necessary. <a href="code-interface.md">Learn more...</a></td></tr><tr><td><code>requirements.txt</code></td><td>List of packages used by your code. They are installed before your code is invoked.</td></tr><tr><td><code>resources/</code></td><td>Directory where your model should be stored. The content is persisted across runs during the transition between the <a href="../other/glossary.md#submission-phase">Submission</a> and <a href="../other/glossary.md#out-of-sample-phase">Out-of-Sample</a> phases.</td></tr></tbody></table>

#### Local Testing

When users are satisfied with their work, they can easily test their implementation:

{% code title="Terminal" %}
```bash
# Run a local test using a shell command
$ crunch test
```
{% endcode %}

#### Pushing your Code

After the code has been tested, the submission needs to be uploaded to the server.

The message is optional and is just a label for users to know what they did.

{% code title="Terminal" %}
```bash
$ crunch push --message "hello world"
```
{% endcode %}

{% hint style="info" %}
Remember to include all your dependencies in a `requirements.txt` file.
{% endhint %}

#### Package version freezes

Before submitting, the CLI does a `pip freeze` in the background to find out what version you are using locally. These versions are then used to freeze the `requirements.txt` on the server.

This is to ensure that if your code works locally with the exact same versions, it should _theoretically_ work the same on the server.

However, this behavior may result in your packages not being installed because:

* you are using custom/externally installed package versions,
* you have installed some packages by force, but PyPI considers them incompatible,
* the architecture is different and the package is platform-specific.

If you want to disable this behavior, use the `--no-pip-freeze` flag.

{% code title="Terminal" %}
```bash
$ crunch push --no-pip-freeze --message "hello world"
```
{% endcode %}

{% hint style="info" %}
Your original requirements will always be preserved.\
Don't hesitate to reach out to us on [Discord](https://discord.gg/veAtzsYn3M) or the [Forum](https://forum.crunchdao.com/) for help.
{% endhint %}

### Hybrid

For some complex setups, users may need to use the CLI to submit a Jupyter Notebook. This can happen if they want to submit with a large pre-trained model, or they want to include non-PyPI packages.

It will be very similar to the Python Script setup:

* [#setting-the-environment-1](participate.md#setting-the-environment-1 "mention"), like for a Python Script.
* Remove the `main.py`.
* Move your notebook to the project directory and name it `main.ipynb`.

{% hint style="info" %}
The `main` name can be changed by using the `--main-file <new_file_name>.py` option. (keep the `.py` at the end)
{% endhint %}

If done correctly, before each crunch push, the CLI will first convert the notebook to a script file before sending it.

{% hint style="warning" %}
[Package version specifiers](participate.md#specifying-package-versions) will not work.\
The `requirements.txt` file must be updated manually.
{% endhint %}

{% hint style="info" %}
[Package version freezes](participate.md#package-version-freezing) are still being done in the background.
{% endhint %}

### Files

If you do not want to use the CLI and did not use a notebook to write your code, you can submit files directly. This is an advanced feature that requires preparation.

The directory layout must be the same as [the CLI directory layout](participate.md#directory-layouts).

#### File Selection Dialog

To add files you can:

* select multiple files by clicking on the "Add file(s)" button
* or select the contents of an entire directory by clicking on the "Add directory" button

<figure><img src="../.gitbook/assets/Files - 1.png" alt=""><figcaption><p>Files selection dialog</p></figcaption></figure>

{% hint style="info" %}
If no files have been selected yet and you add a directory, that directory will be used as the root.
{% endhint %}

{% hint style="info" %}
Due to a limitation of web browsers, it is not possible to select a directory via the "Add file(s)" button or add multiple directories via the "Add directory" button.

To achieve this, either:

* add multiple directories
* or place your submission in a directory and add the directory once.
{% endhint %}

Once added, files can be disabled if you add too many, or renamed if the name is incorrect.

<figure><img src="../.gitbook/assets/Files - 2.png" alt=""><figcaption><p>A model is selected</p></figcaption></figure>

{% hint style="warning" %}
You need to submit the `requirements.txt` yourself.
{% endhint %}

### Setup Tokens

The site generates new tokens every minute, and each token can only be used once within a 3-minute timeframe.

This prevents any problems if your token is accidentally shared, as it will likely have already been used or expired. Even the team shares their expired tokens in Quickstarters.

This token allows the CLI to download the data and submit your submission on your behalf.

## Run

#### Checking your submission

<figure><img src="../.gitbook/assets/Checking your Submission - 1.png" alt=""><figcaption><p>A successful submission.</p></figcaption></figure>

The system parses your work to retrieve the code of the interface functions (`train()` and `infer()`) and their dependencies. By clicking on the right arrow, you can access the contents of your submission.

<figure><img src="../.gitbook/assets/Checking your Submission - 2.png" alt=""><figcaption><p>The view of a submission once properly uploaded</p></figcaption></figure>

#### Running in the Cloud

Once you've submitted, it's time to make sure your model can run in the cloud environment. Click on a submission and then click the **Run in the Cloud** button.

<figure><img src="../.gitbook/assets/Running in the Cloud - 1.png" alt=""><figcaption><p>Click Run in the Cloud to start your run</p></figcaption></figure>

Your code is fed a standard epoch of data and the system simulates an inference.

<figure><img src="../.gitbook/assets/Running in the Cloud - 2.png" alt=""><figcaption><p>If your submission ran properly, you'll see the status as successful.</p></figcaption></figure>

{% hint style="info" %}
A successful run means that the system will be able to call your code on new data to produce the inferences for that customer.
{% endhint %}

#### Debugging with the logs

If your run crashes or you want to better understand how your code behaved, you can review the logs.

<figure><img src="../.gitbook/assets/Debugging With the Logs - 1.png" alt=""><figcaption><p>How to check your execution logs</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Debugging With the Logs - 2.png" alt=""><figcaption><p>Logs of a run</p></figcaption></figure>

{% hint style="warning" %}
Due to abuse, only the first 1,500 lines of a user's code logs will be displayed.
{% endhint %}
