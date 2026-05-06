---
title: "Planning a Development Project using Copilot Agents"
teaching: 0
exercises: 0
---

:::::::::::::::::::::::::::::::::::::: questions 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::: objectives

- Use the built-in planning agent to develop a basic Python application
- Use Copilot to automatically generate a basic agent to gather requirements
- Describe the format and basic configuration of an agents definition file
- Create and use an improved Copilot agent to gather requirements into a set of requirements
- Create and use a Copilot agent to produce a technical specification from a set of requirements

::::::::::::::::::::::::::::::::::::::::::::::::

FIXME: clone example repo

## Our Example Scenario

## Using the Built-in Plan Agent

Instead of "one-shotting" the development of code using an AI coding tool, 
we've seen that a better approach is to plan and implement our code in a step-wise, 
incremental fashion.
So how should we go about this?

One way would be to use the built-in VSCode plan agent that helps developers break down tasks into clear and actionable steps before writing code.
Instead of jumping straight into implementation, it generates structured plans for features or for other code modification activities, improving clarity and efficiency.
It aims to guide users through a thoughtful planning phase to reduce errors and encourage better design and implementation decisions.

FIXME: add learning actions

This is very reasonable approach.
We are now moving from **reactive** development to **intentional** development,
which forces us to consider ways forward and make decisions and capture these within a defined plan that we validate and refine before moving to implementation.

However, there are some limitations with the built-in Plan mode:

- As implied by its name, it actively avoids moving to implementing solutions. Conversations are pulled back to planning instead of moving forward. If we want to go further, we need prompts or another mechanism to do that.
- It follows a predefined workflow that may not fit your working style or process, gathering context and constraints, developing a structured plan before moving to plan execution. This is a very sensible approach and guardrail against premature implementation, but may not offer the flexibility or adherence to how you want to do things.

## Introducing Agents

One way to overcome these limitations would be to define and use a custom agent
that follows a behaviour that we define ourselves.
These give us:

- Specialised, tailored instructions for tasks; with more than one of these, we have reusable workflows we can use in many projects
- Greater action consistency, since these instructions are followed each time
- Define general constraints for output: coding standards, practices, conventions, etc.
- Define guardrails and explicit allowable actions: e.g. read-only

Generally, this approach is much quicker than setting up all this context every time for every kind of task - if you have a set way you tend to do something, define an agent to do it.

However, we've seen that different stages of a development process require different mindsets and approaches.
By creating a single agent that attempts to do everything,
we risk an overly complex definition that is prone to failure.
Instead, let's create a separate agent for each stage of development that is each responsible for generating a specification that requires review before moving on to the next stage:

- **Requirements Gatherer** - responsible for gathering and clarifying requirements, generating a Product Requirements Document (PRD)
- **Technical Archtect** - given a PRD, creates an architecture and overall design, generating a technical design specification
- **Implementer** - given a design specification, creates an implementation

This approach also has the advantage of token efficiency.
By creating more tightly defined agents each with a narrower clarity of purpose,
this minimises the size of the context window - and use of tokens - whilst avoiding ambiguity in any given situation.

There are many ways we could choose to define these agents,
in terms of their overall behaviour and the practices we want them to follow.
For the purposes of this training, we'll consider a generic set of agents that cover the basics but are readily modifiable as needed.
We should ensure their generated stage documents are located in the same directory, so they're logically grouped together, e.g. a `project-docs` directory.


## Creating a Requirements Gathering Agent

There are a number of things we should consider to create our requirements gathering agent, for example:

- **Assumptions** - we should always be explicit and clear what underlying assumptions have been made for the stated requirements, to avoid misunderstandings about what is included
- **User stories with acceptance criteria** - define project requirements in terms of user stories, i.e. "As a [user type], I want [goal] so that [benefit]", each with clear acceptance criteria
- **Success metrics** - generally, what does a successful implementation look like?
- **Out of scope items** - clarify what should not be considered

### A First Try...

One way to create our requirements agent is to use the built-in `/create-agent` VSCode chat command.

For example, in the VSCode chat ensure you have the `GPT-5.4 mini` agent selected in the model dropdown, and enter:

```
/create-agent a requirements gathering agent that creates a requirements specification document `project-docs/requirements.md` based on a prompt, which contains sections on assumptions, user stories, success metrics, and items which are out of scope
```

Here, our request briefly captures the above, explicitly requesting the generation of a `requirements.md` document within a `project-docs` directory.

This generally produces a very reasonable definition,
although given the probablistic nature of LLMs, yours will differ.

For example, this prompt generated a file called `requirements-gatherer.agent.md` that includes the following agent definition:

```markdown
---
description: "Use when turning a prompt into a requirements spec for project-docs/requirements.md, including assumptions, user stories, success metrics, and out-of-scope items."
tools: [read, search, edit]
user-invocable: true
---
You are a specialist at turning a prompt into a concise requirements specification.

Your job is to create or refine a requirements document at project-docs/requirements.md from the supplied prompt.

## Constraints
- DO NOT invent implementation details.
- DO NOT expand scope beyond what the prompt supports.
- DO NOT write design or implementation plans.
- ONLY produce requirements content and ask clarifying questions when necessary to avoid guessing.

## Approach
1. Extract the core problem, intended users, and any explicit constraints from the prompt.
2. Identify assumptions that are necessary to proceed and separate them from confirmed facts.
3. Write clear user stories, success metrics, and out-of-scope items in concise markdown.
4. Keep the document focused on requirements rather than solutions.

## Output Format
Return a markdown requirements document with these sections:
- Assumptions
- User Stories
- Success Metrics
- Out of Scope

If the prompt is too ambiguous to draft responsibly, ask only the minimum clarifying questions needed.
```

Agent definitions tend to follow a common pattern of defining agent metadata, role, and aspects of its overall behaviour separated into subsections.

So at the top of this definition, there is [YAML](https://yaml.org/) front matter that defines metadata about this agent,
including a plain text description, whether this agent can be invoked by the user, and which tools this agent is allowed to use.
This explicit declaration of allowable tools enables us to conform this agent to the Principle of Least Privilege,
ensuring we only give it permissions that it needs to accomplish its role.

In this case:

- `read` - the agent is allowed to read files in this VSCode workspace, such as source code and other files
- `search` - allows the agent to search across this workspace
- `edit` - the agent may edit and modify files within this workspace

If you select the `Configure Tools...` text above this line, you'll see a pop-up dropdown containing a complete set of allowable permissions to select for this agent.

![VSCode agent file configure tools pop-up dropdown window](fig/vscode-agent-configure-tools.png)

Note that these are arranged hierarchically, so we are able to assign sub-permissions within a particular group (e.g. `read/readFile`) if we want to be more specific.

Next, its behaviour starts with an initial declaration of the agent's role,
where it adopts a persona of a specialist writing a requirements specification.

::::::::::::::::::::::::::::::::: callout

## Managing Expectations...

Importantly, note that in this case whilst the role is declared as a requirements `specialist` to set the agent's persona,
we should not consider the output as we would if its coming from a *real* specialist or expert.
This is a dangerous trap to fall into with using generative AI,
since this declaration only provides an anchor for its behaviour,
not a guarantee of its competence!

As with all things generative AI, we should treat any output with skepticism and use it to inform our own thinking and decisions through careful review,
and not blindly accept its assertions.

:::::::::::::::::::::::::::::::::::::::::



::::::::::::::::::::::::::::::::::::: keypoints 

- FIXME

::::::::::::::::::::::::::::::::::::::::::::::::
