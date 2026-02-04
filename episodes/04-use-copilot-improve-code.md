---
title: "How to use Copilot to Improve an Existing Codebase"
teaching: 30
exercises: 30
---

:::::::::::::::::::::::::::::::::::::: questions 

- How can Copilot help me improve my code?
- What can I do to improve my Copilot prompts?
- How can I reuse previous Copilot prompts?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Describe how to use prompt engineering to obtain valuable responses from Copilot
- Use Copilot to improve a segment of code
- Use Copilot to diagnose a failing unit test and fix it
- Use Copilot to add a new, small feature to an existing codebase
- Describe how to use Copilot to assist with Git commits and managing Git history
- Create and use a reusable prompt context
- Create an VSCode Agent Skill to define a specialised and reusable domain-specific task
- Describe key limitations and known issues of using LLM coding assistants within an IDE

::::::::::::::::::::::::::::::::::::::::::::::::

In the previous episode we looked at Copilot giving us guidance and advice to improve our code.
In this episode, we'll look at how Copilot can assist with code modifications more directly,
looking at the following features:

- `Inline suggestions` - where Copilot provides coding suggestions as you type
- `Edit mode` - where Copilot provides broad suggestions (like `Ask` mode) but will also enact them step-by-step directly on our approval
- `Agent mode` - where Copilot directly autonomously undertakes large scale changes, with a single option to approve the changes at the end of the process

These are in ascending order of the autonomy, authority and scope that we delegate to Copilot to make changes.
As we'll see, as we delegate more authority and scope,
the more we should increase our skepticism and diligence in reviewing and understanding suggestions made by such tools.

## Inline Suggestions

Within VSCode, Copilot can provide "inline" suggestions as we type,
which go much further than typical IDE autocomplete suggestions.

If we select the Copilot icon again in the status bar,
we can see the current inline settings:

![](fig/copilot-inline-settings.png)

So here, we can see that inline settings will apply to all Python files.
These suggestions will appear as "ghosted text" suggestion which you can autocomplete with tab,
very similar to how they appear with standard VSCode autocomplete.
There are also `Next edit suggestions` which go beyond the immediate context to make suggestions in other places in your code.
These predict the location and the content of the next edit you'll want to make.

Let's say we want to add a new section describing our coding style.
Directly under `Code Patterns & Conventions`, add:

```markdown
### Coding Style
- 
```

You should see a suggestion appear direcly after your cursor,
something like `Use PEP 8 style guidelines for Python code` or similar.
You can accept this suggestion by pressing `Tab`.
If you continue to add new lines after this, you may find it continues to suggest other things to include,
so we end up with, for example:

```markdown
### Coding Style
- Use PEP 8 style guidelines for Python code
- Use descriptive variable names (e.g., `inflammation_data`, `mean_inflammation`)
- Comment code sections for clarity, especially data processing steps
```

It does this by rapidly incorporating contextual information from a number of sources to infer a suggestion,
including:

- The code file you are editing
- Any code you have currently selected
- Frameworks, languages and dependencies
- Any instructions file

Copilot suggestions are a starting point, but we should alwyays review, understand, and amend as necessary,
as opposed to blindly accepting suggestions.

:::::::::::::::::::::::::::::::::::::: challenge

## Amend the Suggestions

3 mins.

Read through and understand the Copilot suggestions for the Coding Style section of the `copilot-instructions.md` file,
and add/amend as you see fit, perhaps to fit your coding style.

:::::::::::::::::::::::::: solution

For example:

```markdown
### Coding Style
- Follow PEP 8 style guidelines for Python code
- Use descriptive variable names (e.g., `inflammation_data`, `mean_inflammation`)
- Comment code sections for clarity, especially to explain purpose and logic, and to describe data processing steps
```

As we'll see shortly, these coding style guidelines will inform future refactoring of our code.

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::: challenge

## Inline Suggestions for Code

3 mins.

In `inflammation-plot.py`, begin adding an additional parameter to the functions calls in the code,
by placing an additional `, ` at the end of the given list of parameters, and see which inline suggestions are being made, e.g.:

