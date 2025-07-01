# Notebook Processor

When a notebook is submitted, it is first processed to extract the requirements and concatenate all the cells into a single Python file.

This processing involves a lot and sometimes comments too much code.

You can provide hints to help the processor better understand your intentions.

## Automatic line commenting

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

### Global variables

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

{% hint style="info" %}
This method was introduced before the recommended `@crunch/keep:on` command.
{% endhint %}

## Specifying package versions

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

It is possible for multiple import names to resolve to different libraries on PyPI. If this happens, you must specify which one you want. If you do not want a specific version, you can use `@latest`, as without this, we cannot distinguish between commented code and version specifiers.

{% code title="Python Notebook Cell" %}
```python
# Prefer https://pypi.org/project/EMD-signal/
import pyemd # EMD-signal @latest

# Prefer https://pypi.org/project/pyemd/
import pyemd # pyemd @latest
```
{% endcode %}

## Embed Files

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
