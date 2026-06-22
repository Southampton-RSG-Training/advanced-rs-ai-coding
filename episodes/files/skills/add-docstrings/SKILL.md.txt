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
