---
title: "Building something useful"
subtitle: "Interactive visualisation of statistical distributions"

date: 2021-09-07T00:00:00+01:00
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

We're now ready to build something useful!
The plan is to build an interactive app for displaying and exploring statistical distributions. 

## Distribution selector

### Imports

We are going to draw charts, use data frames, and interact with Scipy's stats module, so first, starting from a blank slate, we need to import the following modules:

```python
import altair as alt
import numpy as np
import pandas as pd
import streamlit as st

import scipy.stats
```

### Selector in the sidebar

Let's first allow the user to select a distribution by adding a dropdown selection to the sidebar.
When a distribution is selected, display its name:

```python
st.title('Statistical distributions')

dist_selector = st.sidebar.selectbox(
    "Pick a distribution:",
    ("Beta", "Normal"),
    index=1
)

st.header(dist_selector)

if dist_selector == 'Normal':
    pass
```


### Distribution parameters

Next, let's add sliders for selecting parameter values:

```python
param_range = st.sidebar.slider('Range', value=20.0, min_value=0.0, max_value=100.0, step=0.1, format='%.1f')
param_mean = st.sidebar.slider('Mean', value=0.0, min_value=-30.0, max_value=30.0, step=0.1, format='%.1f')
param_std = st.sidebar.slider('Standard deviation', value=3.0, min_value=0.1, max_value=20.0, step=0.1, format='%.1f')
```


## Displaying properties

### PDF

We will use [Altair](https://altair-viz.github.io/) to produce interactive plots given the selected parameters.
Let's first create a plot of the Normal distribution's probability density function:

```python
x = np.linspace(start=-param_range, stop=param_range, num=400)

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
```

### CDF

We might also want to plot the cumulative density function.
The code for this will be almost identical:

```python
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
```

### Make each section expandable

It's unlikely that a user will want to see everything at the same time: let's make use of the `expander` widget to make the elements collapsible.
Simply add a `with` block as so:

```python
with st.expander('Plot of PDF', expanded=True):
  # PDF code

with st.expander('Plot of CDF', expanded=False):
  # CDF code
```

### Display formulae

Users might want to see the mathematical formulae for the Normal distribution.
Fortunately that's very straightforward: we can simply add a markdown element:

```python
with st.expander('Formulae', expanded=False):
  st.markdown(r'''Parameters:
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
''')
```

### Display LaTeX for formulae

Let's add one more section.
It is often time-consuming to write out the formulae for distributions in a paper or thesis.
We can easily provide users with copyable LaTeX code to typeset the formulae themselves:

{{< highlight python >}}
with st.expander('LaTeX', expanded=False):
  st.markdown(r'''Moments:
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
''')
{{< / highlight >}}

## Exercise

Add a similar set of expanders for a second distribution.
