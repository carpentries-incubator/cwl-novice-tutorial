---
title: "Debugging Workflows"
teaching: 0
exercises: 0
questions:
- "introduce within above lessons?"
objectives:
- "interpret commonly encountered error messages"
- "solve these common issues"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

> ## Learning objectives
> By the end of this episode, learners should be able to
> __recognize and fix simple bugs in their workflow code__.
{: .callout}

When working on a CWL workflow, you will probably encounter errors. There are many different errors possible.
It is always very important to check the error message in the terminal, because it will give you information on the error.
This error message will give you the type of error as well as the line of code that contains the error.
Some of these errors will be explained in this episode. 

A first step to check if your CWL script contains any errors, you can run the workflow with the `--validate` flag.
~~~
cwltool --validate CWL_SCRIPT.cwl
~~~
{: .language-bash}

It is possible that the script is validated, however, it still gets an error. 
If you encounter an error, the best practice is to run the workflow with the `--debug` flag.
This will provide you with extensive information on the error you encounter.
~~~
cwltool --debug` CWL_SCRIPT.cwl
~~~
{: .language-bash}

## YAML errors
First of all, errors in the YAML syntax. When writing a piece of code, it is very easy to make a mistake.

Some very common YAML errors are:
- Using tabs instead of spaces. In YAML files indentations are made using spaces, not tabs. 
Errors when using tabs will show `found character \t`. 
- Typos in field names. It is very easy to forget for example the capital letters in field names.
Errors with typos in field names will show `invalid field`.
- Typos in variable names. Similar to typos in field names, it is easy to make a mistake in referencing to a variable.
These errors will show `Field references unknown identifier.`

## Wiring errors



## Type mismatch

## Format error


(non-exhaustive) list of possible examples:

- YAML errors
- "wiring errors" e.g. where is the output from my step?
- type mismatch
- no formats on input but format is required by workflow

{% include links.md %}
