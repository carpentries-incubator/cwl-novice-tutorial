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

#### Goals

After following one of these tutorials, learners will be able to:

- Know that all output files must be explicitly captured and how to do so
    - How to capture the output files
    - need to specify the files which you want to capture from the tools
    - bulk caputre the output for debug purpose.
        - which file is actually needed.
    - output files in the specific directory or working directory
    - output files in the same directory which has input files
    - stdout & stdin
- Recognize when the same step is being run but the input files vary (or may a parameter varies, or both) and that this is the "scatter" pattern. Know how to implement this using "scatter"
    - What is a CWL scatter
    - difference between scattering and parallel execution
    - running the same program on each file
    - running the same program the same way except for one parameter
    - Advanced: multidimensional scatter
- Be able to split a bash script into a CWL workflow
    - difference between a "control flow" (bash script) and a "data flow" (CWL - and others)
    - identify the inputs and outputs of the script
    - identify the tasks, i.e. the tools being run
    - identify the links, i.e. the data flowing in and out of these tools
    - (not sure about this one) identify and remove some infrastructure-specific details which need to be removed (e.g. if tools are launched with SLURM commands in a bash script, or loaded with Docker or Conda)
- Be able to explain the difference between a CWL tool description and a CWL workflow (description)
    - difference between a tool and the cwl-document that acts as a wrapper for that tool
    - tool wrapper document: describes input/output semantics of command line tool
    - workflow document: describes the input/output of a workflow and specifies the flow of data between tools
- Be able to make understandable and valid names for inputs and outputs (not "input3")
    - always give use case-oriented names or names that describe the content
    - avoid naming them by the tool that produced it or the format
    - so instead of:
        - fastq1
        - bam
        - bed
    - go for:
        - read1
        - aligned_reads
        - regions_of_interest
- Describe all the requirements for running a tool: environment variables, and more
    - https://www.commonwl.org/v1.1/CommandLineTool.html#Runtime_environment
    - Assume the program (baseCommand) is in the system PATH
    - Aren't allowed to change the PATH
    - other environment variable necessary for execution must be set explicitly
    - need a file next to (in the same directory as) another file? use secondaryFiles or InitialWorkDirRequirement
    - Don't hard code the number of threads, use `$(runtime.cores)`
    - Need network access? Not so great, but use NetworkAccess if you must
    - Referencing a file path? Use `type: File` not a string
- Realize that a workflow is a dependency graph
- Be able to include their own script as a step in a CWL workflow
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
- Be able to graph/visualize their workflow, both by hand, and with an automated visualizer
    - use cwlviewer online
    - generate Graphviz diagram using cwltool
    - exercise with the printout of a simple workflow; draw arrows on code; hand draw a graph on another sheet of paper
- Be able to interpret CWL error messages to recognize and fix simple bugs in their workflow code
    - (this is difficult! will need to collect examples first)
- Be able to document purpose, intent, and other factors within their workflow
    - "doc"
    - "label"
    - Workflow level, step level, sub workflow level, tool level, inputs and outputs everywhere.
    - Not required nor recommended to fill these out everywhere! As needed
    - Show example using the CWL viewer
- Know that workflow development can be iterative, can involve sketching, prototypes; that it doesn't have to happen all at once
    - Whiteboard sketch
    - bash script
    - makefile
    - CWL
- How CWL can help you [give credit](https://www.commonwl.org/v1.1/CommandLineTool.html#SoftwarePackage) for all the tools use used
See also https://github.com/common-workflow-language/cwl-utils/blob/master/cwl_utils/cite_extract.py
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
    - Not yet released, expected in Q2 2020
- Run their workflow on a {Sun Grid Engine, LSF, Slurm,..} HPC system
    - explain the benefit and restrictions of HPC systems (high throughput, network restrictions, etc)
    - provide example options (e.g. Toil)
- Convert a GNU Makefile to a CWL workflow

{% include links.md %}
