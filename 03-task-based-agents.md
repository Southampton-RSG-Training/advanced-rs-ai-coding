---
title: "Task-based Approach to Software Development"
teaching: 0
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use the built-in planning agent to develop a basic Python application
- Create reusable Copilot skills for specific development tasks
- Describe the format and basic configuration of a skills definition file
- Describe the purpose of a unit test
- Write and run a unit test to run within the pytest unit testing framework
- Interpret the output of running pytest

::::::::::::::::::::::::::::::::::::::::::::::::

FIXME: intro

## Using the Built-in Plan Agent

### Creating a Plan

Instead of "one-shotting" the development of code using an AI coding tool, 
we've seen that a better approach is to plan and implement our code in a step-wise, 
incremental fashion.
So how should we go about this?

One way would be to use the built-in VSCode plan agent that helps developers break down tasks into clear and actionable steps before writing code.
Instead of jumping straight into implementation, it researches the requested task, analyses any existing project code in read-only mode, and generates structured step-based plans for features or for other code modification activities.
In short, it aims to guide users through a thoughtful planning phase prior to coding to reduce errors and encourage better design and implementation decisions.

Let's try it out now.

1. Select `Plan` from the Copilot mode dropdown in the chat panel.
1. Select `GPT-5.4 mini` selected in the model dropdown:

   `Create a command line tool written in Python that reads in a single CSV data file contained in the data directory passed as an argument, and creates graphical plots saved as PNG images to visualise the mean, minimum, maximum and standard deviation across each column. The tool should use Numpy for statistical analysis and Matplotlib for generating the plots`

1. Press `Enter`
1. Answer any clarifying questions from the planning agent.
1. Observe the step-by-step thinking and actions undertaken by the agent.
1. When the planning agent concludes, select the option to open the plan in the editor, then save it.

You should find you end up with something similar to this, saved as a prompt file in the project folder:

```markdown
## Plan: CSV Statistics Plotter

Build a small Python CLI that takes a directory argument, finds the single CSV inside it, loads the data as a NumPy array, computes column-wise mean, minimum, maximum, and standard deviation, and writes PNG plots with Matplotlib.
The sample file at [data/inflammation-01.csv](/Users/steve/Test/ai-plan-test/data/inflammation-01.csv) has 40 numeric columns and no header row, so the tool should synthesize column labels instead of expecting names in the file.

**Steps**
1. Set up a minimal Python project scaffold with a CLI entry point and dependencies for NumPy and Matplotlib.
2. Implement input discovery and validation so the tool accepts a directory path, confirms there is exactly one CSV file, and reads it into a numeric array.
3. Compute the four requested statistics per column with NumPy, keeping the output shape consistent for plotting.
4. Generate PNG visualizations with Matplotlib, using synthetic column labels like `1` through `40`, and write the images to a separate `output` directory.
5. Add argument handling for input and output locations plus clear error messages for missing files, multiple CSVs, or non-numeric data.
6. Validate the end-to-end flow against the provided sample dataset and confirm the PNGs are created successfully.

**Relevant files**
- `pyproject.toml` for project metadata, dependencies, and the console script.
- A new CLI module under `src/` for loading the CSV, computing stats, and generating plots.
- `README.md` for usage notes and output behavior.

**Verification**
1. Run the CLI against the sample data directory and confirm PNG files are generated without errors.
2. Inspect the images to verify the four statistics are plotted sensibly across all 40 columns.
3. Do not create any unit tests.

**Decisions**
- Assume the directory contains exactly one CSV file.
- Assume the CSV has no header row.
- Prefer separate plots per statistic unless a combined figure is clearer and still readable.
- Keep the scope focused on a single-file workflow rather than batch processing.
```

This now provides us with a planning document which we are able to review and amend as we wish.
This is very reasonable approach:

- Importantly, we are now moving from **ad-hoc** development to **intentional** development,
which forces us to consider ways forward and make decisions and capture these within a defined plan that we validate and refine before moving to implementation.
- We may extend this plan with other sections and further detail as needed.
- It has created a concrete document we can discuss and refine with colleagues before we proceed.
- It also provides a "checkpoint": if the implementation is unsatisfactory we can remove the implementation, amend the plan, and ask Copilot to create the implementation again.

FIXME: expand on the benefits of externalising the agentic reasoning a bit


:::::::::::::::::::::::::::::::::::::: challenge

## Review!

5 mins.

As we know, we should always review the output from generative AI.
So with a skeptical mindset:

- Carefully review the generated plan and ensure it complies with the initial prompt.
- Does the plan match what you want from this tool?
- Refine the plan as needed.
- If you spot any references to creating unit tests, remove them for now - we'll cover this later!
- Add a new line to the `Verification` section: `Do not create any unit tests.`

When you've finished, add your thoughts about how well the built-in planning agent performed this task into the shared document,
noting what it did well and what it could have done better.

::::::::::::::::::::::::::::::::::::::::::::::::


### Enacting the Plan

Once we're happy with our plan, we may proceed with implementation either by selecting the 'Start implementation' option in the chat if it appears,
or by entering something like the following in the chat with the `Agent` mode enabled:

```
Implement the plan
```

FIXME: complete


## Using Skills for On-demand Coding Tasks

FIXME: change these to skills and use them within the implementer agent in the next episode?
FIXME: workflow then becomes: develop/use skills, build agents that use skills, plus diagram for that

