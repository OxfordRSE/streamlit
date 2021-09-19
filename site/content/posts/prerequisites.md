---
title: "Prerequisites"
subtitle: "Getting everything set up"

date: 2021-09-04T00:00:00+01:00
lastmod: 2021-09-19T00:00:00+01:00

fontawesome: true
linkToMarkdown: true

toc:
  enable: true
  keepStatic: false
  auto: true
code:
  copy: true
math:
  enable: true
share:
  enable: true
comment:
  enable: true
---

There are a small number of tools you need in order to create and deploy [Streamlit](https://streamlit.io/) apps.

## Software

### Python

Streamlit is a Python package, and requires [Python 3.6+](https://www.python.org/downloads/).
To verify you have an appropriate version of Python, run:

```
python --version
```

from a command line.
If you see a Python 2 version then try

```
python3 --version
```

instead.

{{< admonition type="tip" open=true >}}
If you are installing Python on Windows, make sure you tick the "add to path" option when presented.
{{< /admonition >}}


### Any text editor

We will be writing some Python code.
Any text editor will will be absolutely fine, but a couple of recommendations are:

- [Visual Studio Code](https://code.visualstudio.com/)
- [PyCharm Community](https://www.jetbrains.com/pycharm/download)


### Git

We will be working with a Git repository hosted on [GitHub](https://github.com/).
You should make sure you have installed [Git](https://git-scm.com/).
You can either use Git from the command line, or use a graphical application such as

- [GitHub Desktop](https://desktop.github.com/)
- [GitKraken](https://www.gitkraken.com/)

## Services

### GitHub

We will be using [GitHub](https://github.com/) to host a Git repository for this workshop.
Please make sure you have a GitHub account and that you are logged in.

### Heroku

[Heroku](https://www.heroku.com/) is a cloud platform for hosting apps.
We will be using Heroku to host our Streamlit apps so they are live for everyone on the internet to play with.
Please make sure you have a Heroku account and that you are logged in.