```python
    data = np.loadtxt(fname=filename, delimiter=',', ... )
```

Try this for the following function calls:
- `np.loadtxt`
- `plt.figure`
- Each of the `.plot` calls to the `axes` variables

1. What's being suggested?
1. Are the suggestions helpful? Are they always the same?
1. Does the code still run?

:::::::::::::::::::::::::: solution

1. For example:

```python
    data = np.loadtxt(fname=filename, delimiter=',', dtype=np.float64)
```

```python
    fig = plt.figure(figsize=(10.0, 3.0), dpi=100)
```

For `.plot`, if the first suggestion of a `color` parameter is selected,
it suggests variants of that for the other calls to `plot`, e.g.

```python
    axes1.plot(data.mean(axis=0), color='blue')
    axes2.plot(data.mean(axis=0), color='red')
    axes3.plot(data.mean(axis=0), color='green')
```

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

## Edit Mode

Edit mode differs from inline suggestions by offering the ability to enact changes step-by-step directly on your approval.
Unlike inline suggestions which appear as you type, Edit mode allows you to request broader changes across multiple lines or functions,
so it's ideal for repetive things like small-scale refactoring of code logic or renaming variables.
It represents a middle ground in terms of autonomy — more direct than inline suggestions but less autonomous than Agent mode.

To get started with using edit mode in VSCode, you highlight the code you want to modify before requesting the change you want.

For example:

1. Select `Edit` as the Copilot mode in the chat window
1. Select the entire for loop in `inflammation-plot.py`; you'll notice that the context now includes this file with the selected line numbers
1. Enter `Add a comment about this code above this line`
1. Press `Enter`

You'll now see a comment added above the loop highlighted in green, with a `Keep` or `Undo` pop-up displayed at the bottom.
Read through the comment, and if you agree that the comment summarises the code sufficiently, select `Keep`.

::::::::::::::::::::::::::::::::: callout

## The Temptation to Blindly Accept!

So note that here, we properly scrutinise the suggestion as opposed to accepting it blindly!
it would be all too easy to just assume it's correct and just accept it,
but it's helpful to remember that tools like Copilot are like a more helpful autocomplete,
not a thinking teammate.
As such, skepticism and review must become a key practice when using such tools.

:::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::: challenge

## A Small Refactor!

3 mins.

Let's assume we want to rename the `axes` variables to have the prefix `subplot` instead,
e.g. instead of `axes1`, we'd have `subplot1`.

Use Copilot edit mode to rename all of these variables,
then verify and accept the changes if you agree with them.
Does the code still run correctly?

:::::::::::::::::::::::::: solution

1. Select the code from the first `axes1` definition to the last use of `subplot3`, e.g.

   ```python
       axes1 = fig.add_subplot(1, 3, 1)
       axes2 = fig.add_subplot(1, 3, 2)
       axes3 = fig.add_subplot(1, 3, 3)

       axes1.set_ylabel('average')
       axes1.plot(data.mean(axis=0))

       axes2.set_ylabel('max')
       axes2.plot(data.max(axis=0))

       axes3.set_ylabel('min')
       axes3.plot(data.min(axis=0))
   ```

1. Ensure `Edit` mode is selected in chat, and enter `Rename axes variables to have a subplot prefix instead`
1. Press `Enter` and you should see something like the following:
   ![](fig/copilot-rename-variables.png)
1. After reviewing the suggestions, select `Keep`

The code still runs correctly.

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::


## Agent Mode

Agent mode differs from edit mode by being outcome driven rather than per-edit driven.
So instead of asking Copilot to change a specific piece of code, you give it a goal, such as adding a feature, or refactoring a module.
Copilot then plans how to achieve that goal and works across the repository to do so.
It may read and modify multiple files, add or update tests, adjust configuration, and iterate over several steps before presenting the result.
In this way, agent mode behaves more like a junior developer taking on a task, rather than a pair programmer responding line-by-line.
Note the increase in authority to modify code, which represents a much greater risk:
the impact of changes is greater and requires more careful (and potentially more involved) review.

