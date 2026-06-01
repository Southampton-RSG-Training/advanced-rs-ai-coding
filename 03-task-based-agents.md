---
title: "Planning and Task-based Approach to Software Development"
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
1. Select `GPT-5.4 mini` selected in the model dropdown
1. Enter the following into the chat:

   `Create a command line tool written in Python that reads in a single CSV data file contained in the data directory passed as an argument, and creates graphical plots saved as PNG images to visualise the mean, minimum, maximum and standard deviation across each column. The tool must use Numpy for statistical analysis and Matplotlib for generating the plots. Create a virtual environment for managing dependencies with pip, and a requirements.txt file to hold the dependencies.`

1. Press `Enter`.
1. Answer any clarifying questions from the planning agent.
1. Observe the step-by-step thinking and actions undertaken by the agent.
1. When the planning agent concludes, select the option to `Open in Editor`, and `Keep`.
1. Save the file that appears by selecting `Save as prompt file` which should appear in the bottom right of the editing window.

You should find you end up with something similar to this, saved as a prompt file (either in the repository root or in the `.github/prompts` directory),
although the content will likely differ:

```markdown
## Plan: CSV statistics plot CLI

Build a small Python CLI that reads a single CSV file from a `data` directory argument, computes mean, min, max, and standard deviation with NumPy, and writes four PNG plots with Matplotlib. The workspace is currently just a clean data folder, so the implementation should scaffold a minimal Python project around a focused command-line entrypoint and dependency management via `venv` plus `requirements.txt`.

**Steps**
1. Create the project scaffold and dependency files, including a virtual-environment workflow and `requirements.txt` entries for NumPy and Matplotlib.
2. Implement the CLI entrypoint that accepts a path to the `data` directory, validates that exactly one CSV file is provided or selected, and routes that file into the analysis pipeline.
3. Implement the analysis layer with NumPy to load the CSV and compute column-wise mean, min, max, and standard deviation.
4. Implement the plotting layer with Matplotlib to generate four separate PNG files, one per statistic, saved alongside the input data or in a predictable output directory.
5. Add basic usage documentation describing how to create the virtual environment, install dependencies, and run the tool.
6. Validate the workflow with a smoke test on one of the existing inflammation CSV files and confirm the PNG outputs are created.

**Relevant files**
- `/Users/steve/Test/advanced-ai-coding-example/requirements.txt` — pinned runtime dependencies for NumPy and Matplotlib.
- `/Users/steve/Test/advanced-ai-coding-example/.gitignore` — likely already covers `venv/` and `__pycache__/`; confirm it is sufficient for the new project files.
- `/Users/steve/Test/advanced-ai-coding-example/<new CLI module>` — command-line parsing, CSV loading, and orchestration.
- `/Users/steve/Test/advanced-ai-coding-example/<new plotting module>` — Matplotlib figure generation and PNG output.
- `/Users/steve/Test/advanced-ai-coding-example/README.md` — setup and usage instructions.

**Verification**
1. Create a virtual environment and install dependencies from `requirements.txt`.
2. Run the CLI against one known CSV in `data/` and verify four PNG files are produced.
3. Open or inspect the outputs to confirm each plot reflects the expected statistic and uses the input file’s columns.
4. Run any available lint or syntax check for the new Python files.

**Decisions**
- Use separate PNG files for mean, min, max, and standard deviation, because that matches the confirmed preference and keeps each output simple to inspect.
- Keep the project minimal and CLI-first; no packaging metadata beyond `requirements.txt` unless it becomes necessary.
- Treat the input as one CSV file selected from the provided data directory, with explicit validation and a helpful error if the directory is empty or contains multiple candidate CSV files.

**Further Considerations**
1. Confirm whether the tool should infer the single CSV automatically from the directory or require the user to pass the CSV file path directly. Recommendation: accept the directory path and infer the single file when possible, while allowing an explicit CSV path if needed.
2. Confirm whether PNGs should be written next to the input CSV or into a dedicated output directory. Recommendation: write them into a sibling `output/` directory to keep generated artifacts separate from source data.
```

These generated plans tend to follow a structure that includes steps, relevant files, verification procedures, decisions made and any other further considerations.

This now provides us with a planning document which we are able to review and amend as we wish.
This is very reasonable approach:

