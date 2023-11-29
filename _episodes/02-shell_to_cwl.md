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
- "A tool description describes the interface to a command line tool."
- "A workflow describes which command line tools to use in one or more steps."
- "A tool descriptor is defined using the `ComandLineTool` class."
- "FIXME: How can we use a tool descriptor in a single step workflow?"
---

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

You should follow the examples in this lesson from your `novice-tutorial-exercises` directory.

~~~
$ cd novice-tutorial-exercises
~~~
{: .language-bash}

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
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmprm65mucw$ echo \
    'Hello world!'
Hello world!
INFO [job echo.cwl] completed success
{}
INFO Final process status is success
~~~
{: .output}

The output displayed above shows that the program has run successfully and its output, `Hello world!`.

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

This block of code contains the `inputs` section of the tool description. This section provides all the inputs that are needed for running this specific tool.
To run this example we will need to provide a string which will be included on the command line. Each of the inputs has a name, to help us tell them apart; this first input has the name : `message_text`.
The field `inputBinding` is one way to specify how the input should appear on the command line.
Here the `position` field indicates at which position the input will be on the command line; in this case the `message_text` value will be the first thing added to the command line (after the `baseCommand`, `echo`).

~~~
outputs: []
~~~
{: .language-yaml}
Lastly the `outputs` of the tool description. This example doesn't have a formal output.
The text is printed directly in the terminal. So an empty YAML list (`[]`) is used as the output.

> ## Script order
> To make the script more readable the `input` field is put in front of the `output` field.
> However CWL syntax requires only that each field is properly defined, it does not require them to be in a particular order.
{: .callout}


> ## Changing input text
>
> What do you need to change to print a different text on the command line?
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

The RNA-seq data from the introduction episode will be used for the first CWL workflow.
The first step of RNA-sequencing analysis is a quality control of the RNA reads using the `fastqc` tool.
This tool is already available to use so there is no need to write a new CWL tool description.

This is the workflow file (`rna_seq_workflow_1.cwl`).

__rna_seq_workflow_1.cwl__
~~~
cwlVersion: v1.2
class: Workflow

inputs:
  rna_reads_fruitfly: File

steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]

outputs:
  quality_report:
    type: File
    outputSource: quality_control/html_file
~~~
{: .language-yaml}

In a __workflow__ the `steps` field must always be present. The workflow tasks or steps that you want to run are listed in this field.
At the moment the workflow only contains one step: `quality_control`. In the next episodes more steps will be added to the workflow.

Let's take a closer look at the workflow. First the `inputs` field will be explained.

~~~
inputs:
  rna_reads_fruitfly: File
~~~
{: .language-yaml}

Looking at the CWL script of the `fastqc` tool, it needs a fastq file as its input. In this example the fastq file consists of *Drosophila melanogaster* RNA reads.
So we call the variable `rna_reads_fruitfly` and it has `File` as its type.
To make this workflow interpretable for other researchers, self-explanatory and sensible variable names are used.

> ## Input and output names
> It is very important to give inputs and outputs a sensible name. Try not to use variable names like `inputA` or `inputB` because others might not understand what is meant by it.
{: .callout}

The next part of the script is the `steps` field.

~~~
steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]
~~~
{: .language-yaml}

Every step of a workflow needs a name, the first step of the workflow is called `quality_control`. Each step needs a `run` field, an `in` field and an `out` field.
The `run` field contains the location of the CWL file of the tool to be run. The `in` field connects the `inputs` field to the `fastqc` tool.
The `fastqc` tool has an input parameter called `reads_file`, so it needs to connect the `reads_file` to `rna_reads_fruitfly`.
Lastly, the `out` field is a list of output parameters from the tool to be used. In this example, the `fastqc` tool produces an output file called `html_file`.

The last part of the script is the `output` field.
~~~
outputs:
  quality_report:
    type: File
    outputSource: quality_control/html_file
~~~
{: .language-yaml}

Each output in the `outputs` field needs its own name. In this example the output is called `quality_report`.
Inside `quality_report` the type of output is defined. The output of the `quality_control` step is a file, so the `quality_report` type is `File`.
The `outputSource` field refers to where the output is located, in this example it came from the step `quality_control` and it is called `html_file`.

When you want to run this workflow, you need to provide a file with the inputs the workflow needs. This file is similar to the `hello_world.yml` file in the previous section.
The input file is called `workflow_input_1.yml`

__workflow_input_1.yml__
~~~
rna_reads_fruitfly:
  class: File
  location: rnaseq/GSM461177_2_subsampled.fastqsanger
  format: http://edamontology.org/format_1930  # FASTA
~~~
{: .language-yaml}

In the input file the values for the inputs that are declared in the `inputs` section of the workflow are provided.
The workflow takes `rna_reads_fruitfly` as an input parameter, so we use the same variable name in the input file.
When setting inputs, the class of the object needs to be defined, for example `class: File` or `class: Directory`.
The `location` field contains the location of the input file, in this case it is a local path, but
we could have directly used the original url `location: https://zenodo.org/record/4541751/files/GSM461177_2_subsampled.fastqsanger`
In this example the last line is needed to provide a format for the fastq file.

Now you can run the workflow using the following command:

~~~
cwltool --cachedir cache rna_seq_workflow_1.cwl workflow_input_1.yml
~~~
{: .language-bash}

~~~
...
Analysis complete for GSM461177_2_subsampled.fastqsanger
INFO [job quality_control] Max memory used: 179MiB
INFO [job quality_control] completed success
INFO [step quality_control] completed success
INFO [workflow ] completed success
{
    "quality_report": {
        "location": "file:///.../GSM461177_2_subsampled.fastqsanger_fastqc.html",
        "basename": "GSM461177_2_subsampled.fastqsanger_fastqc.html",
        "class": "File",
        "checksum": "sha1$e820c530b91a3087ae4c53a6f9fbd35ab069095c",
        "size": 378324,
        "path": "/.../GSM461177_2_subsampled.fastqsanger_fastqc.html"
    }
}
INFO Final process status is success
~~~
{: .output}

> ## Cache Directory
> To save intermediate results for re-use later we use `--cachedir cache`; where `cache` is the directory
> for storing the cache (it can be given any name, here we are just using `cache` for simplicity). You can safely
> delete the `cache` directory anytime, if you need to reclaim the disk space.
{: .callout}


### Exercise
Needs some exercises

- Ask the students to get some information from each report generated for the different data files we've downloaded. This will involve them making simple changes to the yaml configuration file.


{% include links.md %}

