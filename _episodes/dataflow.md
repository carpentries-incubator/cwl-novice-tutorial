---
title: "CWL workflow descriptions"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "explain the difference between a CWL tool description and a CWL workflow"
- "describe the relationship between a tool and its corresponding CWL document"
- "exercise good practices when naming inputs and outputs"
- "Be able to make understandable and valid names for inputs and outputs (not 'input3')"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__explain how a workflow document describes the input and output of a workflow and the flow of data between tools__

{% include links.md %}



CWL Workflows are about chaining different steps.

A workflow is the orchestration of the individual steps.



## Definitions

> A CWL Tool description is a document that enables to run a command line tool, using a CWL runner (i.e. any type of workflow engine that is CWL compatible)

> A CWL Workflow description is a particular chaining of steps (running CWL Tools or CWL Workflows) that enables a their execution with a CWL runner in an orchestrated way.


This is what you see in a header of a CWL Tool Description

(link: https://github.com/common-workflow-library/bio-cwl-tools/blob/release/bwa/BWA-Index.cwl)

```
#!/usr/bin/env cwl-runner
cwlVersion: v1.0
class: CommandLineTool

requirements:
  DockerRequirement:
    dockerPull: "quay.io/biocontainers/bwa:0.7.17--ha92aebf_3"
  InlineJavascriptRequirement: {}

inputs:
  InputFile:
    type: File
    format: edam:format_1929  # FASTA
    inputBinding:
      position: 200
    
  IndexName:
    type: string
    inputBinding:
      prefix: "-p"
      valueFrom: $(self + ".bwt")

#Optional arguments

  algoType:
    type: 
      - "null"
      - type: enum
        symbols:
        - is
        - bwtsw
    inputBinding:
      prefix: "-a"

baseCommand: [bwa, index]

outputs:

  index:
    type: File
    outputBinding:
      glob: $(inputs.IndexName)

$namespaces:
  edam: http://edamontology.org/
$schemas:
  - http://edamontology.org/EDAM_1.18.owl
```


The key element is the first line (`class: CommandLineTool`) - this means that this is something that will be executed as a command line tool.


(link to an example workflow: https://github.com/bioexcel/biobb-wf-md-setup-protein-cwl/blob/master/protein_md.cwl)


```
cwlVersion: v1.0
class: Workflow
label: Example of setting up a simulation system
doc: |
  Common Workflow Language example that illustrate the process of setting up a
  simulation system containing a protein, step by step, using the BioExcel
  Building Blocks library (biobb). The particular example used is the Lysozyme
  protein (PDB code 1AKI).

inputs:
  step1_pdb_name: string
  step1_pdb_config: string

```

The key element here is (again) class (`class: Workflow`) - it implies that this is an orchestration, that can be executed as a workflow by a CWL-supported engine.


**Exercise #0**

Shawn is looking at existing CWL Tools and Workflows to see how to write them.  He knows that the "class" variable specifies whether a CWL description is a workflow or a tool. Looking at the headers below, can you tell whether they are a description of a tool or a workflow?


*TODO*: Add some headers here




**Exercise #1**

Jorge is designing a workflow A that will be utilizing the command line tools B, C and D. What would be the "class" listed in each of the corresponding files A, B, C and D.

_Solution_:

- B, C, and D: `class: CommandLineTool`
- A: `class: Workflow`


**Exercise #2**

Maria finds a workflow description that runs two successive workflows as steps. She wonders if this can actually run. Can you tell?

_Solution_:

A given workflow description can rely only on other workflows, i.e. have no direct calls to specific tools itself. For this reason, the workflow that Maria found can run without any issues.

**Exercise #3** 

Maria is reading a CWL file, but is unsure whether it contains a CWL Tool description. Can you see a Tool description in there?

_Solution_:

Yes, it does contain a Tool description. CWL Workflows can call CWL tool descriptions, but they can also have Tool descriptions embedded inside them.  Embedding Tool descriptions inside of workflows is valid CWL, however this limits the reusability of the Tool descriptions, decreases the readability, and makes debugging CWL harder.



(_Note, to be removed: the following address the questions about the relationship between a tool and its corresponding CWL document_)

**Exercise #4**

Shawn wants to use a <a_tool> in his workflow.  He has found a CWL description for the tool but it does not have all the options he expected. What may be the reason for this:

a) The CWL description is for an older version of <a_tool>.
b) The CWL description was never finished.
c) The author of the CWL description did not need those options when they wrote the CWL Description
Why might this be?

_Solution_:

All three options may be valid.  It could be that this is for an older version of <a_tool>, or that the author did not finish writing the full CWL description.

What is important to remember when using and creating CWL Tools is that they provides a clear interface to call an executable from a CWL runner. A CWL Tool may not provide access to the full features of an executable, often they only contain a subset of options.
<!--.. it may only be those that the creator of the CWL tool needed. -->

As a result, when looking for an existing CWL Tool for the software you want to run, you might find more than one. This is perfectly normal, and the choice between these will be up to you and the requirements of your project. There are best practice-compliant repositories established for such descriptions (e.g. https://github.com/common-workflow-library/bio-cwl-tools), but it should be clear that you can use a description that best fits your own needs.

<!--A single command line tool may correspond to multiple CWL Tool Description files; each description file may have slightly different aspects of the same tool (such as parameters that can be set, number of input/output files etc).-->


(_Note, to be removed: the following address valid names and good practices for input and output names_)


Names for inputs and outputs, just like the names of variables in a program, should be unique, and as self-documenting as possible (have sensible/meaningful names)

- valid names, are any character forbidden. 


**Exercise 5**:

Which of the following are valid choices for input and output names. And which one are actually well-designed as well.

- 2ndInput
- !WrongOne
- This is the correct input
- Output2
- BAMFile
- MetadataCSVFileInputForToolHiSat2AndRNASeqSundayEveningVersionDoNotModifyOrContactMe


_Solution_:

- starts with a number
- starts with a symbol
- has spaces/gaps
- it is valid, but does not convey any useful information
- Valid, and well designed
- valid, but too much information


Regardless of naming conventions, a key aspect to always keep in mind is the fact that any input and/or output that is expected to appear (or be used respectively) needs to be explicitly defined in the corresponding CWL files (the tools and the workflow description files).

