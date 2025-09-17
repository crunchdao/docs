---
description: Experimental polyglot support.
---

# R Usage

{% hint style="warning" %}
This feature is still experimental. If you encounter any issues, please contact us on [Discord](https://discord.com/invite/veAtzsYn3M) or in the [Forum](https://forum.crunchdao.com/).
{% endhint %}

R is now partially supported in the runner.

We recommend using [rpy2](https://rpy2.github.io/) ([link to documentation](https://rpy2.github.io/doc/latest/html/introduction.html)) which embed a R runtime inside Python so it can run in the same process. Spawning a new process is still possible, but not recommended, as it will **considerably slow down your code** for big loops.

## Submit

The R runtime will only be installed if a `requirements.r.txt` file is present in the root directory of your submitted files.

As with Python packages, R packages are subject to a whitelist and can be requested using the same button. In the dialog, chose "R" in the "Programming Language" select.

### Notebooks

#### Importing a package

For notebook users, the packages are automatically extracted from the `importr("<name>")` calls, which is provided by [rpy2](https://rpy2.github.io/).

{% code title="Python Notebook Cell" %}
```python
# Import the `importr` function
from rpy2.robjects.packages import importr

# Import the "base" R package
base = importr("base")
```
{% endcode %}

The following format must be followed:

* The import must be declared at the root level.
* The result must be assigned to a variable; the variable's name will not matter.
* The function name must be `importr`, and it must be imported as shown in the example above.
* The first argument must be a string constant, variables or other will be ignored.
* The other arguments are ignored; this allows for [custom import mapping](https://rpy2.github.io/doc/latest/html/robjects_rpackages.html#importing-r-packages) if necessary.

The line will not be commented, [read more about line commenting here](notebook-processor.md#automatic-line-commenting).

#### Installing a package

If the library is not available in your environment, you can install it using either `apt-get` (the recommended method) or the `install.packages()` from the `utils` package.

{% code title="Python Notebook Cell" %}
```python
# Use `apt-get` as a regular command
!apt-get install r-cran-lme4
```
{% endcode %}

{% code title="Python Notebook Cell" %}
```python
# Use the `utils` package, but it must be imported first
from rpy2.robjects.packages import importr
utils = importr("utils")

# Call `install.packages(...)`, this will be commented on submit
utils.install_packages("lme4")
```
{% endcode %}

#### Declaring a R function

[Any R code can be run using the `robjects.r(code)` function.](https://rpy2.github.io/doc/v2.9.x/html/robjects_rinstance.html)

The following code can be used to [define a function written entirely in R](https://rpy2.github.io/doc/v2.9.x/html/robjects_rinstance.html#evaluating-a-string-as-r-code):

{% code title="Python Notebook Cell" %}
```python
from rpy2.robjects import r

# Declare the function in R and have it return
f = r("""
	f <- function(r, verbose=FALSE) {
		if (verbose) {
			cat("I am calling f().\n")
		}

		2 * pi * r
	}

	# Don't forget to return it
	f
""")

# Call the function
circumference = f(3)
```
{% endcode %}

{% hint style="warning" %}
Even if the import must be at the root level, the call to `robjects.r(code)` must be made within a Python function so that it is not commented out.

We recommend declaring them in a dedicated cell and using the `# @crunch/keep:on` command to prevent them from being commented out. [Read more about line commenting here](notebook-processor.md#automatic-line-commenting).
{% endhint %}

### Files

#### Manage requirements

For CLI or files users, the packages must be provided in a `requirements.r.txt`  file. The format is the same as a regular `requirements.txt` file, except all specs, extras, and comments will be ignored (for now).

Technically, notebook users can also do it [by using embedded files](notebook-processor.md#embed-files).

#### Executing a file

A R script can be run using `robject.r.source(path)`, and the variable will be accessible via the `robject.r[key]`  accessor.

{% code title="Local File: `hello.R`" %}
```r
cat("Hello world from a R file")

x <- 42
```
{% endcode %}

{% code title="Local File: `main.py`" %}
```python
# Import the R instance
from rpy2.robjects import r

# Load and execute the R file
r.source('hello.R')
#> "Hello world from a R file"

# Get a value from the scope
print(r["x"])
#> "42"
```
{% endcode %}

## Running

When a [Run](../../other/glossary.md#run) is started, before using `pip` to install your requirements, R and your requested packages are first installed using `apt-get install r-cran-<name>`. This has the benefit of installing them faster, but it greatly limits the number of choices to only the most downloaded ones (for now).

## Recommendations

### Redirect outputs

R's output going through [`rpy2` callback](https://rpy2.github.io/doc/latest/html/callbacks.html#console-i-o) and can be a bit hard to read, a solution is to redirect the outputs:

{% code title="Notebook Cell" %}
```python
# @crunch/keep:on

from rpy2 import rinterface

rpy2.rinterface_lib.callbacks.consolewrite_print = lambda x: sys.stdout.write(x)
rpy2.rinterface_lib.callbacks.consolewrite_warnerror = lambda x: sys.stderr.write(x)
rpy2.rinterface_lib.callbacks.consoleflush = lambda: sys.stdout.flush(); sys.stderr.flush()
```
{% endcode %}

[The comment at the very top is important because it ensures that your code is not commented out.](notebook-processor.md#automatic-line-commenting)

### Pandas Conversion

[rpy2 supports DataFrame conversion](https://rpy2.github.io/doc/latest/html/pandas.html), but you must use the `with` syntax to enable/disable conversion at the desired location.

An easier way is to use the following helpers:

{% code title="Notebook Cell" %}
```python
import pandas as pd
import rpy2.robjects as ro
from rpy2.robjects import pandas2ri

def convert_df_pandas_to_r(pandas_df: pd.DataFrame) -> "FloatMatrix":
    """
    Convert a pandas's DataFrame to an equivalent R's FloatMatrix.
    """

    with (ro.default_converter + pandas2ri.converter).context():
        return ro.conversion.get_conversion().py2rpy(pandas_df)

def convert_df_r_to_pandas(r_df: "FloatMatrix") -> pd.DataFrame:
    """
    Convert a R's FloatMatrix to an equivalent pandas's DataFrame.
    """

    with (ro.default_converter + pandas2ri.converter).context():
        return ro.conversion.get_conversion().rpy2py(r_df)
```
{% endcode %}
