---
title: "A first app"
subtitle: "Create and run a Streamlit app locally"

date: 2021-09-05T00:00:00+01:00
lastmod: 2021-09-19T00:00:00+01:00

fontawesome: true
linkToMarkdown: true

toc:
  enable: true
  keepStatic: false
  auto: false
code:
  copy: true
math:
  enable: true
share:
  enable: true
comment:
  enable: true
---

In this section we will create an empty GitHub repository, clone it, and create an extremely basic Streamlit app that we will be able to view in a web browser.

The app will not do much at all, but once we have completed this checkpoint we will be set up with everything we need for the rest of the workshop.

## Set up a repository to work with

### Create a Github repository

Create a new repository on [GitHub](https://github.com/).
Make sure that:

- the repository is **public**
- tick "Add a README file"
- tick "Add .gitignore" (recommended) and select Python
- tick "Choose a license" (recommended) and select your preferred license

Click "Create repository".


### Clone the repository

Clone the repository to your computer either using Git on the command line, or using your preferred Git client.

If using Git on the command line, navigate to the directory you want to use and type:

```
git clone https://github.com/<your-github-usernam>/<your-repository-name>.git
```

## Set up a Python environment to work with

### Create a Python virtual environment

From the command line, navigate to the directory you cloned your repository to.
Create a [virtual environment](https://docs.python.org/3/tutorial/venv.html):

```bash
python3 -m venv venv
```

Activate the virtual environment.
On Windows, run:

```PowerShell
venv\Scripts\activate.bat
```

On Linux and macOS run:

```bash
source venv/bin/activate
```

### Update the basic Python modules

Run the following command to make sure the environment is up to date:

```bash
python3 -m pip install --upgrade pip setuptools wheel
```

## Install required Python packages

### Create a requirements file

We will be using several Python packages during this workshop and we may as well install all of them now.
To do that we will create an empty file `requirements.txt` in the base directory of the Git repository.
Open that file in your text editor.

In that file, put the following Python packages:

```txt
altair
numpy
pandas
scipy
streamlit
```

### Install the requirements

To install all the requirements specified in the file we just created, run:

```bash
python3 -m pip install -r requirements.txt
```


## Build and run a basic streamlit app

### Create a python file

Create an empty Python file `app.py` in the base directory of the Git repository.
Open that file in your text editor.

Add the following code.

```python
import streamlit as st

st.title('My first Streamlit app')
```

Save the file.

### Run the streamlit app

From the command line, run:

```bash
streamlit run app.py 
```

This should automatically load the app in your web browser.
If not, copy-and-paste the "Local URL" into your browser.

You should see an empty app with your title visible in the middle of the screen.
Your first Streamlit app!


{{< admonition type="note" open=true >}}
Make sure you add and commit these new files to Git and push to GitHub.

```bash
git add -all
git commit -m "commit message"
git push
```
{{< /admonition >}}
