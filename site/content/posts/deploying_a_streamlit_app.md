---
title: "Deploying a Streamlit app"
subtitle: "Use Heroku to deploy a Streamlit app"

date: 2021-09-09T00:00:00+01:00
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

## Deployment options

### Streamlit Cloud

So far we have been viewing the app locally.
We want to show the world!

Streamlit can be easily deployed on a wide variety of cloud infrastructure; in fact, anywhere a Python app can be hosted.
Streamlit provides a [guide](https://discuss.streamlit.io/t/streamlit-deployment-guide-wiki/5099) on how to deploy apps.

Even better than that is [Streamlit Cloud](https://discuss.streamlit.io/t/streamlit-deployment-guide-wiki/5099) that makes it trivial to deploy public apps for free.
Unfortunately you currently need to apply for an invite to the beta version of their service.


### Heroku

Because of this limitation, we will be deploying our apps to [Heroku](https://www.heroku.com/), which is very nearly as straightforward as using Streamlit Cloud.

## Deploying to Heroku 

### Adding the necessary configuration files

We need to add two small configuration files before deploying to Heroku.

Create a file in the base directory of the Git repository, `setup.sh`, and paste the following code:

```txt
mkdir -p ~/.streamlit/
printf "[server]\nheadless=true\nenableCORS=false\nport=%s\n" "${PORT}" > ~/.streamlit/config.toml
```

Create another file in the base directory of the Git repository, `Procfile`, and paste the following code:

```txt
web: sh setup.sh && streamlit run app.py
```

{{< admonition type="note" open=true >}}
Make sure you add and commit these new files to Git and push to GitHub.

```bash
git add -all
git commit -m "commit message"
git push
```
{{< /admonition >}}


### Linking Heroku and GitHub

Follow these steps to deploy your Streamlit app:

1. Log in to the [Heroku dashboard](https://dashboard.heroku.com/apps).
2. Select 'New' -> 'Create new app'
3. Name your app: the eventual URL will be `https://<app_name>.herokuapp.com/`
4. Select 'Europe', for slightly lower latency
5. Under 'Deployment Method' select 'GitHub'
6. Under 'Connect to GitHub' select your repository and click 'Search', then 'Connect'
7. Under 'Automatic deploys' click 'Enable Automatic Deploys'
8. On this first time, under 'Manual deploy' click 'Deploy Branch'

You should see a window showing the deployment happening.
This may take a minute.

When the process is finished, you should see 'Your app was successfully deployed': click 'View' to see it in action!

## Exercise

In the remaining time, add new features to the app and play around with other Streamlit methods and widgets.
