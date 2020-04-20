---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
>
> This tutorial guides you through the the fundamentals of
> designing and building an analysis workflow.
> It assumes no previous knowledge or experience of workflows
> or Common Workflow Language (CWL),
> but does assume some experience with the Unix command line.
>
> Before following this tutorial,
> you should be comfortable working in a Unix command line environment
> and familiar with fundamental commands (`cd`, `mv`, `mkdir`, etc),
> piping and redirection,
> and simple Bash scripting,
> such as might be gained from following the [Software Carpentry][swc]
> lesson, [The Unix Shell][swc-shell].
> You might also have some experience with running
> tasks on a remote machine (by `ssh` connection)
> and in a cluster (high performance computing) environment.
>
> If you have previously written a workflow description,
> in CWL or another langauge,
> you may want to look instead at the [User Guide][cwl-user-guide].
{: .prereq}

### Target Audience

This tutorial is aimed at researchers
and research software engineers
who would like to begin automating their analyses in workflows.
If you're unsure whether this tutorial is a good fit for you,
you may find our [learner profiles]({{ site.base_url }}/audience/) helpful.
These are also a useful resource during the lesson design process.

### Learning Objectives

#### Goals

Below is a list of draft learning objectives,
(very) roughly broken down into chunks that feel like they belong together.
These chunks _could_ form the basis for splitting tutorial material into episodes.
Most blocks include one objective in __bold__:
this is intended to flag it up as a higher-level objective,
encapsulating multiple smaller,
more fine-grained,
objectives that follow.
The higher-level objectives may be suitable as learning objectives
for the tutorial as a whole
(i.e. they would be shown on the landing page,
used to guide development and structuring of material, etc)
while the "sub-objectives" would apply to specific sections (episodes)
of the tutorial,
and be used to guide the creation of exercises and content of those episodes.

The chunks get a bit less well-defined towards the end.
See [this page]({{ site.base_url }}/objective-notes/) for the original notes these objectives are based on,
created during the March 2020 lesson development sprint.

After following one of these tutorials, learners will be able to:

- __define the files that will be included as output of a workflow__ (lesson-level objective)
- explain that only files explicitly mentioned in a description will be included in the output of a step/workflow
- implement bulk capturing of all files produced by a step/workflow for debugging purposes
- use STDIN and STDOUT as input and output
- capture output written to a specific directory, the working directory, or the same directory where input is located

- __implement scattering of steps in a workflow__ (lesson-level objective)
- explain what is meant by the _scatter_ pattern in workflow design, and how it differs from the similar concept of parallel execution
- identify when the scatter pattern appears in a workflow description
- (FIXME: does the above cover the intended meaning of the following two points from the lesson development sprint?)
  - running the same program on each file
  - running the same program the same way except for one parameter

- __convert a shell script into a CWL workflow__ (lesson-level objective)
- explain the difference between _control flow_ and _data flow_
- identify the inputs and outputs, tasks, and links in a script
- (recognize and remove details and configuration in a script that are specific to particular infrastructure)

- __explain how a workflow document describes the input and output of a workflow and the flow of data between tools__ (lesson-level objective)
- explain the difference between a CWL tool description and a CWL workflow
- describe the relationship between a tool and its corresponding CWL document
- exercise good practices when naming inputs and outputs
- Be able to make understandable and valid names for inputs and outputs (not "input3")

- __describe all the requirements for running a tool__ (lesson-level objective)
- identify all the requirements of a tool and define them in the tool description
- use `runtime` parameters to access information about the runtime environment
- define environment variables necessary for execution
- use `secondaryFiles` or `InitialWorkDirRequirement` to access files in the same directory as another referenced file
- use `$(runtime.cores)` to define the number of cores to be used
- use `type: File`, instead of a string, to reference a filepath

- __explain that a workflow is a dependency graph__

- __be able to include their own script as a step in a workflow__
  - several options for this were identified during the lesson development sprint. we should choose one or two to focus on in the tutorial:
    - make script executable and add to path
        - advantage: quick
        - downside:
            - the scripts are not shipped with the CWL document itself
            - not portable to other execution environment without putting the script first
    - pack the scripts into a docker container and make them executable there:
        - advantage: portable
        - downside:
            - only works for people that are using containers
    - use InitialWorkDirRequirement to list script content directly in the CWLToolWrapper
        - example: https://github.com/CompEpigen/ATACseq_workflows/blob/master/CWL/tools/generate_atac_signal_tags.cwl (bad example as here the scripts are too long, only for short scripts)
        - advantage:
            - portable
            - can be used independent of the dependency management solution
        - disadvantage:
            - explodes the complexity of a CWL tool wrapper
            - having to deal with escapes and so on
    - distribute as seperate package via pip / cran / ...

- __sketch their workflow, both by hand, and with an automated visualizer__ (lesson-level objective)
  - use cwlviewer online
  - generate Graphviz diagram using cwltool
  - exercise with the printout of a simple workflow; draw arrows on code; hand draw a graph on another sheet of paper

- __recognize and fix simple bugs in their workflow code__ (lesson-level objective)
    - interpret commonly encountered error messages

- __document their workflows to increase reusability__
- explain the importance of documenting a workflow
- use description fields to document purpose, intent, and other factors at multiple levels within their workflow
- recognise when it is appropriate to include this documentation

- recognise that workflow development can be iterative i.e. that it doesn't have to happen all at once

- explain the importance of correctly citing research software
- [give credit](https://www.commonwl.org/v1.1/CommandLineTool.html#SoftwarePackage) for all the tools used in their workflows
  - See also https://github.com/common-workflow-language/cwl-utils/blob/master/cwl_utils/cite_extract.py

- customize a workflow at any of the many levels
    - Change the input object
    - Change the default values at the workflow level
    - Change hard coded values at the workflow level
    - Change default value at the Workflow step level
    - Change hard coded values at the Workflow step level
    - Change default values in the CLT description
    - Change hard coded values in the CLT description
    - Change the container
    - Change the tool source itself

---

Objectives that require further discussion, if (and how) they should be included.
- Know what a sub workflow is, how to make one, and when to use them.
- Work around a bad software container (requires to be run as root user inside the container, expects certain directory paths)
- Know how to use CWL v1.2 [dynamic workflow conditionals](https://www.commonwl.org/v1.2.0-dev2/Workflow.html#WorkflowStepInput)
    - Not yet released, expected in Q2 2020
- Run their workflow on a {Sun Grid Engine, LSF, Slurm,..} HPC system
    - explain the benefit and restrictions of HPC systems (high throughput, network restrictions, etc)
    - provide example options (e.g. Toil)
- Convert a GNU Makefile to a CWL workflow

{% include links.md %}
