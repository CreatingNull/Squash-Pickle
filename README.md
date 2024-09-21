# Squash Pickle

Like a pickle, only smaller\*.

Tiny python package that compresses your pickles using gzip.
Quacks like a pickle.

\* For small objects (\< 100 bytes) gzip overhead can end up increasing size.
Only squash your pickles when you are working with big objects.

______________________________________________________________________

## Getting Started

First install the package, this has no additional dependencies:

```shell
pip install squashpickle
```

Then simply replace your `pickle` calls with `sqaushpickle` ones.