With the aid of Copilot's planning agent we've created an initial plan, reviewed and refined it, and had Copilot create an initial implementation for us based on the plan.
However, there are other tasks typically conducted during development,
and Copilot (and other similar generative AI tools and infrastructure such as Claude) allows us to encapsulate these tasks by creating our own custom *skills*.
By defining common development tasks as skills, when it comes to implementation, we can call on these skills to lighten the load.

FIXME: diagram showing developer agency vs autonomy: focus is on a person at the center driving development, reviewing suggestions and making decisions. AI is just another tool

Let's create some skills using this approach to help us with some development tasks.

First, from within our coding directory, create a new directory `.github/skills` to hold our skills, e.g.

```bash
mkdir .github/skills
```

VSCode also has a useful extension that's useful for identifying issues with skills that we develop, so let's install that now:

1. Select the `Extensions` icon on the sidebar.
1. Add `@id:ms-vscode.vscode-chat-customizations-evaluations` into the search box, which is a specific reference for the extension we want.
1. Select `Install`

This extension - which actually acts as a Copilot skill - helps us find contradictions in skill prompt logic as well as other ambiguities and issues.
This extension is also useful for identifying issues with other Copilot AI prompt files, such as `.instructions.md`, `.prompt.md` and `.agent.md` files.

### A Code Linter Skill

Code linters are automated tools that analyse source code to identify potential errors, stylistic inconsistencies, and violations of coding standards before the software is run.
They help developers maintain cleaner and more readable code by highlighting issues such as unused variables, formatting problems, syntax mistakes, and poor programming practices.
Linters are commonly integrated into Integrated Development Environments (IDEs), and in the case of VSCode, additional linting tools can be installed as extensions, such as [Pylint](https://marketplace.visualstudio.com/items?itemName=ms-python.pylint) or [flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8).

Let's define a skill that uses Pylint to identify code issues, provide a summary and plan of action, and fix these issues when the plan is approved.

In VSCode, agents are typically defined in a file with a `.agent.md` suffix.
Create a new `code-linter.agent.md` file in the `.github/agents` directory, and add the following contents:

~~~markdown
---
name: pylint-fix
description: Identify and fix code styling issues using the Pylint code linter.
compatibility: Requires python3
version: 1.0.0
---

This skill automatically fixes code issues identified by pylint.

## Constraints

- ONLY make modifications based on output from pylint
- ONLY make modifications to files in the `src/` directory

## Approach

1. Run pylint and capture the output from running the command.
2. Analyse pylint output to identify warnings.
3. Apply fixes only with 100% confidence.
4. Re-run pylint to verify fixes.
5. Provide a summary of what was fixed.

## Commands to Use

- Ensure the virtual environment is activated before running pylint

```bash
python -m pylint src/
```
~~~


#### Improving our Skill

As we have it, the definition may seem reasonable, but let's check it over.

:::::::::::::::::::::::::::::::::::::: challenge

## Class Exercise: So What's Wrong with It?

5 mins.

What do you think could be improved? Is there anything missing?

Add your thoughts to the shared document.

::::::::::::::::::::::::::::::::::::::::::::::::

One way to help us identify any issues is to use the `Chat Customizations Evaluations` extension we installed earlier:

1. Select `Analyze` that should have appeared at the bottom of the `SKILL.md` file editor pane.
1. A pop-up box will request permission to conduct the analysis. Select `Allow` (or similar).
1. When the analysis is complete, select `Show problems` when the pop-up appears, which will display a list of issues in the `PROBLEMS` tab in the bottom VSCode pane.

You should see a whole host of issues that indicate problems with ambiguity, cognitive load, coverage gaps, amongst others.

Within the file code editor, you should see these issues highlighted with underlines.
If you hover over either the entries in the `PROBLEMS` pane, or the editor underlines,
a description will pop up explaining the issue.

:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Fix our `SKILL.md`

10 mins.

Go through each of the identified issues and fix them.

One way to do this such that you get an opportunity to review the suggestions before they are applied,
is to first right-click on the entry in the `PROBLEMS` pane and select `Explain` which drafts a set of proposed changes.

The Chat Customisations Evaluations tool can be a bit overzealous,
so if you think a suggestion is overkill, feel free to ignore it.

If you're happy with the change, select the `Apply to...` icon at the top right of the suggestion to apply it to the agent file,
then select `Keep` in the editor window.
Otherwise, `Undo` the change and add your own fix.

Note: you may find that even once the identified problems are fixed, issues in the `PROBLEMS` pane still remain.
Restarting VSCode fixes this.

:::::::::::::::::::::::::: solution

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::: callout

## When You Have a Hammer...

...everything looks like a nail.

We've created a generative AI skill for this which seems useful.
But using AI tools come at a cost - typically, they take a while to run and are prone to error.
What we should also consider is whether a more dedicated tool would be a better fit.
For example, would simply using a code linting tool such as [ruff]() be enough?
If we wanted automatic code formatting, would [black]() be sufficient?

It's a trade-off we should consider.
AI-based solutions often offer powerful, broader sets of capabilities,
but dedicated tools run more efficiently and with a greater degree of precision.

:::::::::::::::::::::::::::::::::::::::::


### A Documentation Skill

FIXME: get learners to create the agent themselves from a brief spec (inc. use of mkdocs), and then use chat customisations evaluations extension to remedy and fix issues


### A Unit Test Builder Skill

FIXME: use built-in test agent 



::::::::::::::::::::::::::::::::::::: keypoints 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::
