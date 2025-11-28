# Matplotlib

- [Matplotlib](#matplotlib)
  - [Some Useful Links](#some-useful-links)
  - [Introduction](#introduction)
  - [Initial steps](#initial-steps)
  - [Explicit vs Implicit Interfaces](#explicit-vs-implicit-interfaces)
  - [`plot`](#plot)
  - [Adding a title](#adding-a-title)
  - [`grid`](#grid)
  - [Adjusting the x-y Axes](#adjusting-the-x-y-axes)
    - [Hiding the x-y axes](#hiding-the-x-y-axes)
    - [Changing the x-y axes values](#changing-the-x-y-axes-values)
    - [Adding labels to x-y axes](#adding-labels-to-x-y-axes)
    - [Customizing the ticks on the x-y axes](#customizing-the-ticks-on-the-x-y-axes)
  - [Legend for the curves](#legend-for-the-curves)
  - [Saving visualizations](#saving-visualizations)
  - [Colors, Line Styles, and Markers](#colors-line-styles-and-markers)
    - [Colors](#colors)
    - [Line style](#line-style)
    - [Markers](#markers)
    - [Combining colors, markers, and line styles](#combining-colors-markers-and-line-styles)
    - [More Styling](#more-styling)
      - [`plt.tight_layout()`](#plttight_layout)
      - [`tick_params()`](#tick_params)
      - [`hlines()` and `vlines()`](#hlines-and-vlines)
  - [Subplots](#subplots)
  - [Styles](#styles)
  - [Bar Graphs](#bar-graphs)
  - [Scatter Plot](#scatter-plot)
  - [Histogram](#histogram)
  - [Pie Chart](#pie-chart)
  - [Matplotlib Defaults](#matplotlib-defaults)
  - [Interactive vs Non-Interactive mode](#interactive-vs-non-interactive-mode)

<hr>

## Some Useful Links

- https://wesmckinney.com/book/plotting-and-visualization
- [Matplotlib Documentation](https://matplotlib.org/)
- https://www.youtube.com/watch?v=axSTGczvYIE

<hr>

## Introduction

Matplotlib is an integral part of the scientific Python ecosystem, and it is used for visualization. It is an extension of NumPy. It provides a Matlab-like interface for plotting and visualization.

## Initial steps

The matplotlib package is rather large since it encompasses quite a bit of functionality. Fortunately for us, all we need is the `pyplot` module. Rather than importing the whole `matplotlib` package, we will only import the `pyplot` module. Note that `pyplot` is traditionally aliased as `plt`.

```py
import matplotlib.pyplot as plt
```

To show the Matplotlib visualizations in the notebook, you must run the following magic command:

```
%matplotlib inline
```

This forces to show the output inline, directly below the code cell that produces the visualization.

<hr>

## Explicit vs Implicit Interfaces

Matplotlib has two major application interfaces, or styles of using the library:

- An explicit "Axes" interface that uses methods on a Figure or Axes object to build a visualization step by step. This has also been called an "object-oriented" interface.

- An implicit "pyplot" interface that keeps track of the last Figure and Axes created, and adds details to the object it thinks the user wants.

The "Axes" interface is how Matplotlib is implemented, and many customizations and fine-tuning end up being done at this level.

This interface works by instantiating an instance of a Figure class, using a `subplots` method (or similar) on that object to create one or more Axes objects, and then calling drawing methods on the Axes (`plot` in this example):

```py
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.subplots()
ax.plot([1, 2, 3, 4], [0, 0.5, 1, 0.2])
```

We call this an "explicit" interface because each object is explicitly referenced, and used to make the next object. Keeping references to the objects is very flexible, and allows us to customize the objects after they are created, but before they are displayed.

The `pyplot` module shadows most of the Axes plotting methods to give the equivalent of the above, where the creation of the Figure and Axes is done for the user:

```py
import matplotlib.pyplot as plt

plt.plot([1, 2, 3, 4], [0, 0.5, 1, 0.2])
```

We can use either interface to create visualizations. It's useful to take into account below differences between 2 interfaces:

- "Plotting" functions, i.e. functions that add data, are named the same and have identical parameters on the Axes and in pyplot. Example:
  - Axes: `ax.plot(x, y)`
  - pyplot: `plt.plot(x, y)`
- Functions that retrieve properties are named like the property in pyplot and are prefixed with `get_` on the Axes.
  - Axes: `label = ax.get_xlabel()`
  - pyplot: `label = plt.xlabel()`
- Functions that set properties are named like the property in pyplot and are prefixed with `set_` on the Axes.
  - Axes: `ax.set_xlabel("time")`
  - pyplot: `plt.xlabel("time")`

| Operation          | Axes interface             | pyplot interface       |
| ------------------ | -------------------------- | ---------------------- |
| Creating figures   | `fig, ax = plt.subplots()` | `plt.subplots()`       |
| Plotting data      | `ax.plot(x, y)`            | `plt.plot(x, y)`       |
| Getting properties | `label = ax.get_xlabel()`  | `label = plt.xlabel()` |
| Setting properties | `ax.set_xlabel("time")`    | `plt.xlabel("time")`   |

Matplotlib keeps a reference to all of the open figures created via `pyplot.figure` or `pyplot.subplots` so that the figures will not be garbage collected. Figures can be closed and deregistered from pyplot individually via `pyplot.close`; all open Figures can be closed via `plt.close('all')`.

<hr>

## `plot`

The plot method needs the values of x and y and the plotting options.

Here is an example using the pyplot interface:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.show()
```

Here is an example using the object-oriented interface:

```py
import numpy as np
import matplotlib.pyplot as plt

fig = plt.figure()
ax = fig.add_subplot()
# instead of the above 2 lines, we could have also used `fig, ax = plt.subplots()`

x = np.arange(6)
y = x + 1

ax.plot(x, y)

plt.show()
```

If only one argument is passed to `plt.plot`, it takes it as y axis coordinates and for x axis coordinates it takes the length of the argument provided. For example, if we run `plt.plot([1.5, 3])` matplotlib plots a line graph connecting two points (0, 1.5) and (1.0, 3.0).

We can draw multiple graphs by using the drawing method several times:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)
plt.show()
```

We can also draw multiple graphs by using a single `plt.plot` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y, x, -y)
plt.show()
```

<hr>

## Adding a title

We can add a title to the graph using the `plt.title` or `ax.set_title` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)

plt.title("Simple plot")

plt.show()
```

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.set_title("Simple plot")

plt.show()
```

<hr>

## `grid`

We can enable a grid in the visualizations using the `plt.grid(True)` or `ax.grid(True)` method.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y, x, -y)

plt.grid(True)

plt.show()
```

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y, x, -y)

ax.grid(True)

plt.show()
```

<hr>

## Adjusting the x-y Axes

### Hiding the x-y axes

We can turn off the axis with `plt.axis('off')`, `ax.axis('off')`, and `ax.set_axis_off()`.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)

plt.title("Simple plot")
plt.axis("off")

plt.show()
```

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.set_title("Simple plot")
ax.axis("off")

plt.show()
```

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.set_title("Simple plot")
ax.set_axis_off()

plt.show()
```

### Changing the x-y axes values

We can also customize the values of the axes using the `plt.axis`, and `ax.axis` methods.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)

plt.grid(True)

# the first two values are for the x-axis, the latters are for the y-axis
plt.axis([0, 2, -5, 5])
print(plt.axis())
plt.show()
```

The statement `plt.axis([0, 2, -8, 0])` sets the values of the axes. The first pair, (0, 2), refers to the limits for the x-axis, and the second pair, (-8, 0), refers to the limits for the y-axis.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.grid(True)

# the first two values are for the x-axis, the latters are for the y-axis
ax.axis([0, 2, -5, 5])
print(ax.axis())

plt.show()
```

Instead of `plt.axis`, we can also use `plt.xlim` and `plt.ylim` methods to customize the limit values for axes:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)

plt.grid(True)
plt.xlim([0, 2])
plt.ylim([-5, 5])
print(plt.axis())
plt.show()
```

In the object-oriented interface, we need to use `ax.set_xlim` and `ax.set_ylim` methods:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.grid(True)
ax.set_xlim([0, 2])
ax.set_ylim([-5, 5])
print(ax.axis())

plt.show()
```

### Adding labels to x-y axes

We can add the labels for the axes using the `plt.xlabel` and `plt.ylabel` methods:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)

plt.xlabel("x label")
plt.xlim([0, 2])

plt.ylabel("y label")
plt.ylim([-5, 5])

plt.title("Simple Plot")
plt.show()
```

In the object-oriented interface, we need to use `ax.set_xlabel` and `ax.set_ylabel` methods:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)

ax.set_xlabel("x label")
ax.set_xlim([0, 2])

ax.set_ylabel("y label")
ax.set_ylim([-5, 5])

ax.set_title("Simple Plot")
plt.show()
```

### Customizing the ticks on the x-y axes

You can customize the values on the x- and y-axes using the `plt.xticks(<tick_locations>, <tick_labels>)` and `plt.yticks` methods:

```py
import numpy as np
import matplotlib.pyplot as plt

x = y = np.arange(5)

plt.plot(x, y)

plt.xticks(range(len(x)), ['a', 'b', 'c', 'd', 'e'])
plt.yticks(range(5), [100, 101, 102, 103, 104])

plt.show()
```

In the object-oriented interface, we need to use `ax.set_xticks` and `ax.set_yticks` methods:

```py
import numpy as np
import matplotlib.pyplot as plt

x = y = np.arange(5)

fig, ax = plt.subplots()

ax.plot(x, y)

ax.set_xticks(range(len(x)), ["a", "b", "c", "d", "e"])
ax.set_yticks(range(5), [100, 101, 102, 103, 104])

plt.show()
```

<hr>

## Legend for the curves

We can use the `label` argument of the `plot` method to set the labels for curves, and use the `plt.legend` method to show the labels:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y, label="y line")
plt.plot(x, -y, label="-y line")
plt.legend()

plt.show()
```

Here is an example with the object-oriented interface:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y, label="y line")
ax.plot(x, -y, label="-y line")
ax.legend()

plt.show()
```

We can also provide a title for the legend:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y, label="y line")
plt.plot(x, -y, label="-y line")
plt.legend(title="Data:")

plt.show()
```

We can also set the labels for curves within the `plt.legend` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)
plt.legend(["y line", "-y line"])

plt.show()
```

Here is an example with the object-oriented interface:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

fig, ax = plt.subplots()

ax.plot(x, y)
ax.plot(x, -y)
plt.legend(["y line", "-y line"])

plt.show()
```

We can indicate the location of the legend using the `loc` argument of the `plt.legend` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y)
plt.plot(x, -y)
plt.legend(["y line", "-y line"], loc="lower center")

plt.show()
```

These are the values that the `loc` argument can have:

- `'best'`
- `'upper right'`
- `'upper left'`
- `'lower left'`
- `'lower right'`
- `'right'`
- `'center left'`
- `'center right'`
- `'lower center'`
- `'upper center'`
- `'center'`

<hr>

## Saving visualizations

We can save the visualization using the `plt.savefig("<name_of_pic>")` method.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(6)
y = x + 1

plt.plot(x, y, x, -y)
plt.legend(["y line", "-y line"], loc="lower center")

plt.grid(True)
plt.savefig("test.png")
plt.show()
```

<hr>

## Colors, Line Styles, and Markers

### Colors

Here are all the primary colors supported by Matplotlib:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y + 0.4, "g")
plt.plot(x, y + 0.2, "y")
plt.plot(x, y, "r")
plt.plot(x, y - 0.2, "c")
plt.plot(x, y - 0.4, "k")
plt.plot(x, y - 0.6, "m")
plt.plot(x, y - 0.8, "w")
plt.plot(x, y - 1, "b")

plt.show()
```

We can also write the previous code as follows:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(
  x, y + 0.4, "g",
  x, y + 0.2, "y",
  x, y, "r",
  x, y - 0.2, "c",
  x, y - 0.4, "k",
  x, y - 0.6, "m",
  x, y - 0.8, "w",
  x, y - 1, "b"
)

plt.show()
```

### Line style

We can customize the line style as follows:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(
  x, y, "-",
  x, y + 1, "--",
  x, y + 2, "-.",
  x, y + 3, ":"
)

plt.show()
```

### Markers

There are many options to change the markers:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y, '.')
plt.plot(x, y+0.5, ',')
plt.plot(x, y+1, 'o')
plt.plot(x, y+2, '<')
plt.plot(x, y+3, '>')
plt.plot(x, y+4, 'v')
plt.plot(x, y+5, '^')
plt.plot(x, y+6, '1')
plt.plot(x, y+7, '2')
plt.plot(x, y+8, '3')
plt.plot(x, y+9, '4')
plt.plot(x, y+10, 's')
plt.plot(x, y+11, 'p')
plt.plot(x, y+12, '*')
plt.plot(x, y+13, 'h')
plt.plot(x, y+14, 'H')
plt.plot(x, y+15, '+')
plt.plot(x, y+16, 'D')
plt.plot(x, y+17, 'd')
plt.plot(x, y+18, '|')
plt.plot(x, y+19, '_')

plt.show()
```

### Combining colors, markers, and line styles

We can combine the setting of colors, markers and line styles as below:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y, "mo--")
plt.plot(x, y + 1, "g*-.")

plt.show()
```

We can also customize everything explcitily and in great detail using the below arguments:

- `color`,
- `linestyle`,
- `linewidth`,
- `marker`,
- `markerfacecolor`,
- `markeredgecolor`,
- `markeredgewith`,
- `markersize`

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(
  x, y,

  color="g",

  linestyle="--",
  linewidth=2.5,

  marker="^",
  markerfacecolor="r",
  markeredgecolor="b",
  markeredgewidth = 2.5,
  markersize = 10
)

plt.show()
```

<hr>

### More Styling

#### `plt.tight_layout()`

Sometimes subplots on the same figure might overlap. We can manually adjust this or we can use the `plt.tight_layout` method to let matplotlib to try to automatically adjust the figure.

```py
import matplotlib.pyplot as plt

# Create a figure with two subplots
fig, (ax1, ax2) = plt.subplots(1, 2)

# Plot data on the first subplot
ax1.plot([1, 2, 3], [4, 5, 6])
ax1.set_title("Subplot 1")
ax1.set_xlabel("X-axis")
ax1.set_ylabel("Y-axis")

# Plot data on the second subplot
ax2.plot([1, 2, 3], [6, 5, 4])
ax2.set_title("Subplot 2")
ax2.set_xlabel("X-axis")
ax2.set_ylabel("Y-axis")

# Adjust layout to prevent overlap
plt.tight_layout()

# Display the figure
plt.show()
```

#### `tick_params()`

The `tick_params` method in Matplotlib is used to customize the appearance of the x-axis and y-axis tick labels, tick locations, and grid lines. There are many parameters that could be adjusted using this method.

Here is a basic example, where we rotate the tick labels for the y axis:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y)
plt.plot(x, -y)

plt.tick_params(axis="y", rotation=315)

plt.show()
```

#### `hlines()` and `vlines()`

The `hlines` method plots horizontal lines at each y from `xmin` to `xmax`.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y)

# Use vlines to create vertical lines at x-coordinates 0 and 5
plt.vlines([1, 3], ymin=0, ymax=5, colors='r', linestyles='dashed')

plt.show()
```

The `vlines` method plots vertical lines at each x from `ymin` to `ymax`.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(5)
y = x

plt.plot(x, y)

# Use vlines to create vertical lines at x-coordinates 0 and 5
plt.hlines([1, 3], xmin=0, xmax=5, colors='r', linestyles='dashed')

plt.show()
```

<hr>

## Subplots

You can show multiple separate graphs in the same output. This technique is known as subplotting. Subplots can have their own titles, own labels, and other specifications.

The first two arguments passed to `plt.subplot()` represent the grid size, and the third argument indicates the position of that particular subplot.

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(3)

plt.subplot(2, 2, 1)
plt.plot(x, x**2)

plt.subplot(2, 2, 2)
plt.plot(x, x**3)

plt.subplot(2, 2, 3)
plt.plot(x, x**4)

plt.show()
```

We can also use the `plt.subplots` method to have several subplots on the same figure:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(3)

fig, axs = plt.subplots(2, 2)

axs[0, 0].plot(x, x**2)
axs[0, 1].plot(x, x**3)
axs[1, 0].plot(x, x**4)

plt.show()
```

We can also add subplots to a figure this way:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(3)

fig = plt.figure()

ax0 = fig.add_subplot(2, 2, 1)
ax0.plot(x, x**2)

ax1 = fig.add_subplot(2, 2, 2)
ax1.plot(x, x**3)

ax2 = fig.add_subplot(2, 2, 3)
ax2.plot(x, x**4)

plt.show()
```

<hr>

## Styles

There are many built-in styles in Matplotlib. A style dictates things such as marker size, colors, and fonts.

Here is an example of applying built-in `ggplot` style:

```py
import numpy as np
import matplotlib.pyplot as plt

plt.style.use("ggplot")
data = np.random.randn(10)

plt.plot(data)
plt.show()
```

To see all the available styles, we can use `plt.style.available`.

Here is an example of reverting the style back to the default one:

```py
import numpy as np
import matplotlib.pyplot as plt

plt.style.use("default")
data = np.random.randn(10)

plt.plot(data)
plt.show()
```

Styling is applied globally to the entire notebook. But we can locally change the styling for a block of code using the `with plt.style.context()` statement:

```py
import numpy as np
import matplotlib.pyplot as plt

with plt.style.context("Solarize_Light2"):
  data = np.random.randn(10)

  plt.plot(data)
  plt.show()
```

<hr>

## Bar Graphs

A bar graph is a representation of discrete and categorical data items with bars. You can represent the data with vertical or horizontal bars. For vertical bars, we need to use the `bar` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(4)
y = x + 1

plt.bar(x, height=y)
plt.show()
```

We can combine bar graphs as follows:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(4)
y = x + 1

plt.bar(x, height=y, color="b", width=0.25)
plt.bar(x + 1/2, height=y + 5, color="g", width=0.25)

plt.show()
```

We can have horizontal bar graphs using the `plt.barh` method:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(4)
y = x + 1

plt.barh(y, width=x, color="b", height=0.25)

plt.show()
```

We can also combine horizontal bar graphs as follow:

```py
import numpy as np
import matplotlib.pyplot as plt

x = np.arange(4)
y = x + 1

plt.barh(y, width=x, color="b", height=0.25)
plt.barh(y + 1/2, width=x + 5, color="g", height=0.25)

plt.show()
```

<hr>

## Scatter Plot

We can usually visualize a set of two variables with a scatter plot. One variable is assigned to the x-axis, and another is assigned to the y-axis.

```py
import numpy as np
import matplotlib.pyplot as plt

N = 1000
x = np.random.rand(N)
y = np.random.rand(N)

size = 10

plt.scatter(x, y, s=size, alpha=0.5)
plt.show()
```

We can also set the size of the x-y pairs to be variable:

```py
import numpy as np
import matplotlib.pyplot as plt

N = 1000
x = np.random.rand(N)
y = np.random.rand(N)

size = (50 * x * y)

plt.scatter(x, y, s=size, alpha=0.5)
plt.show()
```

<hr>

## Histogram

We can create a histogram using the `plt.hist` method. To use this method, we will need data, and number of buckets to divide the data into. In the below example, we have numbers as data and we define buckets equal to the cardinality (number of distinct elements) of the set.

```py
import numpy as np
import matplotlib.pyplot as plt

x = [1, 3, 5, 1, 2, 4, 4, 2, 5, 4, 3, 1, 2]

plt.hist(x, bins=5, width=0.5)

plt.show()
```

<hr>

## Pie Chart

We can draw a pie chart using the `plt.pie` method:

```py
import numpy as np
import matplotlib.pyplot as plt

data = np.array([35, 25, 25, 15])

plt.pie(data)
plt.show()
```

We can add labels to the pie chart using the `labels` argument:

```py
import numpy as np
import matplotlib.pyplot as plt

data = np.array([35, 25, 25, 15])

plt.pie(data, labels=["A", "B", "C", "D"])

plt.legend(title="Data:")
plt.show()
```

We can separate the parts of the pie a bit using the `explode` argument:

```py
import numpy as np
import matplotlib.pyplot as plt

data = np.array([35, 25, 25, 15])

plt.pie(
  data,
  labels=["A", "B", "C", "D"],
  explode=[0.0, 0.1, 0.15, 0.25]
)

plt.legend(title="Data:")
plt.show()
```

We can also enable the shadows:

```py
import numpy as np
import matplotlib.pyplot as plt

data = np.array([35, 25, 25, 15])

plt.pie(
  data,
  labels=["A", "B", "C", "D"],
  explode=[0.0, 0.05, 0.15, 0.25],
  shadow=True
)

plt.legend(title="Data:")
plt.show()
```

<hr>

## Matplotlib Defaults

Matplotlib uses the **matplotlibrc** file to store default values for various environment and figure parameters used across matplotlib functionality. Hence, this file is very long. These default values are customizable to apply for all the plots within a session.

The following code block provides the path to the file containing all configuration parameters:

```py
import matplotlib as mpl

mpl.matplotlib_fname()
```

You can use the `print(matplotlib.rcParams)` command to get all the default parameter settings from this file.

The `matplotlib.rcParams` command is used to change these default values to any other supported values, one parameter at a time.

The `matplotlib.rcdefaults()` command is used to restore default parameters.

<hr>

## Interactive vs Non-Interactive mode

Matplotlib can be used in an interactive or non-interactive modes. In the interactive mode, the graph display gets updated after each statement. In the non-interactive mode, the graph does not get displayed until explicitly asked to do so.

The latest versions of Jupyter Notebook seem to display the figure without calling `plt.show()` command explicitly. However, in Python shell or embedded applications, `plt.show()` or `plt.draw()` is required to display the figure on the screen.

Using the following commands, interactive mode can be set on or off, and also checked for current mode at any point in time:

- `matplotlib.pyplot.ion()` to set the interactive mode ON
- `matplotlib.pyplot.ioff()` to switch OFF the interactive mode
- `matplotlib.is_interactive()` to check whether the interactive mode is ON (True) or OFF (False)

<hr>
