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

As a first step to check if your CWL script contains any errors, you can run the workflow with the `--validate` flag.
~~~
cwltool --validate CWL_SCRIPT.cwl
~~~
{: .language-bash}

It is possible that the script is validated, however, it still gets an error. 
If you encounter an error, the best practice is to run the workflow with the `--debug` flag.
This will provide you with extensive information on the error you encounter.
~~~
cwltool --debug CWL_SCRIPT.cwl
~~~
{: .language-bash}

### YAML errors
First of all, errors in the YAML syntax. When writing a piece of code, it is very easy to make a mistake.

Some very common YAML errors are:
- Using tabs instead of spaces. In YAML files indentations are made using spaces, not tabs. 
Errors when using tabs will show `found character \t`. 
![]({{page.root}}/fig/YAML_error_tab.png)
- Typos in field names. It is very easy to forget for example the capital letters in field names.
Errors with typos in field names will show `invalid field`.
![]({{page.root}}/fig/YAML_error_typo_fieldname.png)
- Typos in variable names. Similar to typos in field names, it is easy to make a mistake in referencing to a variable.
These errors will show `Field references unknown identifier.`
![]({{page.root}}/fig/YAML_error_typo_variable.png)

### Wiring error
Wiring errors often occur when you forget to add an output from a workflow's step to the `outputs` section.
This doesn't cause an error message, but there won't be any output in your directory.
To get the desired output you have to run the workflow again.
Best practice is to check your `outputs` section before running your script to make sure all the outputs you want are there.

### Type mismatch
Type errors take place when there is a mismatch in type between variables.
When you declare a variable in the `inputs` section, the type of this variable has to match the type in the YAML inputs file
and the type used in one of the workflows steps.
The error message that is shown when this error occurs will tell you on which line the mismatch happens.

![]({{page.root}}/fig/Type_error.png)

### Format error
Some files need a specific format that needs to be specified in the YAML inputs file, for example the fastq file in the RNA-seq analysis.
When you don't specify a format, an error will occur. You can for example use the [EDAM](https://www.ebi.ac.uk/ols/ontologies/edam) ontology.

![]({{page.root}}/fig/Format_error.png)


{% include links.md %}
