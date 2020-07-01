# DepHell

Project management for Python. https://dephell.readthedocs.io/index.html

## Convert poetry pyproject.toml to setup.py

```bash
$ dephell deps convert --from pyproject.toml --from-format poetry --to setup.py --to-format setuppy
```

or

Add below config to `pyproject.toml` or `dephell.toml` and run `dephell deps convert`

```toml
[tool.dephell.main]
from = {format = "poetry", path = "pyproject.toml"}
to = {format = "setuppy", path = "setup.py"}
```

