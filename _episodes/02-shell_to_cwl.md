---
title: "CWL and Shell Tools"
teaching: 0
exercises: 0
questions:
- "What is the difference between a CWL tool description and a CWL workflow?"
- "How can we create a tool descriptor?"
- "How can we use this in a single step workflow?"
objectives:

- "describe the relationship between a tool and its corresponding CWL document"
- "exercise good practices when naming inputs and outputs"
- "understand how to reference files for input and output"
- "explain that only files explicitly mentioned in a description will be included in the output of a step/workflow"
- "implement bulk capturing of all files produced by a step/workflow for debugging purposes"
- "use STDIN and STDOUT as input and output"
- "capture output written to a specific directory, the working directory, or the same directory where input is located"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

> ## learning objectives
> By the end of this episode,
> learners should be able to
> __explain how a workflow document describes the input and output of a workflow and the flow of data between tools__
> and __describe all the requirements for running a tool__
> and __define the files that will be included as output of a workflow__.
{: .callout}

CWL workflows are written in the YAML syntax. This [short tutorial](https://www.commonwl.org/user_guide/yaml/)
explains the parts of YAML used in CWL. A CWL document contains the workflow and the
requirements for running that workflow. All CWL documents should start with two lines of code:

~~~
cwlVersion: v1.2
class: 
~~~
{: .language-yaml}

The `cwlVersion` string defines which standard of the language is required for the tool or workflow. The most recent version is v1.2.

The `class` field defines what this particular document is. The majority of CWL documents will fall into
one of two classes: `CommandLineTool`, or `Workflow`. The `CommandLineTool` class is used for
describing the interface for a command-line tool, while the `Workflow` class is used for connecting those
tool descriptions into a workflow. In this lesson the differences between these two classes are explained,
how to pass data to and from command-line tools and specify working environments for these, and finally
how to use a tool description within a workflow.

## Our first CWL script

To demonstrate the basic requirements for a tool descriptor a CWL description for the popular "Hello world!" demonstration will be examined.

__echo.cwl__
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: echo

inputs:
  message_text:
    type: string
	inputBinding:
	  position: 1

outputs: []
~~~
{: .language-yaml}

Next, the input file: `hello_world.yml`.

__hello_world.yml__
~~~
message_text: Hello world!
~~~
{: .language-yaml}

We will use the reference CWL runner, `cwltool` to run this CWL document (the `.cwl` workflow file) along with the `.yml` input file.

~~~
cwltool echo.cwl hello_world.yml
~~~
{: .language-bash}

~~~
INFO .../cwltool
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmprm65mucw$ echo \
    'Hello world!'
Hello world!
INFO [job echo.cwl] completed success
{}
INFO Final process status is success
~~~
{: .output}

The output displayed above shows that the program has run succesfully and its output, `Hello world!`. 

Let's take a look at the `echo.cwl` script in more detail.

As explained above, the first 2 lines are always the same, the CWL version and the class of the script are defined. 
In this example the class is `CommandLineTool`, in particular the `echo` command.
The next line, `baseCommand`, contains the command that will be run (`echo`). 

~~~
inputs:
  message_text:
    type: string
	inputBinding:
	  position: 1
~~~
{: .language-yaml}

This block of code contains the `inputs` section of the workflow. This section provides all the inputs that are needed for the workflow. 
In this example a string is needed that will be printed on the command line. An id is defined first for the input: `message_text`. 
Then the type of the input needs to be defined. In this case it will be a `string`. The `inputBinding` describes how the input should appear on the command line.
The `position` field indicates at which position the input will be on the command line, in this example it is at the first position.

~~~
outputs: []
~~~
{: .language-yaml}
Lastly the `outputs` of the workflow. This example doesn't have a formal output.
The text is printed directly in the terminal. So an empty YAML list (`[]`) is used as the output. 

> ## Script order
> To make the script more readable the `input` field is put in front of the `output` field. 
> However CWL syntax requires only that each field is properly defined, it does not require them to be in a particular order. 
{: .callout}


> ## Changing input text
>
> What do you need to change to print another text on the command line?
>
> > ## Solution
> >
> > To change the text on the command line, you only have to change the text in the `hello_world.yml` file.
> > 
> > For example:
> > ~~~
> > message_text: Good job!
> > ~~~
> > {: .language-yaml}
> {: .solution}
{: .challenge}


## CWL single step workflow

Next the RNA-seq data will be used for the first CWL workflow. The first step of RNA-sequensing analysis is a quality control of the RNA reads using `fastqc`.
This tool is already available to use so there is no need to write a new CWL tool description.

This is the workflow file (`rna_seq_workflow.cwl`).

__rna_seq_workflow.cwl__
~~~
clwVersion: v1.2
class: Workflow

inputs:
  fastq: File
  
steps:
  fastqc:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
	in:
	  reads_file: fastq
    out: [html_file]

outputs: 
  qc_html:
    type: File
	outputSource: fastqc/html_file
~~~
{: .language-yaml}

In a __workflow__ the `steps` field must always be present. The workflow tasks or steps that you want to run are listed in this field. 
At the moment the workflow only contains one step: `fastqc`. In the next episodes more steps will be added to the workflow. 

Let's take a closer look at the workflow. First the `inputs` field will be explained.

~~~
inputs:
  fastq: File
~~~
{: .language-yaml}

Looking at the CWL script of the `fastqc` tool, it needs a fastq file as its input. So this workflow also needs a `File` for its inputs. 
To make this workflow interpretable for other researchers, self-explanatory and sensible variable names are used.
In this case, `fastq` is used as the input name for the fastq file.

> ## Input and output names
> It is very important to give inputs and outputs a sensible name. Try not to use variable names like `inputA` or `inputB` because others might not understand what is meant by it.
{: .callout}

The next part of the script is the `steps` field. 

~~~
steps:
  fastqc:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
	in:
	  reads_file: fastq
    out: [html_file]
~~~
{: .language-yaml}

Every step of a workflow needs an name, the first step of the workflow is called `fastqc`. Each step needs a `run` field, an `in` field and an `out` field. 
The `run` field contains the location of the CWL file of the tool to be run. The `in` field connects the `inputs` field to the `fastqc` tool. 
The `fastqc` tool has an input parameter called `reads_file`, so it needs to connect the `reads_file` to `fastq`. 
Lastly, the `out` field is a list of output parameters from the tool to be used. In this example, the `fastqc` tool produces an output file called `html_file`.

The last part of the script is the `output` field.
~~~
outputs: 
  qc_html:
    type: File
	outputSource: fastqc/html_file
~~~
{: .language-yaml}

Each output in the `outputs` field needs its own name. In this example the output is called `qc_html`. 
Inside `qc_html` the type of output is defined. The output of the `fastqc` step is a file, so the `qc_html` type is `File`. 
The `outputSource` field refers to where the output is located, in this example it came from the step `fastqc` and it is called `html_file`.

When you want to run this workflow, you need to provide a file with the inputs the workflow needs. This file is similar to the `hello_world.yml` file in the previous section. 
The input file is called `workflow_input.yml`

__workflow_input.yml__
~~~
fastq:
  class: File
  location: rnaseq/raw_fastq/Mov10_oe_1.subset.fq
  format: http://edamontology.org/format_1930
~~~
{: .language-yaml}

In the input file the values for the inputs that are declared in the `inputs` section of the workflow are provided. The workflow takes `fastq` as an input parameter.
When setting inputs, the class of the object needs to be defined, for example `class: File` or `class: Directory`. The `location` field contains the location of the input file.
The last line is needed in this example to provide a format for the fastq file.

Now you can run the workflow using the following command:

~~~
cwltool rna_seq_workflow.cwl workflow_input.yml
~~~
{: .language-bash}

### Exercise
Needs some exercises



{% include links.md %}




