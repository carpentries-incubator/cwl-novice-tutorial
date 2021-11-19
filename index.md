---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

### For Contributors

Use the _Episodes_ menu above to browse through the pages for
indivual blocks of learning objectives.
These pages can be used for designing challenges/exercises
for the tutorial,
as described in detail on [this Issue](https://github.com/common-workflow-lab/cwl-novice-tutorial/issues/7).

You may find it helpful during lesson/exercise design to
refer to [these draft concept maps](https://docs.google.com/presentation/d/1aVdK8LHkgtESBunCQ-p7XmEl8NB9XbgDsH67X0_2HWg/edit#slide=id.g72208cbc10_0_264)
for the tutorial material

> ## Example rendered exercise
>
> This is the body of the challenge.
>
> > ## Solution
> >
> > This is the body of the solution.
> {: .solution}
{: .challenge}

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
If you're unsure whether this tutorial is a good fit for you
check the prerequisites listed above.
You may also find our [learner profiles][audience] helpful.
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
See [this page][objective-notes] for the original notes these objectives are based on,
created during the March 2020 lesson development sprint.

After following one of these tutorials, learners will be able to:

- __define the files that will be included as output of a workflow__ (lesson-level objective)
  - explain that only files explicitly mentioned in a description will be included in the output of a step/workflow
  - implement bulk capturing of all files produced by a step/workflow for debugging purposes
  - use STDIN and STDOUT as input and output
  - capture output written to a specific directory, the working directory, or the same directory where input is located
- __convert a shell script into a CWL workflow__ (lesson-level objective)
  - explain the difference between _control flow_ and _data flow_
  - identify the inputs and outputs, tasks, and links in a script
  - (recognize and remove details and configuration in a script that are specific to particular infrastructure)
  - (remove loops for moment, these will be covered in intermediate lesson with scatter)
- __explain how a workflow document describes the input and output of a workflow and the flow of data between tools__ (lesson-level objective)
  - explain the difference between a CWL tool description and a CWL workflow
  - describe the relationship between a tool and its corresponding CWL document
  - exercise good practices when naming inputs and outputs
  - Be able to make understandable and valid names for inputs and outputs (not "input3")
- __describe the file and variable requirements for running a tool__ (lesson-level objective)
  - identify all the requirements of a tool and define them in the tool description
  - use `runtime` parameters to access information about the runtime environment
  - define environment variables necessary for execution
  - use `secondaryFiles` or `InitialWorkDirRequirement` to access files in the same directory as another referenced file
  - use `type: File`, instead of a string, to reference a filepath
- __explain that a workflow is a dependency graph__
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

{% include links.md %}
