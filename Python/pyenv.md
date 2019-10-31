# pyenv

Simple Python version management

https://github.com/pyenv/pyenv

## Install python from pyenv (with brew)

Install pyenv via brew

```bash
brew install pyenv
```

Install Prerequisites

```bash
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git
```

Install python 3.8.0

```bash
$ pyenv install 3.8.0
Downloading Python-3.8.0.tar.xz...
-> https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tar.xz
Installing Python-3.8.0...
```

## How to fix: 'readline extension was not compiled'

The problem:

```bash
$ pyenv install 3.8.0
...
python-build: use readline from homebrew
WARNING: The Python readline extension was not compiled. Missing the GNU readline lib?
```

1. Uninstall `readline` from `brew`
2. Temporarily remove `brew` from `$PATH`

## References

- https://github.com/pyenv/pyenv
- https://github.com/pyenv/pyenv/wiki/Common-build-problems