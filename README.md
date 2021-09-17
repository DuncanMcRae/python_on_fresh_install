# python_on_fresh_install
How to setup a new linux distro for use with python

_Updated: 2021-09-17_

# THE TLDR;
pyenv -> pipx -> poetry -> python  
also: git  
In general [Best practice for setting up a python env](https://towardsdatascience.com/best-practices-for-setting-up-a-python-environment-d4af439846a)

# PYENV
What is it: pyenv lets you easily switch between multiple versions of Python as well as create virtual environments. We will only be using if for the python versions and **NOT** the virual envs (poetry will do that).
How to install it:
* [Install dependancies](https://github.com/pyenv/pyenv/wiki#suggested-build-environment)
* Install it from git clone following [this guide](https://github.com/pyenv/pyenv)
* If Linux using  the terminal zsh, you need to install the echos in to `~/.zprofile` (might not exist) and `~/.bash_profile`
* init the shell with the zsh echo in to the `~./zshrc`

How to use it:
* list available versions of python `pyenv install -l`
* install the version that you want `pyenv install 3.9.7`
* update the shims after install with `pyenv rehash`
* set version as global `pyenv global 3.9.7` or local (that folder) `pyenv local 3.9.7`
* check what versions of python are installed `pyenv versions` and what is running `pyenv --version`

# PIPX
What is it: pipx allows you to install global python tools that will be useable throughout all virtual envs.  Linting tools like `mypy`, `flake8`, formatters like `black`, and dependency management tools like `poetry` can be installed once globally and reused across projects  
How to install it:
* [pipx git page](https://github.com/pypa/pipx)
* Make sure you have an active global python working `pyenv global` then install with `pip install pipx`
* Add pipx to `PATH` by running `pipx ensurepath`
* Install tools `pipx black`, `pipx poetry` and the others.

# POETRY


# GIT


# VSCODE
What is it: VisualStudio Code is a software IDE that enables users to install bolt-on languages and extensions and themes as needed.  
How to install it:
* Many ways to install it, but running Linux Manjaro 21.X I could only get python to install in to it if I used the install from snapd.
* Snap is installed by default on a full install of Manjaro, but if you have installed the lite version then you will need to install it [install snap](https://snapcraft.io/docs/installing-snap-on-manjaro-linux)
How to configure it:
* 