- Importantly, we are now moving from **ad-hoc** development to **intentional** development,
which forces us to consider ways forward and make decisions and capture these within a defined plan that we validate and refine before moving to implementation.
- We may extend this plan with other sections and further detail as needed.
- It has created a concrete document we can discuss and refine with colleagues before we proceed.
- It also provides a "checkpoint": if the implementation is unsatisfactory we can remove the implementation, amend the plan, and ask Copilot to create the implementation again.

FIXME: expand on the benefits of externalising the agentic reasoning a bit


:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Review!

5 mins.

As we know, we should always review the output from generative AI.
So with a skeptical mindset:

- Carefully review the generated plan and ensure it complies with the initial prompt.
- Does the plan match what you want from this tool?
- Refine the plan as needed.
- Ensure that a README.md file is created to hold instructions on how to install the prerequisites in a virtual environment and run the tool. If there isn't, add a new line about it to the `relevant files` section.
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

The build-in agent mode will then create an implementation based on the plan.
You'll need to review and approve various actions the agent needs to take in order to follow the plan.
If the agent seems to pause for too long, look for anything it wants you to select from the search dropdown at the top of the VSCode window.

:::::::::::::::::::::::::::::::::::::: challenge

## How did it do?

5 mins.

Run the tool on the command line, following the instructions in the README.md.
Does it work as specified?
Does it operate as you intended?
For example (your command many differ):

```bash
python csv_statistics_plotter.py data/inflammation-01.csv
```

Add your thoughts to the shared document.

::::::::::::::::::::::::::::::::::::::::::::::::


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

## A Docstring Writer Skill

Let's start with a very common task in software development, that of adding descriptions of our functions (and methods and modules) to our code.
These are commonly referred to as docstrings.

Let's define a skill that adds docstring suggestions to our code.
Our code may already have docstrings defined, but perhaps we want to ensure that all docstrings follow a common format,
such as restructured text, e.g.:

```python
def concat(arg1, arg2):
    """Returns the result of concatenating ``arg1`` with ``arg2``.

    :param arg1: A first string to concatenate.
    :type arg1: str
    :param arg2: A second string to add to the first string.
    :type arg2: str

    :return: A string containing the concatenation of the two arguments.
    :rtype: str
    """
    # This is the actual function implementation.
    return arg1 + arg2
```

In VSCode, agents are typically defined in a file with a `.agent.md` suffix.
Create a new `add-docstrings.agent.md` file in the `.github/agents` directory, and add the following contents:

~~~markdown
---
name: add-docstrings
description: Writes docstrings for Python code in the reST format.
compatibility: Requires python3
metadata:
  version: 1.0.0
---

This skill adds docstrings to Python source code files.

## Constraints

- ONLY make modifications to Python source code files.
- ALWAYS create module, function and method docstrings.
- Docstrings should ALWAYS be in the reStructuredText format.
- Where docstrings already exist, ALWAYS convert them to the reStructuredText format.

## Approach

1. Analyse source code and identify where new docstrings are needed, or existing ones need to be updated.
2. Make the identified changes to the source code.
3. Provide a summary of what was done.
~~~

We're being careful to define constraints on the behaviour here in addition to defining the approach to take as a sequence of simple, explicit steps.

Once you've saved the file, run the skill by entering `/add-docstrings` into the VSCode chat.
You should see reST docstrings added to your code.

:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Review!

5 mins.

We should always review AI-generated content,
so take a look at each of the newly added docstrings in turn and only select `Keep` if you agree with them!
If not, correct them instead.

::::::::::::::::::::::::::::::::::::::::::::::::


## A Code Linter Skill

Sometimes we want our skills to explicitly execute commands and do something based on analysing the output of those commands.
Code linters are well established tools for statically analysing code and reporting common code issues.
So how should we handle this?

