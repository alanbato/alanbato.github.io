---
title:  "Goodish practices in Python"
excerpt: "My take on must-have libraries, tools and practices in Python"
---
This post is an attempt to collect all the things I like to use and do when developing projects using Python.
What this post _isn't_ is an objective and thorough analysis of tools and practices that should be taken at face value.

I'd love to hear your suggestions, and PRs are [welcome](https://www.github.com/alanbato/alanbato.github.io).
Let's get started.

{% include figure image_path="https://media.giphy.com/media/3orieUW0YhumQu210I/giphy.gif" caption="Practical Python" %}

### Text Editor
I'm carefully trying not to start an editor war here, but after having used several text editors and IDEs in the past, I've come to really like [Visual Studio Code](https://code.visualstudio.com/).

Lightweight, slim, and highly extensible, VS Code it's a really good tool for writing Python. Plus, it's also [Open Source](https://github.com/Microsoft/vscode).
This is a list some of the awesome [Python features](https://code.visualstudio.com/docs/languages/python) it has built-in and a there's also a [Getting Started Guide](https://code.visualstudio.com/docs/python/python-tutorial) you can follow along to explore them.

Here's a list of some of the plugins I use and the user settings I have set.

Plugins:
* Python
* Django Snippets/Template
* Docker
* Emacs Keymap
* GitLens
* Happy Flasker
* Jinja
* Jupyter
* Material Palenight Theme

Settings:
```
{
    "python.sortImports.path": "~/miniconda2/bin/isort",
    "python.jediPath": "~/.local/lib/python3.5/site-packages/jedi",
    "python.autoComplete.preloadModules": [
        "Numpy",
        "Pandas"
    ],
    "python.jupyter.defaultKernel": "user/bin/python3",
    "python.formatting.autopep8Path": "~/miniconda2/bin/autopep8",
    "editor.formatOnPaste": true,
    "editor.formatOnSave": true,
    "python.linting.flake8Enabled": true,
    "python.linting.pylintEnabled": false,
    "python.linting.lintOnTextChange": false,
    "python.unitTest.pyTestEnabled": true,
    "python.linting.flake8Path": "~/miniconda2/bin/flake8",
}
```

## Environment
{% include figure image_path="https://media.giphy.com/media/3w3uWvsRK08pi/giphy.gif" caption="Getting around is easy when you're a snake." %}

### Folder structure
### Cookiecutter
### Virtual Environments
When you have several Python projects and several dependencies with different versions on each one of those, it's a good idea to start thinking about isolating your projects from one another.

Virtual Environments to the rescue!

[There](https://pypi.python.org/pypi/virtualenv) [have](https://virtualenvwrapper.readthedocs.io/en/latest/index.html) [been](https://github.com/brainsik/virtualenv-burrito) [many](https://docs.python.org/3/library/venv.html) [attempts](https://conda.io/docs/) to create _the_ virtual environment tool for Python. And I know what you're thinking, but no, you won't find the answer to which one is the best here because you can't handle the truth. 

_(Answer: None is the best.)_

I, however, am fond of [Pipenv](Pipenv). Made by Kenneth Reitz ([@kennethreiz](https://twitter.com/kennethreitz)), Pipenv is a tool that is basically an interface to a package manager (`pip`), a virtual environment tool (`virtualenv`), and a dependency specification file (`Pipfile`). Pipenv makes using these three tools as easy as `pipenv install`.

## Code Style
{% include figure image_path="https://media.giphy.com/media/LcoK2zRKbQlUc/giphy.gif" caption="A snake with a lot of style." %}
### PEP 8
There are many style guides when it comes to Python formatting, but many people take [PEP 8](https://www.python.org/dev/peps/pep-0008/) as the official style guide for Python. I recommend reading it at [pep8.org](pep8.org).
### Flake8
Coala????
### Formatting
Black (when it works)


## Docs
{% include figure image_path="https://media.giphy.com/media/T3YmeL5TuhNJu/giphy.gif" caption="The dangers of bad documentation" %}
### Comments & Docstrings
Not many options here, so going with pydocstyle is a good idea :)

## Code
### Iteration
### attrs
https://glyph.twistedmatrix.com/2016/08/attrs.html
### Classes
### Dunder methods
See Fluent Python
### Try...except
### ...else...finally
### Context managers
files
databases

## Exploration
### IPython
### Jupyter Notebook
### Jupyter Lab
### Expose.js

## Debugging
### pdb
### q

## Continuous Integration
### Pytest
### Travis

## Other
### subprocess
### environ
### mail
### APIs
### openpyxl