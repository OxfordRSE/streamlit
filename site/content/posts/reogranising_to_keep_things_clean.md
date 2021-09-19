---
title: "Keeping things clean"
subtitle: "Keep app code clean by moving functionality to libraries"

date: 2021-09-08T00:00:00+01:00
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

So far so good!
We have an app displaying statistical distributions.
A user can select the distribution and move sliders to change parameter values.

The main problem is we now have a single python file with a few hundred lines of code.
Not the end of the world, yet, but there are dozens of distributions we might want to add.
Things could quickly get out of hand!

## Move functionality to a module

### What do we need to keep?

The high level flow of an app should be kept in `app.py`.
It is certainly useful for the developer to see a good overview of how the app works in the main Python file.

All of the code specific to the normal distribution, however, can be moved.

### Creating a module

Let's create a Python module called `distributions`.
This consists of a directory `distributions` with at least one file, `__init__.py`.
We will also create another file for each distribution in our app.

Create a directory and a files so that the Git repository top level directory contains:

```
repository_root/
├── app.py
├── distributions/
    └── __init__.py
    └── normal.py
```

In the file `__init__.py`, paste the following code:

```python
from .normal import normal_distribution
```

and in `normal.py` we will put all the Normal-specific functionality from the last section:

{{< highlight python >}}
import altair as alt
import numpy as np
import pandas as pd
import streamlit as st

import scipy.stats


def normal_distribution():
    param_range = st.sidebar.slider('Range', value=20.0, min_value=0.0, max_value=100.0, step=0.1, format='%.1f')
    param_mean = st.sidebar.slider('Mean', value=0.0, min_value=-30.0, max_value=30.0, step=0.1, format='%.1f')
    param_std = st.sidebar.slider('Standard deviation', value=3.0, min_value=0.1, max_value=20.0, step=0.1, format='%.1f')

    x = np.linspace(start=-param_range, stop=param_range, num=400)

    with st.expander('Plot of PDF', expanded=True):
        y = scipy.stats.norm.pdf(x, param_mean, param_std)
        pdf_data = pd.DataFrame.from_dict({
            'x': x,
            'probability density': y,
        })
        line_chart = alt.Chart(pdf_data).mark_line().encode(
            x='x',
            y='probability density',
            tooltip=['x', 'probability density'],
        )
        st.altair_chart(line_chart.interactive(), use_container_width=True)

    with st.expander('Plot of CDF', expanded=False):
        y = scipy.stats.norm.cdf(x, param_mean, param_std)
        cdf_data = pd.DataFrame.from_dict({
            'x': x,
            'cumulative density': y,
        })
        line_chart = alt.Chart(cdf_data).mark_line().encode(
            x='x',
            y='cumulative density',
            tooltip=['x', 'cumulative density'],
        )
        st.altair_chart(line_chart.interactive(), use_container_width=True)

    with st.expander('Formulae', expanded=False):
        st.markdown(formulae_text)

    with st.expander('LaTeX', expanded=False):
        st.markdown(latex_text)


formulae_text = r'''Parameters:
- mean: $\mu\in\mathbb{R}$
- standard deviation: $\sigma\in\mathbb{R}^+$
---
Support:
- $x\in\mathbb{R}$
---
Moments:
- $\mathrm{E}(X) = \mu$
- $\mathrm{Var}(X) = \sigma^2$
---
Probability density function (PDF):

$$
f(x|\mu,\sigma) = \frac{1}{\sqrt{2\pi\sigma^2}}\text{exp}\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
$$
---
Cumulative distribution function (CDF):

$$
F(x|\mu,\sigma) = \frac{1}{2}\left[1+\text{erf}\left(\frac{x-\mu}{\sigma\sqrt{2}}\right)\right]
$$

where

$$
\text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_{0}^{x} e^{-t^2}\mathrm{d}t
$$
is the error function.
'''

latex_text = r'''Moments:
```latex
\mathrm{E}(X) = \mu
```
```latex
\mathrm{Var}(X) = \sigma^2
```
---
Probability density function (PDF):
```latex
f(x|\mu,\sigma) = \frac{1}{\sqrt{2\pi\sigma^2}}\text{exp}\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
```
---
Cumulative distribution function (CDF):
```latex
F(x|\mu,\sigma) = \frac{1}{2}\left[1+\text{erf}\left(\frac{x-\mu}{\sigma\sqrt{2}}\right)\right]
```
where
```latex
\text{erf}(x) = \frac{2}{\sqrt{\pi}}\int_{0}^{x} e^{-t^2}\mathrm{d}t
```
is the error function.
'''

{{< / highlight >}}

{{< admonition type="note" open=true >}}
By organising the Normal distribution code into a function, we can also put our long raw strings at the bottom of the file which really helps keep the code neat.
{{< /admonition >}}


## Use the module from our app

Now that the Normal distribution functionality is in a separate module, we can simply import it and call the new function:

```python
import streamlit as st

from distributions import normal_distribution


st.title('Statistical distributions')

dist_selector = st.sidebar.selectbox(
    "Pick a distribution:",
    ("Beta", "Normal"),
    index=1
)

st.header(dist_selector)

if dist_selector == 'Normal':
    normal_distribution()
```

The entire app is now little more than a dropdown box and a function call.


{{< admonition type="note" open=true >}}
Don't forget to add and commit all changes to Git and push to GitHub.

```bash
git add -all
git commit -m "commit message"
git push
```
{{< /admonition >}}
