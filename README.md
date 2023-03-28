# publishing_abclf

## Prepare directory structure

My main source is [this article](https://realpython.com/pypi-publish-python-package/#create-a-small-python-package). As per the tutorial I prepare the structure as so:

```
realpython-reader/
│
├── src/
│   └── reader/
│       ├── __init__.py
│       ├── __main__.py
│       ├── config.toml
│       ├── feed.py
│       └── viewer.py
│
├── tests/
│   ├── test_feed.py
│   └── test_viewer.py
│
├── LICENSE
├── MANIFEST.in
├── README.md
└── pyproject.toml
```

In my case the structure will look like this:

```
abclf/
│
├── src/
│   └── abclf/
│       ├── __init__.py
│       ├── __main__.py
│       ├── config.toml
│       ├── unbalanced.pickle
│       └── balanced.pickle
│
├── LICENSE
├── MANIFEST.in
├── README.md
└── pyproject.toml
```


## 2023-03-21T09:35:26

I can install the package locally, but the static lexicon files are not propagated.

I included them in MANIFEST.in (`include src/abclf/*.pickle`) and in setup.py: 
```python
from setuptools import setup

include_package_data = True
package_data = {
    'abclf': ['*.pickle'],
}
setup()

```

but so far nothing helped. In the end I hardcoded unballanced lexicon in `__init__.py`.

# 2023-03-21T12:18:07

Once the code is ready, which can be verified with `python -m pip install -e .` and testing in the python shell,
build files were made with `python -m build`. This creates a directory `dist`:
```
abclf/
├── dist/
├── src/
│   └── abclf/
│       ├── __init__.py
│       ├── __main__.py
│       ├── config.toml
│       ├── unbalanced.pickle
│       └── balanced.pickle
│
├── LICENSE
├── MANIFEST.in
├── README.md
├── requirements.txt
└── pyproject.toml

```

with a whl file and a tar.gz. The package is finally uploaded with twine:

```bash
twine upload dist/*
```