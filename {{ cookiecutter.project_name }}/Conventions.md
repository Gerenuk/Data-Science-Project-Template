Project Organization
====================

```
    ├── input              <- Data can go into this folder directly unless it fits a subfolder. Everything that scripts take as input, goes here (except models).
    │   ├── temp           <- Intermediate data that has been transformed. Can be re-generated. Expect this to be lost.
    │   └── raw            <- The original, unprocessed, immutable data dump as it was received.
    │
    ├── doc                <- Project/data documentation. Sphinx-doc is recommended (generate with sphinx-quickstart)
    │
    ├── explore            <- Exploration scripts and results. Your personal folder is created here.
    │   └── <person>       <- Personal exploration which is not meant to be used or understood by others. Subfolders at will.
    │
    ├── models             <- Trained models and meta-information they used
    │
    ├── results            <- Analysis, reports, presentations and other deliveries. Not meant to be read by scripts.
    │
    ├── scripts            <- Source code for this project. Put key(!) general scripts directly here.
    │   │                     (!) Only for cleaned code stored as *.py files (since it is Git-friendly and Jupyter notebooks are not)
    │   ├── lib            <- Shared/helper/library functions, to be imported/re-used among data scientists and stages. Add this to your PYTHONPATH.
    │   ├── prepare        <- Scripts to transform/prepare the data and create features. Will be used less as modeling begins.
    │   └── temp           <- Short-term scripts which will probably not be delivery at the end. Promote scripts if they become important.
    │
    └── README.md          <- Short project description, contacts and data sources. (Markdown format)
```

Notes
=====
* always create subfolders as needed
* you may delete the .gitkeep file, once the directory isn't empty anymore. These files exist, since Git doesn't check in empty directories

Data conventions
================

General
-------
* if the data format supports it, use correct types for NULL, dates, ...
* remember to keep `/data/raw` really as it was delivered, though
* the Feather format is a nice option for tables
* use Parquet for large files on HDFS

Text format data
----------------
* avoid text formats if possible, but if you only gets text format, try to have the following:
* use `\t` as separator
* use *.tsv ending
* save dates in iso8601 format (e.g. `2016-07-12T02:59:08Z`; truncation possible, by skipping last parts as needed)
* for missing value use `#NA` (see [pandas.read_csv](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html))s

Feature naming
==============

* All feature names should be prefixed with `<origin>_` where origin is a label for the source
* Existing columns should be converted to lower-case Python keyword-compatible names for convenience and prefixed by a data source tag (letters, digits, underscore)
* Avoid reserved keywords (`keyword.kwlist`) and common [Python](https://docs.python.org/3/library/functions.html)/[http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html](Pandas) functions as names
* Generated features should be prefixed by a script specific label (e.g. use `feat_prefix="ft_"` in the script)

Data science process
====================
* make the whole pipeline runnable by a single command
* fill in NULL values only at end of feature processing (or calculations based on these substitute values will give misleading results)
* create a text file with the list of (numeric) features used in modeling (excluding target column)

Open ideas
==========
* set some environment variable
* "external" data source separate folder?
* set your PYTHONPATH to scripts/lib
* name_mapping.yml: Mapping from new shortened names to old names in raw data
* projectconfig.py: Config with global variables and commands for project
* dependency framework: home-made, Drake, pydoit, ... (no best option found yet)
* Script naming:
    * mark scripts which produce data which is re-used by other scripts (maybe prepend ">")
    * rename scripts to include all steps and results
    * possibly mark dependencies by enumeration
* Feature type abbrevations:
    * 'n' for numerical variables; these generally have a zero point and can be added/divided
    * 'i' for integer numerical variable, but only if good reason to no use 'n'
    * 'd' for numerical "difference" variables which do not have a zero point; you can take differences, but adding or dividing usually doesn't make sense
    * 'c' for categorical variables
    * 'o' for ordered(ordinal) categorical variables
    * 'b' for binary variables, encoded as 0 / 1 integer.
    * 't' for timestamp (any complete, compatible time formats which occur in the data set)
    * 's' for string; a free-text format which has too many different values to be seen as categorical
