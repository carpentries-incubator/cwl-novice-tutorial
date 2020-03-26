---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---
FIXME: home page introduction

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
>
> FIXME
{: .prereq}

#### Learner profile outlines

#### Researcher who expects to publish protocol

- who are they?
    - life sciences background
    - already learned essential computational research skills - attended a Software Carpentry workshop last year?
    - developing a new experimental protocol 
    - writing a pipeline/protocol to analyse the data generated
        - this combines multiple command line/bioinformatics tools in a few shell scripts that run on their research group's local server
- what problem are they having?
    - when they publish this research, they expect to be asked to include a reproducible description of the data analysis
    - the funding body requires them to publish details of their analyses in full
    - need to adapt existing script(s) to work in local HPC environment, as incoming experimental data requires upscaling of analysis
- how will the tutorial help them?
    - after applying what they've learned in the tutorial, their analysis pipeline will be 
        - more portable between compute environments
        - easy to share alongside publication of research findings (separate methods paper?)
    - after applying what they've learned in the tutorial, they will be able to provide information of provenance of research findings on request e.g. from reviewer/funder/collaborator
        - more robust to changes in tool/dependancy versions and adjustments to the protocol itself

#### RSE who inherited protocol and now needs to deploy/scale up

- who are they?
    - bioinformatics background
    - [some details about their training here...]
    - just joined a new lab and inherited several scripts from departing postdoc
    - leading development on a new tool
- what problem are they having?
    - all of these things will need to be deployed/deployable in a cloud environment soon
    - since the postdoc left, several key dependencies have been updated and the pipeline currently doesn't run
        - it feels like every time they manage to fix these problems, another update is released and everything breaks all over again
    - the group leader wants to change the short read alignment tool used in a key step in the existing workflow
- how will the tutorial help them?
    - after applying what they've learned in the tutorial, their analysis pipeline will be 
        - more portable between compute environments
        - more robust to changes in tool/dependancy versions
        - more maintainable
            - robust to adjustments to the protocol itself
            - quicker to make these adjustments

#### The Masters/PhD student who wants to learn workflows

- a lot of wet lab hours during Masters
- wants to learn computational research skills
- has been assigned task of implementing pipeline for variation of ChIP-seq analysis
- has never connected before to a remote machine from the command line
- supervisor is aware of some of the benefits of implementing this kind of workflow
    - pipeline will end up being run often and on large scale
    - why CWL? to ensure resulting workflow could be run anywhere, most flexible once workflow is described


### Learning Objectives

After following one of these tutorials, learners will be able to:


- Know that workflow development can be iterative, can involve sketching, prototypes; that it doesn't have to happen all at once
- How CWL can help you [give credit](https://www.commonwl.org/v1.1/CommandLineTool.html#SoftwarePackage) for all the tools use used
See example [here](https://github.com/common-workflow-language/cwl-utils/blob/master/cwl_utils/cite_extract.py)
- Be able to explain the difference between a CWL tool description and a CWL workflow (description)
- Be able to make understandable and valid names for inputs and outputs (not "input3")
- Describe all the requirements for running a tool: environment variables, and more
- Realize that a workflow is a dependency graph
- Know that all output files must be explicitly captured and how to do so
- Recognize when the same step is being run but the input files vary (or may a parameter varies, or both) and that this is the "scatter" pattern. Know how to implement this using "scatter"
- Be able to split a bash script into a CWL workflow
- Be able to include their own script as a step in a CWL workflow
- Be able to document purpose, intent, and other factors within their workflow
- Be able to graph/visualize their workflow, both by hand, and with an automated visualizer
- Be able to interpret CWL error messages to recognize and fix simple bugs in their workflow code
- Be able to customize a workflow at any of the many levels
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
- Run their workflow on a {Sun Grid Engine, LSF, Slurm,..} HPC system
- Convert a GNU Makefile to a CWL workflow

{% include links.md %}
