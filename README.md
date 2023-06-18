# deadcode
`deadcode` package implements `DC100` - `unused-global-name` check for detecting
defined variables/functions/classes but never used in a whole code base.

# Installation
```shell
pip install deadcode
```

## Usage
```shell
deadcode .
```

Command line options:
| Option                    | Type  | Meaning  |
|---------------------------|-------|----------|
|`--exclude`                | list (comma separated values) | Path expressions to completely skips files from being analysed. |
|`--ignore-names`           | list (comma separated values) | Removes provided list of names from the error output. |
|`--ignore-names-in-files`  | list (comma separated values) | Unused names from files matching provided path expressions. |

Command line option usage example:
```
deadcode . --exclude=venv,tests --ignore-names=BaseTestCase --ignore-names-in-files=migrations
```

The same options can be provided in `pyproject.toml` settings file:
```
[tool.deadcode]
exclude = ["venv", "tests"]
ignore-names = ["BaseTestCase"]
ignore-names-in-files = ["migrations"]
```

# Contributing
- `make check` - runs unit tests and other checks using virtual environment.

# Rationale
`ruff` and `flake8` - don't have rules for unused global code detection, only for local ones `F823`, `F841`, `F842`.
`deadcode` package tries to add a new `DC100` check for detecting variables/functions/classes which are not used in a whole code base.

There is an alternative `vulture` package, which provides many false positives. `deadcode` - tries to find less, but findings are with higher confidence.
`deadcode` - is supposed to be used inline with other static code checkers like `ruff`.

# Known limitations
If the same unused name is repeated in several files - it wont be detected.

Files with syntax errors will be ignored, because `deadcode` uses `ast` to build abstract syntax tree for name usage detection.

It is assumed that `deadcode` will be run using the same Python version as the checked code base is implemented in.
