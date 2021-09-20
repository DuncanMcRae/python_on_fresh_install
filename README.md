# python_on_fresh_install
How to setup a new Linux install for use with python (also relevant for MacOS and Windows)

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
* check to see if the shims are at the start of your path `echo $PATH`

How to use it:
* list available versions of python `pyenv install -l`
* install the version that you want `pyenv install 3.9.7`
* update the shims after install with `pyenv rehash`
* set version as global `pyenv global 3.9.7` or local (that folder) `pyenv local 3.9.7`
* check what versions of python are installed `pyenv versions` and what is running `pyenv --version`

# PIPX
What is it: pipx allows you to install global python tools that will be useable throughout all virtual envs.  Linting tools like _mypy_, _flake8_, formatters like _black_, and dependency management tools like _poetry_ can be installed once globally and reused across projects  
How to install it:
* [pipx git page](https://github.com/pypa/pipx)
* Make sure you have an active global python working `pyenv global` then install with `pip install pipx`
* Add pipx to _PATH_ by running `pipx ensurepath`
* Install tools `pipx black`, `pipx poetry` and the others.
* Check black installed `black --version`
* List all installed python libraries `pipx list`. This also shows where the apps are exposed on the $PATH. Take note for vscode.

# POETRY
What is it: [poetry](https://python-poetry.org/) is a python packaging and dependancy management tool that can also create virtual environments. NOTE: `pip` does not check for dependancy issues, so `poetry` is better.  
How to use it:  
* [Basic usage](https://python-poetry.org/docs/basic-usage/)
* you can build a new project using `poetry new project_name` and this will create a _recommended_ project. You can also initialise an existing project using `poetry init`. Initialisation asks you a host of questions to build the `pyproject.toml` file - creating a new project defaults these values. I have found using `poetry new project_name` is better than initialising a project that is already in progress using `poetry init` as this does not appear to insert _python_ in to the `pyproject.toml` file. I have found poetry will fail on some library importing for unknown reasons as _python_ is not present in the toml file - this happens with scipy.
* Adjust the toml file as needed.
* move in to the new project created by poetry `cd project_name`.
* Add thrid party python libraries using `poetry add pandas`. This will initially create a virtual environment for you and if it is not the first time you run it, it will check that there are no dependancy issues.
* Check what libraries are installed using poetry not pip `poetry show`
* You can see the active virtual env `poetry env list` and get a full breakdown of it `poetry env info`. You may need the Path from the last command for configuring vscode.
* You can see all the dependencies in a tree like presentation `poetry show --tree` - this is cool.

# GIT
What is it: Awesome. Git is a tool for tracking changes in files (software). You can fork software, merge forks back to the main branch and push and pull files from are remote repository (github) to your local system.
* setup a two-factor authorisation or create a 'token' in github to give you the ability to push/pull between remote<>local through the github API: _Settings>Developer Settings>Personal Access Tokens_. I just save this token down in my LastPass password manager browser extension as a note and add it to my favourites so I can get at it quickly.
* There is truck loads you can do with git/github, but what I do typically is outlined in the WORKFLOW section below.
* Terminal git and github have different default branches with github being main and git (terminal) master. We change git terminal to change this with `git config --global init.defaultbranch main`.

# VSCODE
What is it: VisualStudio Code is a software IDE that enables users to install bolt-on languages and extensions and themes as needed.  
How to install it:
* Many ways to install it, but running Linux Manjaro 21.1.3 I could only get python to install in to vscode if I used the install from snapd. It failed to install Jupyter any other way.
* Snap is installed by default on a full install of Manjaro, but if you have installed the lite version then you will need to install it [install snap](https://snapcraft.io/docs/installing-snap-on-manjaro-linux)
How to configure it:
* Must have extensions: Bookmarks, Bracket Pair Colorizer 2, GitLens, indent-rainbow, Material Icon Theme, Python (installs Jupyter and Pylance), Python Docstring Generator, vscode-icons. I also theme it with Dracula (as my entire system is themed this way).
* File>Preferences>Settings. Type _python formatting_ and make sure Black Path is correct as per `pipx list`. Also Black Args set with `--line-length\n79`. All changes are saved in to the vscode's settings.json file found `~/.config/Code/User/settings.json`. See below for mine (note: the defaultInterpreterPath changes): 
  
![image](https://user-images.githubusercontent.com/32591094/133871418-e50a6424-9cd5-4dd1-a768-d29c59b663ce.png) 

# WORKFLOW
* Create a new project in GitHub.com and select to add: _README.md_, _LICENCE_ (TheUnlicence) and the _.gitignore_ (Python).
* Use poetry to create a new project `poetry new test_project`.
* Initialise git for the project by moving in to it `cd test_project` and then `git init`. This will create a _main_ git branch on your local system.
* Remove the `README.rst` file as github has no use for it. `rm README.rst`
* In terminal pull the git in to your working directory `git pull https://github.com/DuncanMcRae/test_project`. Note: you may have to provide a username and password/token if it is a private repository.
* Use pyenv to assign a local version of python to the folder `pyenv local 3.9.7`. This creates a hidden `.python-version` file.
* Add python modules `poetry add numpy`. This will error and you need to modify the python requirements in the \*.toml file. This will also, even if it fails, generate a virtual environment. Add all the libraries you need.
* Use `poetry env info` to grab the path for the virtual environment.
* Open vscode and `ctrl+shift+P` or `View>Command Palette`. Type `Python Select Interpreter` then `+ Enter interpreter path` and paste the path from `poetry env info`. You should be able to see the python version and virtual environment in the lower left corner of the status bar inside vscode.
* You can either work with git inside vscode or continue to use the shell. The below workflow will use the shell.
* You can see which files have changed on the local system `git status`, you can add them to the _staging area_ `git add <filename>`. Git takes glob like commands so `git add *` will add all changes. You then `git commit -m "description of commit"` to lock those changes in to your local git database.
* Send the changes to the remote repository `git push <url>`. This should ask you for your username and password. My password is the token I created for remote access to the github API. As we have configured the local system to have main as our init git, this push doesn't create a new pull-request at the remote repository.
* You can then `git pull` and `git push` all day long.

# ADDITIONAL WORKFLOW ROUTINES
* Branching
