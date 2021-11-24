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
requirements for running that workflow. All CWL document should start with two lines of code:

~~~
cwlVersion: v1.2
class: 
~~~
{: .language-yaml}

The `cwlVersion` string defines which standard of the language is required for the tool or workflow. The most recent version is v1.2.

The `class` field defines what this particular document is. The majority of CWL documents will fall into
 one of two classes: `CommandLineTool`, or `Workflow`. The `CommandLineTool` class is used for
describing the interface for a command-line tool, while the `Workflow` class is used for connecting those
tool descriptions into a workflow. In this lesson we will learn the differences between these two classes,
how to pass data to and from command-line tools and specify working environments for these, and finally
how to use a tool description within a workflow.




## Our first CWL script

To demonstrate the basic requirements for a tool descriptor we will create a CWL description for the popular "Hello world!" demonstration.

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

Next, we create our input file: `hello_world.yml`.

__hello_world.yml__
~~~
message_text: Hello world!
~~~
{: .language-yaml}

Now we can run our CWL script with the reference CWL runner, `cwltool`. This command needs our `.cwl` workflow file along with our input file.

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

The output displayed above shows us that the program has run succesfully and what its output is, `Hello world!`. 

Let's take a look at the `echo.cwl` script in more detail.

As explained above, the first 2 lines are always the same. You define the CWL version and which class you are using. In this example we have a `CommandLineTool`, in particular the `echo` command.
The next line, `baseCommand`, contains the command we want to run (`echo`). 

~~~
inputs:
  message_text:
    type: string
	inputBinding:
	  position: 1
~~~
{: .language-yaml}

This block of code contains the `inputs` section of our workflow. In this section we provide all the inputs that are needed for the workflow. 
In this example a string is needed that will be printed on the command line. We first define an id for our input: `message_text`. 
Then we need to define the type of the input. In our case it will be a `string`. The `InputBinding` describes how the input should appear on the command line.
The `position` field indicates at which position the input will be on the command line, in our example at 1.

~~~
outputs: []
~~~
{: .language-yaml}
Lastly the `outputs` of our workflow. In this example we don't have a formal output. Our text is printed directly on the command line. So we use an empty list (`[]`) as our output. 

> ## Script order
> To make our script more readable we have put the `input` field in front of the `output` field. However CWL syntax requires only that each field is properly defined, it does not require them to be in a particular order. Try changing around the order of fields in our example script, and run the validation step on these to make sure this is true.
{: .callout}


> ## Changing input text
>
> If you would want to print another text on the command line, what do you need to change?
>
> > ## Solution
> >
> > To change the text on the command line, you only need to change the text in the `hello_world.yml` file.
> > 
> > For example:
> > ~~~
> > message_text: Good job!
> > ~~~
> > {: .language-yaml}
> {: .solution}
{: .challenge}


## CWL single step workflow

Next we will be using the RNA-seq data for our first CWL workflow. The first step of the RNA-seq process is quality control of the RNA reads using `fastqc`.
This tool is already available for us to use so we don't have to write a tool descriptor for it.

First we create our workflow file: `rna_seq_workflow.cwl`.

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

In this workflow we use the `steps` field. In this field workflow tasks or steps that are to be run are listed. At the moment we only have a workflow of one step: `fastqc`. 
In the next episode we will add more steps to our workflow. 

Let's take a closer look at the workflow. First we will investigate our `inputs`.

~~~
inputs:
  fastq: File
~~~
{: .language-yaml}

Looking at the `fastqc` tool, it needs a fastq file as its input. So we also need a `File` for our inputs. To make this workflow interpretable for other researchers, we use self-explanatory variable names.
In our case, we use `fastq` as our input name.

> ## Input and output names
> It is very important to give your inputs and outputs a sensible name. Try to not use variable names like `inputA` or `inputB` because others might not understand what is meant by it.
{: .callout}

The next part of the script is the `steps` field. Workflows need a `steps`field, in which are listed the workflow tasks or steps that are to be run.

~~~
steps:
  fastqc:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
	in:
	  reads_file: fastq
    out: [html_file]
~~~
{: .language-yaml}

Every step of a workflow needs an name, the first step of our workflow is called `fastqc`. Each of the steps needs a `run` field, `in` field and `out` field. 
The `run` field contains the location of the cwl file of the tool we want to run. The `in` field connects the `inputs` field to the `fastqc` tool we want to run.
The `fastqc` tool has an input parameter callen `reads_file`, so we need to connect the `reads_file_fi` to `fastq`. 
Lastly, the `out` field is a list of output parameters from the tool we want to use. In our example, the `fastqc` tool produces an output file called `html_file`.

The last part of the script is the `output` field.
~~~
outputs: 
  qc_html:
    type: File
	outputSource: fastqc/html_file
~~~
{: .language-yaml}

For our output, we need to declare the type of output and what parameter has the output value. The output of the `fastqc` step is a file, so we define `qc_html` type as `File`.
The `outputSource` field refers to where the output is located, in our example it came from the step `fastqc` and it is called `html_file`.

To be able to run this workflow, we need to provide a file with the inputs. This file is similar to the `hello_world.yml` file in the previous section. 
We will call our input file `workflow_input.yml`

__workflow_input.yml__
~~~
fastq:
  class: File
  location: rnaseq/raw_fastq/Mov10_oe_1.subset.fq
  format: http://edamontology.org/format_1930
~~~
{: .language-yaml}

In the input file we provide the values for the inputs declared in the `inputs` section of our workflow. Our workflow takes `fastq` as an input parameter.
When setting inputs, we need to define the class of the object, for example `class: File` or `class: Directory`. The last line is needed to provide a format for the fastq file.

Now we can finally run our workflow:

~~~
cwltool rna_seq_workflow.cwl workflow_input.yml
~~~
{: .language-bash}


{% include links.md %}




