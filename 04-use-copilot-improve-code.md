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

::::::::::::::::::::::::::::::::::::: keypoints 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::
