# Python

[TOC]

# TODO(dcv): Add pep8 documentation.

## Basics

### Virtual Environment

Environments are no fun. Dependencies are no fun. I am not smart. So mitigate
these problems by using a
[virtual environment](https://docs.python.org/3/library/venv.html). These allow
creation of a “virtual” isolated Python installation and install packages into
that virtual installation. Starting from scratch everytime will allow an easier
googling experience when issues arise because of a reproducible tabula rasa
state.

#### Pre-checks

First, check python and pip versions before diving in, ensure they are
appropriate. Also get `virtualenv`:

```bash
python3 --version  # Python 3.8.2
python3 -m pip install --user --upgrade pip  # pip in /Users/davisv/Library/Python/3.8/lib/python/site-packages (21.1.3)
python3 -m pip install --user virtualenv  # virtualenv in /Users/davisv/Library/Python/3.8/lib/python/site-packages (20.4.7)
```

NOTE: For Python >3.3, `venv` is preferred module for setting up virtual
environments.

#### Creating venv

Start with a new environment:

```bash
python3 -m venv named_virtual_env
```

Running this command creates:

-   The target directory under your current working directory.
-   A `pyvenv.cfg` file.
-   A `bin` subdirectory.
-   A `lib/pythonX.Y/site-packages` subdirectory with `pip`, `setuptools`, and
    an `easy_install.py` python script.

NOTE: exclude the virtual environment directory in `.gitignore`.

NOTE: If an existing directory is specified, it will be re-used. The `cnfg` file
has a home key pointing to the Python installation from which the command was
run. The `bin` folder contains a copy of the Python binary/binaries.

Then activate with the binary script (bash/zsh specific). When a virtual
environment is active, the `VIRTUAL_ENV` environment variable is set to the path
of the virtual environment; `echo` to check, also check python.

```bash
source named_virtual_env/bin/activate
echo $VIRTUAL_ENV  # $HOME_PATH/$PROJECT/named_virtual_env
which python  # $HOME_PATH/$PROJECT/named_virtual_env/bin/python
```

TIP: When returning, `source` in, then `echo` to confirm. when going into
jupyter, `screen` in. Then can detach with `ctrl-a + ctrl-d` and see it in
`screen -ls`. That PID can be killed with `screen -XS ## quit` where ## is the
PID. Finally `deactivate` the virtual env when finished (always).

#### Install packages @version; requirements file

Strangely, the pip version might not be the same as in precheck. Just do a quick
upgrade.

```bash
pip install --upgrade pip
```

Install a package at a certain version (optional after package name).

```bash
python3 -m pip install pandas==1.3.0
```

Instead of installing packages individually, pip allows you to declare all
dependencies in a `requirements.txt` file and then can pass them to pip.
`freeze` returns a list of current packages at certain versions.

```bash
python3 -m pip freeze > requirements.txt
python3 -m pip install -r requirements.txt
```

`requirements.txt` can specify dependencies as well to make sure other packages
cooperate effectively.

```
pkg1
pandas==1.3.0
pkg3>=1.0,<2.0
```

TIP: Run `python -m pip completion --zsh >> ~/.zprofile` for extra CLI support.

### Functions

A good function has variables with typesetting and a docstring defining the
function purpose and variable usage.

```python
def CombineData(
    independent_column: pd.Series,
    other_column: pd.Series,
    some_threshold: float = 0.01) -> pd.DataFrame:
  """Combines lists into a dictionary and casted to a pandas DataFrame.

  Takes the cartesian product to create all possible combinations.

  Args:
    independent_column::pd.Series
      Full list of valid things.
    other_column::pd.Series
      Full list of valid other things.
    some_threshold::float
      Unnused threshold decimal to show default values, which doesnt need to be
      mentioned when calling the function.

  Returns:
    pd.DataFrame of all combinations of things and other things.
  """
  diccionario = dict(Column1=list(independent_column),
                     OtherColumn=list(other_column))

  # For each value in each list, zip the product together and place result
  # in a DataFrame. Asterisk (*) unpacks an iterable.
  return
    pd.DataFrame(
      list(
        (dict(zip(diccionario, x))
           for x in itertools.product(*diccionario.values()))
        )
      )
```

### Generators

Generators are like a function, except it doesn't `return` a value and exit a
process, it instead collects possible values with `yield` to be called later. It
can pause at a saved state and iterate over an item at command. This is useful
for dealing with large data collections (possibly infinite) that shouldn't be
stored in memory.

```python
def FindExamples(examples:list):
  """Finds examples from the list that match IsGoodExample criteria"""
  for example in examples
    if IsGoodExample(example):
      yield example
```

```python
generator_object = FindExamples(list(df.ItemSeries))

# Get first 10 examples
example_batch = []
for i in range(10):
  example_batch.append(next(generator_object))

# Do something else
ProcessExamples(example_batch)

# Get next 10 examples
example_batch = []
for i in range(10):
  example_batch.append(next(generator_object))
```

### Classes

A class helps define a structure for object-oriented programming by grouping
related functionality and data. `self` is a feature that references a classes
own variables and functions within the class itself. Commonly there is a class
initializer that contains its own variables (including inputs) at the beginning
of the initialization of an instance of the class.

```python
class Employee:
  def __init__(self, user_name, job_family, tenure):
    self.employee_name = user_name
    self.employee_job_family = job_family
    self.employee_tenure = tenure

 def Googleversary(self):
   self.employee_tenure += 1

 def GetJobFamily(self):
   return self.job_family

# Create a new employee instance.
davisv = Employee('dcv', 'multi_analyst', 2)

# Accessors are functions that get class variables.
print(davisv.GetJobFamily())
# multi_analyst

# Mutators are functions that modify a class' variable.
davisv.Googleversary()

# Class variables are public, so they can be accessed directly without a
# function call.
print(davisv.employee_tenure)
# 3
```

TIP: Double undescores in front of a variable make it private to the class,
which would require a function call to retrieve it. E.g.: `__employee_tenure`

### Dictionaries, Tuples, Lists, and Sets

These are all collections of data with similar and properties.

-   Dictionaries store ordered, mutable, and nonduplicate values in key:value
    pairs. Defined by braces `{key:value}` and can contain different types of
    collections.
-   Tuples store ordered, immutable values with parentheses `(,...)` and allows
    duplicates.
-   Lists are ordered and mutable values stored sequentially within brackets
    `[,...]` with any length and any number of duplicates.
    -   Python lists are actually arrays, a collection of ordered items
        (indexable) of the same size.
    -   Lists can actually be stored in different parts of memory and linked by
        the location of each item pointing to the next.
    -   Linked lists point to the next item and can point to the previous item,
        which makes inserting and removing elements easy.
-   Sets store multiple unique items in an unindexed, unordered, and immutable
    fashion within braces `{,...}`.

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>Find a dictionary key corresponding to the maximum value.</h5>

```python
max(months, key=months.get) if months else None
```

</section>

TIP: See [Python time complexity](https://wiki.python.org/moin/TimeComplexity)
for operations on lists, sets, and dictionaries.

## Libraries

Library      | Use Case    | Description                                             | Import/Documentation
------------ | ----------- | ------------------------------------------------------- | --------------------
Pandas       | Analysis    | Perform analytical operations on data tables            | [`import pandas as pd` ](https://pandas.pydata.org/docs/)
NumPy        | Mathematics | Optimized array and matrix manipulation                 | [`import numpy as np`](https://numpy.org/)
SciPy        | Mathematics | Algorithms and convinient math funcs built on the NumPy | [`from scipy import <Fn>`](https://docs.scipy.org/doc/scipy/reference/index.html)
SciKit-Learn | Modeling    | Machine Learning models and data mining tools           | [`from sklearn import <Class>`](https://scikit-learn.org/stable/index.html)
StatsModel   | Modeling    | Hearty statistical modeling                             | [`import statsmodels.api as sm`](https://www.statsmodels.org/stable/index.html)
Matplotlib   | Data Viz    | Basic level data visualization                          | `import matplotlib.pyplot as plt`
Seaborn      | Data Viz    | Easier, prettier version of matplotlib                  | `import seaborn as sns`
Altair       | Data Viz    | Declarative Vega visualizations, Interactive charts     | `import altair as alt`
Plotly       | Data Viz    | Plotly.express - Easy and advanced charting             | `import px`

### Pandas

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>Join in Pandas just like SQL.</h5>

```Python
selected_cols = ['identifier', 'column_1', 'column_2']
df_final_map = (
    df_base[selected_cols]
      .merge(df_map_1, how='left', on='column_2')
      .merge(df_map_2, how='left', on=[primary_key, 'column_1'])
      .fillna(0)
  )
```

</section>

## Colab

### Cell Magic

TIP: `%` evaluates in-line runs, `%%` evaluates the entire cell execution.

Time cell executions: `%timeit RunFunction(df)`

Evaluate memory use: `%memit RunFunction(df)`

Line profiler runs on a certain command to profile the execution
[magic-prun](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-prun).

#### I/O

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>Upload a file to a cloud instance from your local machine.</h5>

```python
import some_tool
uploaded = some_tool.upload_files()
csv = uploaded.files['example.csv']
```

Now we have a copy of example.csv in the runtime.

</section>

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>Read a file.</h5>

```python
# TODO(DCV): Replace file_opener

action_path = '/system/cell/home/my_data_path/ttl=180d/upload.csv'
for f in file_opener.Glob(action_path):
  df = pd.read_csv(file_opener.Open(f))
```

</section>

<section class="zippy" style="margin-left: 40px; border: 1px solid #ddd;">
  <h5>F1 Client Query Builder (Not recommended).</h5>

```python
# TODO: Add imports. Connect to a SQL engine.

client = sql_engine.Client('database')
query_request = sql_engine.QueryBuilder(
    sql_engine.RequestOptions(),
    'SELECT * FROM dcv.MyTable', None)

# TODO: Efficiently build a dataframe.

print([x for x in client.Query(query_request).GetIterator()])

#Output:
[('col1_row1',), ('col1_row2',), ...]
```

</section>

## Visualization

TODO: Add visualization notes.

## Other

#### Logging / Debugging

# link to a logging reference

-   Logging is automatically in C++ mode `logging.use_cpp_logging()`
-   Can switch to python logging mode with `logging.use_python_logging()`

# tracer notes