In the last episode we looked at the suggestions made by two Copilot models to improve the code.
Let's now use agent mode to do this as a refactoring activity,
directly amending our code with the goal to improve its readability.

1. Set Copilot's mode to `Agent`
1. Select a model you'd like to use
1. Enter the following prompt: `Refactor this code to improve it's modularity and readability`
1. Press `Enter`
1. Once suggested improvements appear, review the changes and either:
   - If you agree with them and think they are an improvement on the original code, `Keep` the suggestions
   - If you don't agree with them, cancel the suggestions and select the circular `Retry` icon at the bottom left of the chat response until you get something more acceptable

One example of output:

```python
"""
Analyze and visualize inflammation data.

This script reads inflammation CSV files from data/, computes statistics
(mean, max, min) across patients for each day, and saves corresponding
plots as PNG files.
"""

import glob
from pathlib import Path

import numpy as np
from matplotlib import pyplot as plt


def load_inflammation_data(filename: str) -> np.ndarray:
    """Load inflammation data from a CSV file.
    
    Args:
        filename: Path to the CSV file containing inflammation data.
        
    Returns:
        A 2D numpy array where rows are patients and columns are days.
    """
    return np.loadtxt(fname=filename, delimiter=',')


def create_statistics_plot(data: np.ndarray, figure_size: tuple = (10.0, 3.0)) -> plt.Figure:
    """Create a figure with three subplots showing mean, max, and min values.
    
    Args:
        data: A 2D numpy array of inflammation values.
        figure_size: Tuple of (width, height) for the figure in inches.
        
    Returns:
        The matplotlib Figure object containing the three subplots.
    """
    fig = plt.figure(figsize=figure_size)
    
    # Create subplots for mean, max, and min statistics
    subplots = [fig.add_subplot(1, 3, i) for i in range(1, 4)]
    
    statistics = [
        ('average', data.mean(axis=0)),
        ('max', data.max(axis=0)),
        ('min', data.min(axis=0)),
    ]
    
    for subplot, (label, values) in zip(subplots, statistics):
        subplot.set_ylabel(label)
        subplot.plot(values)
    
    fig.tight_layout()
    return fig


def process_inflammation_file(filename: str) -> None:
    """Load inflammation data and save a statistics plot.
    
    Args:
        filename: Path to the CSV file to process.
    """
    print(filename)
    data = load_inflammation_data(filename)
    fig = create_statistics_plot(data)
    output_filename = f"{filename}.png"
    fig.savefig(output_filename)
    plt.close(fig)


def main() -> None:
    """Load and process all inflammation data files."""
    filenames = sorted(glob.glob('data/inflammation-*.csv'))
    for filename in filenames:
        process_inflammation_file(filename)


if __name__ == '__main__':
    main()
```

So in this instance, we can see that there are a whole swathe of changes:

- Modularised the codebase by refactoring into four functions: use of a main function called from the top-level script, for loading a CSV file, generating a plot for a set of data, and processing a particular inflammation file
- Docstrings have been added for each of the functions and the module
- The processing of the average, maximum and minimum values has been refactored into a loop iterating over a data structure
- The subplots are generated within a loop using `zip()` to provide corresponding pairs of array elements into the loop.
If we didn't like this particular style, we might use Edit mode on this segment to simpligy it

Note that it differs substantially from the version shown from a similar question made in Ask mode earlier,
and whilst it is more modular,
it's now 127 lines of code where before it was 32 lines - 
we might consider this to be quite an over-engineered overkill for what 


::::::::::::::::::::::::::::::::: callout

## What if I don't get a Good Response?

If you aren't getting a good response despite retrying several times,
it suggests that the prompt is either not reflective of what you're really after or isn't specific enough,
so try amending the request.

:::::::::::::::::::::::::::::::::::::::::

Now once the suggestions have been integrated,
we're still able to undo these changes, e.g. either by selecting `Edit` and `Undo` from the VSCode menu,
or pressing `Ctrl + Z` (or `Cmd/Windows Key + Z`).
We're also able to edit a previous prompt in the chat window,
perhaps adding more specifics for what we want.

:::::::::::::::::::::::::::::::::::::: challenge

## A Large Refactor!

