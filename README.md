# ![Logo](resources/Squash-Pickle.png) Squash Pickle

[![License](https://img.shields.io/:license-mit-blue.svg)](https://github.com/CreatingNull/Squash-Pickle/blob/master/LICENSE.md)
[![PyPI - Python Version](https://img.shields.io/pypi/pyversions/squashpickle?logo=python&logoColor=white)](https://pypi.org/project/squashpickle/)
[![PyPI](https://img.shields.io/pypi/v/squashpickle?logo=pypi&logoColor=white)](https://pypi.org/project/squashpickle/#history)
[![pre-commit.ci status](https://results.pre-commit.ci/badge/github/CreatingNull/Squash-Pickle/main.svg)](https://results.pre-commit.ci/latest/github/CreatingNull/Squash-Pickle/main)
[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/CreatingNull/squash-pickle/run-tests.yaml?branch=main&label=tests&logo=github)](https://github.com/CreatingNull/squash-pickle/actions/workflows/run-tests.yaml)

Like a pickle, only smaller\*.

Tiny python package that compresses your pickles using gzip.
Quacks like a pickle.

\* For small objects (< 100 bytes) gzip overhead can end up increasing in size.
Only squash your pickles when you are working with big objects.

______________________________________________________________________

## Getting Started

First install the package, this has no additional dependencies:

```shell
pip install squashpickle
```

Then replace your `pickle` calls with `squashpickle` ones.
`squashpickle` implements, `dump`, `dumps`, `load`, and `loads` functions.

To avoid confusion, it is recommended to dump and load to file with `.gz` extension eg: `pkl.gz`, but there is nothing enforcing this.

______________________________________________________________________

## Performance

The GZIP compression can have a **HUGE** impact on large objects.
Say you are pickling something like a polars / pandas dataframe; these pickles may end up being hundreds of MBs.
With squashpickle can get compression ratios exceeding 10x.

For example, if we load a large dataframe of Australian weather data.
Using pickle, this object serializes to `37794198` bytes (~37.8MB).
Dumping the same dataframe with `squashpickle` results in `3370363` bytes (~3.4MB), around 9% of the overall file.

```python
import polars as pl
import pickle
import squashpickle

df = pl.read_csv(r"C:\temp\weatherAUS.csv", null_values=["NA"])
print(len(pickle.dumps(df)), len(squashpickle.dumps(df)))
```

As with any compression, there is a performance cost to achieving the smaller files.
For objects \<1MB this is hardly noticeable, but for objects hundreds of MBs the delay can be significant.
It'll depend on your use case if this is a worthwhile tradeoff.

______________________________________________________________________

## Security

`squashpickle` uses Python's `pickle` and `gzip` modules internally. As with `pickle` and `gzip`, never load data from untrusted sources. Deserializing malicious pickle data can execute arbitrary code.
