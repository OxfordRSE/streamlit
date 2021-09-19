---
title: "Adding interactivity"
subtitle: "Sliders, dropdowns, and more"

date: 2021-09-06T00:00:00+01:00
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

Notice how we did not have to write any boilerplate to get a working Streamlit app?
Streamlit repeatedly re-runs the entire file `app.py`, so the entire app file is being looped over and over.
This means you (as the app developer) do not have to handle any event loops or callbacks yourself.

Too add something more interesting to the app, we could take a quick look at:

- the [Streamlit getting started documentation](https://docs.streamlit.io/en/stable/getting_started.html).
- the [Streamlit API cheat sheet](https://share.streamlit.io/daniellewisdl/streamlit-cheat-sheet/app.py)


## Static content

We can display all sorts of static content such as variables, but also text, maths and code.
The latter three can be written using standard [markdown](https://commonmark.org/) syntax.

### Variables

```python
a = 12.34
st.write(a)
```

The `st.write` method can display many things including all basic data types as well as Pandas data frames, numpy arrays and more.

### Text

Text can be displayed with `st.write`:

```python
st.write('Some text here')
```

but also with markdown, which also allows us to add styling:

```python
st.markdown('Text can be **bold** and _italic_.')
```

```python
st.markdown(r'''Raw strings can be very useful: we can
- create
- bullet
- lists

and other Markdown things like horizontal lines

---
''')
```

### Maths

There is a method for displaying maths using LaTeX, but for consistency we can continue using markdown.
Display equations use the normal `$$` delimiters, whereas inline equations use a single `$`:

```python
st.markdown(r'''
$$
i\hbar\frac{\partial}{\partial t} \Psi(x,t) = \left [ - \frac{\hbar^2}{2m}\frac{\partial^2}{\partial x^2} + V(x,t)\right ] \Psi(x,t)
$$
''')
```

### Code

Similarly, we can display code with syntax highlighting using markdown.
We simply put the code between triple backticks, indicating the language on the first line of backticks:

{{< highlight python >}}
st.markdown(r'''
```c
int main() {
  return 0;
}
```
''')
{{< / highlight >}}


## Dynamic content

What we really want is for users to be able to interact with our app.
Streamlit makes this very straightforward with a growing number of widgets.

The return type of a widget is usually a basic Python type, meaning we use them as follows:

### Checkboxes

To display a checkbox, we can simply add:

```python
st.checkbox('Tick me')
```

But this allows us to affect whether elements are visible or not:

```python
if st.checkbox('Tick me to see'):
  st.write(42)
```

### Dropdown selections

```python
selection = st.selectbox('Choose an option', ["Nothing here", "or here", "but something here"])

if selection == "but something here":
  st.write("You found me!")
else:
  st.write("Try again")
```

### Sliders

Sliders are integer by default:

```python
int_val = st.slider("Pick an integer", min_value=1, max_value=10, value=5)
st.write(int_val)
```

But they can be floating point if you pass floating point values:

```python
float_val = st.slider("Pick a float", min_value=1.0, max_value=10.0, value=5.0, step=0.1, format="%.1f")
st.write(float_val)
```

## Layout

There are several layout options available for Streamlit apps including expanders (which we will see later), columns, and the sidebar.

### Sidebar

A special layout element in Streamlit is the sidebar.
We can add all kinds of elements as normal, but with the following syntax:

```python
st.sidebar.write("Adding text to the sidebar...")
```

We now know everything we need in order to make something useful!

{{< admonition type="note" open=true >}}
Make sure you add and commit these changes to Git and push to GitHub.

```bash
git add -all
git commit -m "commit message"
git push
```
{{< /admonition >}}