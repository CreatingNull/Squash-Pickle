# ![Logo](resources/Squash-Pickle.png) Squash Pickle

Like a pickle, only smaller\*.

Tiny python package that compresses your pickles using gzip.
Quacks like a pickle.

\* For small objects (< 100 bytes) gzip overhead can end up increasing size.
Only squash your pickles when you are working with big objects.

______________________________________________________________________

## Getting Started

First install the package, this has no additional dependencies:

```shell
pip install squashpickle
```

Then simply replace your `pickle` calls with `sqaushpickle` ones.
`squashpickle` implements, `dump`, `dumps`, `load`, and `loads` functions.

------------------------------------------------------------------------

## Performance

The GZIP compression can have a **HUGE** impact on large objects.
Say you are pickling something like a polars / pandas dataframe, these pickles may end up being hundreds of MBs.
With squashpickle can get compression ratios exceeding 10x.

For example if we load a large dataframe of australian weather data.
Using pickle this object serialises to `37794198` bytes (~37.8MB).
Dumping the same dataframe with `squashpickle` results in `3370363` bytes (~3.4MB), around 9% of the overall file.

```python
import polars as pl
import pickle
import squashpickle

df = pl.read_csv(r"C:\temp\weatherAUS.csv", null_values=["NA"])
print(len(pickle.dumps(df)), len(squashpickle.dumps(df)))
```

As with any compression, there is a performance cost to achieving the smaller files.
For objects <1MB this is hardly noticeable, but for objects hundreds of MBs the delay can be significant.
It'll depend on your use case if this is a worthwhile tradeoff.