5 mins.

Perhaps the suggestions look generally quite good,
but maybe it isn't quite what we're after.

Edit the agent mode prompt in the chat (using a model of your choice) to include more specificity in the request, for example:

- `Refactor this code to improve its modularity and readability, using the reST format for docstrings`
- `Refactor this code to improve its modularity and readability, and easy to understand for a novice coder`

You might want to `Retry` or amend your prompt a few times to obtain something you find acceptable!

:::::::::::::::::::::::::: solution

For the second prompt above, for example (note the increased level of commenting, more descriptive docstrings,
and less complex coding patterns for data processing and generating the plots):

```python
"""
Inflammation Data Visualization Script

This script processes inflammation data from CSV files and creates visualizations
showing average, maximum, and minimum inflammation levels across patients for each day.
"""

import glob
import numpy as np
from matplotlib import pyplot as plt


def load_inflammation_data(filepath):
    """
    Load inflammation data from a CSV file.

    Args:
        filepath (str): Path to the CSV file to load

    Returns:
        np.ndarray: 2D array of inflammation data (rows=patients, columns=days)
    """
    data = np.loadtxt(fname=filepath, delimiter=',')
    return data


def create_statistics_figure(data):
    """
    Create a figure with three subplots showing statistics of inflammation data.

    Args:
        data (np.ndarray): 2D array of inflammation data

    Returns:
        tuple: (figure object, list of subplot axes)
    """
    # Create figure with three subplots side by side
    fig = plt.figure(figsize=(10.0, 3.0))

    # Create three subplots for average, max, and min
    subplots = [
        fig.add_subplot(1, 3, 1),
        fig.add_subplot(1, 3, 2),
        fig.add_subplot(1, 3, 3),
    ]

    return fig, subplots


def plot_statistics(subplots, data):
    """
    Plot average, maximum, and minimum statistics on the given subplots.

    Args:
        subplots (list): List of matplotlib axes objects
        data (np.ndarray): 2D array of inflammation data
    """
    # Calculate statistics across all patients (axis=0)
    average = data.mean(axis=0)
    maximum = data.max(axis=0)
    minimum = data.min(axis=0)

    # Plot average on first subplot
    subplots[0].set_ylabel('average')
    subplots[0].plot(average)

    # Plot maximum on second subplot
    subplots[1].set_ylabel('max')
    subplots[1].plot(maximum)

    # Plot minimum on third subplot
    subplots[2].set_ylabel('min')
    subplots[2].plot(minimum)


def save_figure(figure, filename):
    """
    Save a matplotlib figure as a PNG file.

    Args:
        figure: matplotlib figure object
        filename (str): Output filename (will add .png extension)
    """
    output_filename = filename + '.png'
    figure.savefig(output_filename)
    print(f"Saved visualization: {output_filename}")


def process_inflammation_files(data_directory='data/inflammation-*.csv'):
    """
    Process all inflammation CSV files and create visualizations.

    This function:
    1. Finds all CSV files matching the pattern
    2. Loads data from each file
    3. Creates a figure with statistical plots
    4. Saves the figure as a PNG file

    Args:
        data_directory (str): Glob pattern for finding CSV files
    """
    # Find all inflammation CSV files
    filenames = sorted(glob.glob(data_directory))

    # Process each file
    for filename in filenames:
        print(f"Processing: {filename}")

        # Load data from file
        data = load_inflammation_data(filename)

        # Create figure with subplots
        fig, subplots = create_statistics_figure(data)

        # Plot statistics on the subplots
        plot_statistics(subplots, data)

        # Arrange subplots so labels don't overlap
        fig.tight_layout()

        # Save figure as PNG file
        save_figure(fig, filename)


# Main execution
if __name__ == '__main__':
    process_inflammation_files()
```

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::: instructor

## Quick Check: Ask the Audience

Following the exercise, paste the following comment into the chat, and ask for thumbs up/down as a reaction to it,
and summarise the responses:

Were you able to achieve something that you regard as well written code you can understand?

:::::::::::::::::::::::::::::::::::::


::::::::::::::::::::::::::::::::::::: keypoints 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::