Code linters are automated tools that analyse source code to identify potential errors, stylistic inconsistencies, and violations of coding standards before the software is run.
They help developers maintain cleaner and more readable code by highlighting issues such as unused variables, formatting problems, syntax mistakes, and poor programming practices.
Linters are commonly integrated into Integrated Development Environments (IDEs), and in the case of VSCode, additional linting tools can be installed as extensions, such as [Pylint](https://marketplace.visualstudio.com/items?itemName=ms-python.pylint) or [flake8](https://marketplace.visualstudio.com/items?itemName=ms-python.flake8).

We can install Pylint into our virtual environment by ensuring our virtual environment is activated, and doing:

```bash
python -m pip install pylint
```

Let's now define a skill that uses Pylint to identify code issues, provide a summary and plan of action, and fix these issues when the plan is approved.
Linters also suggest the use of docstrings, so we should account for that and stipulate a docstring format we typically use.

Create a new directory `pylint-fix` in the `.github/skills` directory,
and add the following contents to a new `SKILL.md` file within the `pylint-fix` directory:

~~~markdown
---
name: pylint-fix
description: Identify and fix code styling issues using the Pylint code linter.
compatibility: Requires python3
metadata:
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
~~~

### Improving our Skill

As we have it, the definition may seem reasonable, but let's check it over.

:::::::::::::::::::::::::::::::::::::: challenge

## Class Exercise: So What's Wrong with It?

5 mins.

This skill is more complicated than our first one.
What do you think could be improved? Is there anything missing?

Add your thoughts to the shared document.

::::::::::::::::::::::::::::::::::::::::::::::::

One way to help us identify any issues is to use the `Chat Customizations Evaluations` extension we installed earlier:

1. Select `Analyze` that should have appeared at the bottom of the `SKILL.md` file editor pane.
If you receive an error here, check the syntax of the `SKILL.md` file.
1. A pop-up box will request permission to conduct the analysis. Select `Allow` (or similar).
1. When the analysis is complete, select `Show problems` when the pop-up appears, which will display a list of issues in the `PROBLEMS` tab in the bottom VSCode pane.

You should see a whole host of issues that indicate problems with ambiguity, cognitive load, coverage gaps, amongst others.

Within the file code editor, you should see these issues highlighted with underlines.
If you hover over either the entries in the `PROBLEMS` tab, or the editor underlines,
a description will pop up explaining the issue.

:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Fix our `SKILL.md`

10 mins.

Go through each of the identified issues and fix them.

One way to do this such that you get an opportunity to review the suggestions before they are applied,
is to first right-click on the entry in the `PROBLEMS` tab and select `Explain` which drafts a set of proposed changes.

The Chat Customisations Evaluations tool can be a bit overzealous,
so if you think a suggestion is overkill, feel free to ignore it.

If you're happy with the change, select the `Apply to...` icon at the top right of the suggestion to apply it to the skill file,
then select `Keep` in the editor window.
Otherwise, `Undo` the change and add your own fix.

Note: you may find that even once the identified problems are fixed, issues in the `PROBLEMS` tab still remain.
This appears to be a bug, and restarting VSCode should fix this.

:::::::::::::::::::::::::: solution

You can find an example improved `SKILL.md` [here](../learners/files/skills/pylint-fix/SKILL.md).

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

Run the skill by entering `/pylint-fix` in the VSCode chat window.
Once complete, you should see that the skill has corrected a number of issues and added docstrings to our codebase.


::::::::::::::::::::::::::::::::: callout

## When You Have a Hammer...

...everything looks like a nail.

We've created a generative AI skill for this which seems useful.
But using AI tools come at a cost - typically, they take a while to run and are prone to error.
What we should also consider is whether a more dedicated tool on its own would be a better fit.
For example, would simply using a code linting tool such as pylint, flake8, [pycodestyle](https://pycodestyle.pycqa.org/en/latest/) or [ruff](https://docs.astral.sh/ruff/) on its own - and following it's suggestions - be enough?
If we wanted automatic code formatting, would [black](https://black.readthedocs.io/en/stable/) be sufficient?

It comes down to a series of trade-offs we should consider.
AI-based solutions often offer powerful, broader sets of capabilities that *may* save time,
but dedicated tools run more efficiently and with a greater degree of precision, but *may* require more manual work.

:::::::::::::::::::::::::::::::::::::::::


## A Unit Test Builder Skill

A critical step in writing robust, reproducible code is to test your software,
and in addition to testing software manually,
the writing and running of unit tests is established practice that can save you time - for those types of test thare are amenable to automation.

[Pytest](https://docs.pytest.org/en/stable/) is a popular Python testing framework that makes it straightforward to write and run automated tests.
Unit tests help developers verify that individual functions and components behave as expected,
reducing the likelihood of defects and making it safer to modify or extend software over time.
By writing tests, teams can detect problems early and increase confidence that changes have not introduced old issues known as regressions.
Pytest is widely used because it has a simple and readable syntax, requires minimal boilerplate code, provides detailed failure reports
and integrates easily with continuous integration (CI) systems to automate testing as part of the development workflow.

### A Quick Tour of Pytest

Let's set up Pytest and create a directory to hold our tests:

```bash
python -m pip install pytest
mkdir tests
```

Within the tests directory, we'll also create an example test in a new `test_example.py`:

```python
def test_addition():
    assert 1 + 2 == 3
```

Ideally, we'd of course create a test that actually tests the code we've written,
but since your code will likely differ in structure we've just used a generic example.
We can now run this test (and any others that may be created in the future within this file) by doing:

```bash
python -m pytest tests/test_example.py
```

And you should see the following:

```
================================ test session starts ================================
platform darwin -- Python 3.14.2, pytest-9.0.3, pluggy-1.6.0
rootdir: /Users/steve/Test
configfile: pyproject.toml
collected 1 item                                                                    

tests/test_example.py .                                                       [100%]

================================= 1 passed in 0.01s =================================
```

Delete the `tests` directory along with its contents.

:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Create, Review and Refine a Unit Test Writer Skill

15 mins.

Based on what we've learned from writing our pylint skill,
write a skill for automatically Pytest unit tests.
Ensure it has the following sections:

- Constraints
- Approach: list the steps the skill needs to take to generate the unit tests
- Commands to Use

Once finished, use the Chat Customisations Evaluations tool as before to review the skill and suggest improvements,
and refine the skill until you are happy with it.

Finally, use the skill to create some unit tests, e.g. enter `add-tests` into the chat window.

:::::::::::::::::::::::::: solution

An initial try:

~~~markdown
---
name: add-tests
description: Create Pytest unit tests.
compatibility: Requires python3
metadata:
  version: 1.0.0
---

This skill writes Pytest unit tests.

## Constraints

- ONLY write tests and test files in the `tests` directory.
- Tests should ONLY be written for Pytest.

## Approach

1. Examine the code and identify suitable unit tests.
2. Write the identified unit tests.
3. Run pytest to verify the tests.
3. Provide a summary of what was created and the results of running pytest.

## Commands to Use

- Ensure the virtual environment is activated before running pytest

```bash
python -m pytest tests/
```
~~~

After running the Chat Customizations Evaluation tool on it:

~~~markdown
---
name: add-tests
description: Create Pytest unit tests.
compatibility: Requires python3
metadata:
  version: 1.0.0
---

This skill writes Pytest unit tests.

## Constraints

- ONLY write tests and test files in the `tests` directory.
- If the `tests` directory does not exist, create it and add an empty `__init__.py` file before writing any test files.
- Tests should ONLY be written for Pytest.
- Name test files as `test_<source_module>.py` and test functions as `test_<function_name>_<scenario>`, e.g. `test_parse_input_empty_string`.

## Approach

1. Examine the code and identify unit tests for all public functions and methods, prioritizing logic branches, edge cases, and error conditions. Exclude trivial one-line wrappers unless they have meaningful behavior.
2. Write the identified unit tests.
3. Run pytest to verify the tests.
4. If pytest exits with a non-zero code, report the specific failing tests and their error output, then attempt to fix the test code. Do not report success until all tests pass.
5. Provide a summary of what was created and the results of running pytest.

## Commands to Use

- Ensure the virtual environment is activated before running pytest

```bash
python -m pytest tests/
```
~~~

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::: challenge

## Class Exercise: How did it do?

5 mins.

Run the tests on the command line, e.g.:

```bash
python -m pytest -m tests/
```

Did they pass?

Take a look at the tests that have been written,
and add your thoughts to the shared document on the following questions:

- Can you understand them?
- How useful are they to you?
- How could they be improved? What's missing?

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::: callout

## Pros and Cons

We should consider how we use AI tools to verify the code it has written very carefully.
AI-generated tests often focus on the most obvious behaviours and may miss important edge cases, error conditions, and domain-specific requirements that an experienced developer would consider.
Tests may therefore provide a false sense of confidence while leaving significant defects undetected.

Another concern is that writing tests is itself a valuable design activity.
Creating unit tests forces developers to think carefully about how code should behave,
as well as its interfaces, assumptions and failure modes.
If AI generates all the tests, developers may miss opportunities to deepen their understanding of the code and identify design weaknesses.

AI-generated tests can also suffer from the same issues as AI-generated code: incorrect assertions, unrealistic test data, brittle implementation-dependent checks, or tests that merely reproduce the logic of the code under test rather than independently verifying it.
In some cases, the tests may pass while providing little meaningful verification.

As with development, the most effective approach is usually to treat AI as an assistant rather than an author.
AI can help generate test scaffolding, suggest test cases, and identify potential edge conditions,
but developers should review, refine, and validate the resulting tests to ensure they genuinely improve software quality.

:::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::: callout

## Should we Even use AI at all for Generating Tests?

As we've said before, it's easy to reach for AI to attempt a task for us.
In this case, beyond using AI to set up the test scaffolding,
there are established tools that are more specifically designed to create unit tests for software.

For example, [Hypothesis](https://hypothesis.readthedocs.io/en/latest/) is a property-based testing library for Python.
It allows you to specify the *domain* of data values you wish to test for a given test case,
and it generates a distribution of test values that select for edge cases within that domain that you test against high-level assertions.

:::::::::::::::::::::::::::::::::::::::::


## A Documentation Skill

Although writing good software documentation is critical for understanding and using code, it's often a task many developers find tedious.
However, there are patterns to writing documentation that are amenable to AI assistance, albeit with some important caveats.

A typical tool for writing software documentation is [MkDocs](https://www.mkdocs.org/).
MkDocs is a lightweight and open-source static site generator designed specifically for creating software documentation from Markdown files.
Developers use MkDocs because it provides a simple and efficient way to write, organise, and publish documentation that is easy to maintain alongside source code in version control repositories.
It automatically generates a professional-looking website from documentation files, supports navigation menus, search functionality, themes, and extensions, and integrates well with platforms such as GitHub for automated deployment.
The process of authoring and generating documentation is straightforward compared to many other methods,
and whilst this helps to reduce the burden on writing documentation somewhat,
writing docs is often a mechanistic process.

### A Quick Tour of MkDocs

For example, we can create a new MkDocs project in our code by installing the `mkdocs` Python libraries and invoking it:

```bash
python -m pip install mkdocs mkdocstrings[python]
python -m mkdocs new .
```

This will create two files in your project: `mkdocs.yml` and `docs/index.md`.
The first file `mkdocs.yml` is the configuration file for your documentation site,
which serves as the central configuration hub for your MKDocs documentation.
It tells MKDocs how to structure your documentation site, which plugins and themes to use,
how to organize navigation, etc.
`docs/index.md` is the main landing page of your documentation site.

Let's first look at the `mkdocs.yml` file.
It is almost empty, so let's edit it to contain the following:

```yaml
site_name: Inflammation Data Analysis

nav:
  - Overview: index.md

plugins:
  - search
  - mkdocstrings
```

Here we give a name to our documentation site,
and set up the navigation menu with one item `Overview` that links to `index.md`.
We also enable two plugins, `search` to provide search functionality in the documentation site, and `mkdocstrings` to automatically generate API reference documentation from Python docstrings (i.e. like the ones generated by our `pylint-fix` skill).

We can try to render the documentation site locally and see what it looks like:

```bash
python -m mkdocs build
python -m mkdocs serve
```

This will start to build a local static documentation site and serve it at a local web server. 
By default, it will be available at `http://127.0.0.1:8000/`, which will also show in the terminal output.
You can open this URL in your web browser to view the documentation site.

Ordinarily, we could iterate on writing and verifying what's built until a first version is ready,
and if we had this repository in our own GitHub account,
we could go ahead and deploy this to our repository with `python -m mkdocs gh-deploy` which would deploy it to a new `gh-pages` branch and it would be visible from the repository's GitHub Pages website.

:::::::::::::::::::::::::::::::::::::: challenge

## Solo Exercise: Create, Review and Refine an MkDocs Skill

15 mins.

Write a skill for automatically generating documentation using MkDocs.
Ensure it has the following sections:

- Constraints
- Approach: list the steps the skill needs to take to generate the documentation
- Initial `mkdocs.yml` Template: the starting content for the `mkdocs.yml` file
- Documentation Sections: a list of sections to include in the documentation, including an overview of the software, software prerequisites, and how to run the software
- Commands to Use

Once finished, use the Chat Customisations Evaluations tool as before to review the skill and suggest improvements,
and refine the skill until you are happy with it.

Finally, use the skill to create the documentation, e.g. enter `build-mkdocs` into the chat window.

:::::::::::::::::::::::::: solution

Here's one solution:

~~~markdown
---
name: build-docs
description: Author and build software documentation using MkDocs.
compatibility: Requires python3
metadata:
  version: 1.0.0
---

This skill uses MkDocs to create and build software documentation using MkDocs

## Constraints

- ONLY make modifications to the `mkdocs.yml` file and files in the `docs/` directory
- If `mkdocstrings` is not installed or the project does not use Python docstrings, remove it from the plugins list and notify the user.

## Approach

1. Inspect the codebase to identify software prerequisites needed to run the software and the automated tests.
2. Create a `mkdocs.yml` file from the template below.
3. Write documentation sections as specified below.
4. Provide a summary of what was done.

## `mkdocs.yml` Template

```yaml
site_name: Replace with the actual name of the software being documented.

nav:
  - Overview: index.md
  - Software Prerequisites: prerequisites.md
  - Running the Software: running.md
  - Running the Tests: tests.md
  - API: api.md

plugins:
  - search
  - mkdocstrings
```

## Documentation Sections

- `docs/index.md`: a brief overview of what the software aims to do, its high-level architecture, and the key technologies or libraries it uses.
- `docs/prerequisites.md`: contains instructions on how to install the software prerequisites needed to run the software.
- `docs/running.md`: instructions on how to run the software.
- `docs/tests.md`: instructions on how to run the automated tests.
- `api.md`: API documentation based on the code and its docstrings.

## Commands to Use

- Ensure the virtual environment is activated before building mkdocs

```bash
python -m mkdocs build
```
~~~

Following the Chat Customizations Evaluation tool:

~~~markdown
---
name: build-docs
description: Author and build software documentation using MkDocs.
compatibility: Requires python3
metadata:
  version: 1.0.0
---

This skill uses MkDocs to create and build software documentation using MkDocs

## Constraints

- ONLY make modifications to the `mkdocs.yml` file and documentation files.
- If `mkdocs.yml` already exists, preserve any existing `site_name` and any nav entries not covered by the template, and merge the template structure into it rather than overwriting it.
- If `mkdocstrings` is not installed or the project does not use Python docstrings, remove it from the plugins list, remove the `API Reference` entry from `nav`, populate `docs/api.md` with manually authored API documentation extracted from the source code and note that it is not auto-generated, and notify the user.

## Approach

1. Inspect the codebase to identify software prerequisites needed to run the software and the automated tests.
2. Create a `mkdocs.yml` file from the template below.
3. Write documentation sections as specified below.
4. Provide a summary of what was done.

## `mkdocs.yml` Template

```yaml
site_name: Replace with the actual name of the software being documented.

nav:
  - Overview: index.md
  - Prerequisites: prerequisites.md
  - Running: running.md
  - Tests: tests.md
  - API Reference: api.md

plugins:
  - search
  - mkdocstrings
```

## Documentation Sections

- `docs/index.md` - a brief overview of what the software aims to do, its high-level architecture, and the key technologies or libraries it uses.
- `docs/prerequisites.md` - contains instructions on how to install the software prerequisites needed to run the software. If the project contains a requirements.txt, pyproject.toml, or similar dependency file, include virtual environment setup instructions. Otherwise, omit that subsection.
- `docs/running.md` - instructions on how to run the software.
- `docs/tests.md` - instructions on how to run the automated tests. If no automated tests are found in the codebase, state that clearly in the file and keep the page in the nav with a brief placeholder note.
- `docs/api.md` - API documentation based on the code and its docstrings, or manually authored API documentation extracted from the source code if `mkdocstrings` is not available; note when the documentation is not auto-generated.

## Commands to Use

- If a virtual environment is already available, activate it before running the build command.

```bash
python -m mkdocs build
```
~~~

:::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::: challenge

## Class Exercise: How did it do?

5 mins.

Serve the built documentation and view it in a browser, e.g.:

```bash
python -m mkdocs serve
```

Direct a browser to http://localhost:8000/

Take a look at the documentation,
and add your thoughts to the shared document on the following questions:

- Would they be adequate for another developer unfamiliar with the project to understand (at a basic level) what it's about, how to set it up, and how to run the code and the tests?
- How could they be improved?

::::::::::::::::::::::::::::::::::::::::::::::::


## Summary

So far we've created AI-based tools for common development tasks,
as well as explored the limitations and risks of using AI for such activities.

FIXME: finish summary


::::::::::::::::::::::::::::::::::::: keypoints 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::
