---
title: "CWL vs Shell Workflows"
teaching: 0
exercises: 0
questions:
- "What is the difference between a CWL tool description and a CWL workflow?"
- "How can we create a tool descriptor?"
- "How can we use this in a single step workflow?"
objectives:
- "dataflow objectives:"
- "explain the difference between a CWL tool description and a CWL workflow"
- "describe the relationship between a tool and its corresponding CWL document"
- "exercise good practices when naming inputs and outputs"
- "Be able to make understandable and valid names for inputs and outputs (not 'input3')"
- "describing requirements objectives:"
- "identify all the requirements of a tool and define them in the tool description"
- "use `runtime` parameters to access information about the runtime environment"
- "define environment variables necessary for execution"
- "use `secondaryFiles` or `InitialWorkDirRequirement` to access files in the same directory as another referenced file"
- "use `$(runtime.cores)` to define the number of cores to be used"
- "use `type: File`, instead of a string, to reference a filepath"
- "converting shell to cwl objectives:"
- "identify tasks, and data links in a script"
- "recognize loops that can be converted into scatters"
- "finding and reusing existing CWL command line tool descriptions"
- "capturing outputs objectives:"
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

CWL workflows are defined in a YAML script, containing the workflow and the requirements for running that workflow. All CWL scripts should start with two lines of code:
~~~
cwlVersion: v1.2
class:
~~~
{: .language-yaml}
The `cwlVersion` string defines which standard of the language is required for the tool or workflow. The most recent version is v1.2, and defaulting to this will enable your scripts to use all versions of the language, though some workflow engines which are not up-to-date may not run the script. This is, however, a hurdle to be tackled when you reach it.

The `class` field defines what this particular script is. The majority of CWL scripts will fall into one of two classes: `CommandLineTool`, or `Workflow`. The former class is used for describing the interface for a command-line tool, while the latter class is used for collecting those tool descriptions into a workflow. In this lesson we will learn the differences between these two classes, how to pass data to and from command-line tools and specify working environments for these, and finally how to use a tool descriptor within a workflow.





## Tool Descriptors

To demonstrate the basic requirements for a tool descriptor we will recreate the standard hello world example. __Note: replace this with a domain specific (but similar complexity) example!__ This is the shell `echo` command that we will use:
~~~
$ echo 'hello World!'
~~~
{: .language-bash}
~~~
hello World!
~~~
{: .output}

Create a file, `echo.cwl`, to contain your CWL example for this:
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: [echo, 'hello World!']
~~~
{: .language-yaml}
We present `baseCommand` as a two item list containing the command and the input string. CWL will combine these two items (in the order given) to make the full command when the script is run. This is not a complete tool descriptor yet, however - to find out what is missing we can use `cwl-runner` to validate the script:
~~~
$ cwl-runner --validate echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
ERROR Tool definition failed validation:
echo.cwl:1:1: "outputs" section is not valid.
~~~
{: .output}
`cwl-runner` has noted that we are missing an `outputs` section, so we will add this to our script:
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: [echo, 'hello World!']
outputs: []
~~~
{: .language-yaml}
note that we are using an empty list `[]` here, as we do not want to capture any output for the moment. We will now validate the script again:
~~~
$ cwl-runner --validate echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
ERROR Tool definition failed validation:
echo.cwl:1:1: Object `echo.cwl` is not valid because
                        tried `CommandLineTool` but
                          missing required field `inputs`
~~~
{: .output}
`cwl-runner` has noted that we are missing an `inputs` field, so we will add this also to our script (again as an empty list):
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: [echo, 'hello World!']
inputs: []
outputs: []
~~~
{: .language-yaml}
We will now validate the script again:
~~~
$ cwl-runner --validate echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
echo.cwl is valid CWL.
~~~
{: .output}
Finally we have a valid CWL tool descriptor, so we will run this using `cwl-runner`:
~~~
cwl-runner echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmpwvj2kdvw$ echo \
    'hello World!'
hello World!
INFO [job echo.cwl] completed success
{}
INFO Final process status is success
~~~
{: .output}
Our script has been run successfully!

> ## Location, Location, Location
> Note the string after `INFO [job echo.cwl]` shows the location `/private/tmp/docker_tmpwvj2kdvw` in our example, but will show a different randomly named, and temporary, directory when you run this example. CWL will always create a separate, temporary, working directory for running each tool instance. This ensures that the run-time environment for each tool instance is well controlled, and does not contain anything left behind by another tool.
{: .callout}

> ## Script order
> To make our script more readable we have put the `input` field in front of the `output` field. However CWL syntax requires only that each field is properly defined, it does not require them to be in a particular order. Try changing around the order of fields in our example script, and run the validation step on these to make sure this is true.
{: .callout}

So far our script is rather limited, with no inputs specified, and the string that we are printing out has been merged into the `baseCommand`. We will now split out the input string, so that we can make this tool more flexible.

We remove the `hello World!` string from the `baseCommand` (where it should not have been in the first place...), and create an `inputs` item which we will call `message_text`:
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: echo
inputs:
  message_text:
    type: string
    default: 'hello World!'
    inputBinding:
      position: 1

outputs: []
~~~
{: .language-yaml}
We set the type of `message_text` to string, and set the `inputBinding` `position` (defining where the input item appears after the `baseCommand`) as 1. We also give a default value, `hello World!`, for this item, which will be used if the item is not defined within an input file.

We can now validate, and then run, this tool again:
~~~
$ cwl-runner --validate echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
echo.cwl is valid CWL.
~~~
{: .output}
~~~
$ cwl-runner echo.cwl
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmprm65mucw$ echo \
    'hello World!'
hello World!
INFO [job echo.cwl] completed success
{}
INFO Final process status is success
~~~
{: .output}

The script is now ready to accept an input from us. This we will put in another YAML file (`moon.yml`):
~~~
message_text: 'hello Moon!'
~~~
{: .language-yaml}

And then run:
~~~
$ cwl-runner echo.cwl moon.yml
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmpu7z20wc7$ echo \
    'hello Moon!'
hello Moon!
INFO [job echo.cwl] completed success
{}
INFO Final process status is success
~~~
{: .output}
Note that we have not yet captured the output from the command. We can do this by adding an outputs item to the script:
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: echo
inputs:
  message_text:
    type: string
    default: 'hello World!'
    inputBinding:
      position: 1

outputs:
  message_out:
    type: stdout

~~~
{: .language-yaml}
Here we have added the `message_out` item, which has been given type `stdout` (as we want to capture the output at the command line, rather than any particular files).

Now we run the script:
~~~
$ cwl-runner echo.cwl moon.yml
~~~
{: .language-bash}
~~~
INFO .../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmp7k0bqeg5$ echo \
    'hello Moon!' > /private/tmp/docker_tmp7k0bqeg5/9611f21693018fe4ce4bf1f3884e47dae2ce3aab
INFO [job echo.cwl] completed success
{
    "message_out": {
        "location": "file:///.../9611f21693018fe4ce4bf1f3884e47dae2ce3aab",
        "basename": "9611f21693018fe4ce4bf1f3884e47dae2ce3aab",
        "class": "File",
        "checksum": "sha1$d4413a97a36059e8855168ac7939a4cb5d4da9c9",
        "size": 12,
        "path": "/.../9611f21693018fe4ce4bf1f3884e47dae2ce3aab"
    }
}
INFO Final process status is success
~~~
{: .output}
Note that the output has been saved as a file called `9611f21693018fe4ce4bf1f3884e47dae2ce3aab` (not `message_out`, which is only the output variable name) in this instance. When you run this script the file name will be different. You can open this text file with a text editor to confirm that it contains the expected message.

It is not very user-friendly to have our script return a randomly named file each time, so we will make use of the `stdout` field to specify the name of the text file that we want standard output to be captured to:
~~~
cwlVersion: v1.2
class: CommandLineTool

baseCommand: echo
inputs:
  message_text:
    type: string
    default: 'hello World!'
    inputBinding:
      position: 1

stdout: output.txt

outputs:
  message_out:
    type: stdout
~~~
{: .language-yaml}

Running the script will produce this output:
~~~
$ cwl-runner echo.cwl moon.yml
~~~
{: .language-bash}
~~~
INFO /.../cwl-runner 3.0.20200807132242
INFO Resolved 'echo.cwl' to 'file:///.../echo.cwl'
INFO [job echo.cwl] /private/tmp/docker_tmp5iazb06g$ echo \
    'hello Moon!' > /private/tmp/docker_tmp5iazb06g/output.txt
INFO [job echo.cwl] completed success
{
    "message_out": {
        "location": "file:///.../output.txt",
        "basename": "output.txt",
        "class": "File",
        "checksum": "sha1$d4413a97a36059e8855168ac7939a4cb5d4da9c9",
        "size": 12,
        "path": "/.../output.txt"
    }
}
INFO Final process status is success
~~~
{: .output}
Note now that the file returned is called `output.txt`, but it has the same contents as the previous, randomly named, file.




## Example Exercises

Use https://github.com/bcosc/fast_genome_variants/blob/main/README.md to create a `CommandLineTool`

> ## Exercise 1
>
> Create the baseCommand for running the joint_haplotype caller using the fast_genome_variants README.
>
> > ## Solution
> > The base command should use the path to the binary and the type of variants you're calling.
> >
> > ~~~
> > baseCommand: [fgv, joint_haplotype]
> > ~~~
> > {: .language-yaml }
> {: .solution}
{: .challenge}

> ## Exercise 2:
>
> When working in a cloud environment, you need to specify what machine type you would like to run on. Which means the job has to have specific parameters describing the RAM, Cores and Disk space (for both temporary and output files) it requires.
>
> Create the `ResourceRequirements` field for running 2 BAMs for
> the `fgv joint_haplotype` command.
>
> > ## Solution:
> >
> > ~~~
> > requirements:
> >   ResourceRequirement:
> >     ramMin: 4000
> >     coresMin: 2
> > ~~~
> > {: .language-yaml }
> >
> > FGV requires 2 GiB of memory for each bam input,
> > and the unit for `ramMin` is in MiB,
> > so we need approximately 4000 MiB to meet the requirement.
> > FGV also requires 1 core for each BAM, so here we ask for at least 2 cores.
> >
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> 1. Create the `input` field for running `fgv_joint_haplotype`
> 2. Add an optional flag for calling a GVCF output
> 3. Add a string input for intervals `chr2:1-10000`
> 4. Add an output name.
>
> > ## Solution:
> >
> > ~~~
> > inputs:
> >   bam:
> >     type: File[]
> >     inputBinding:
> >       position: 1
> >       prefix: -bam
> >     secondaryFiles:
> >       - .bai
> >   gvcf:
> >     type: boolean
> >     inputBinding:
> >       position: 2
> >       prefix: -gvcf
> >   interval:
> >     type: string
> >     inputBinding:
> >       position: 3
> >   output_name:
> >     type: string
> >     inputBinding:
> >       position: 4
> >       prefix: -o
> > ~~~
> > {: .language-yaml }
> {: .solution}
{: .challenge}

> ## Exercise 4:
>
> Create the output variable for the CommandLineTool and name it output_vcf.
>
> > ## Solution:
> >
> > ~~~
> > outputs:
> >   output_vcf:
> >     type: File
> >     outputBinding:
> >       glob: $(inputs.output_name)
> > ~~~
> > {: .language-yaml }
> {: .solution}
{: .challenge}

> ## Exercise 5:
>
> TODO
>
> > ## Solution:
> >
> >
> {: .solution}
{: .challenge}

## Capturing Output

> ## Exercise 1
>
> Using this `CommandLineTool` description,
> what files would be the output of the tool?
>
> ~~~
> cwlVersion: v1.0
> class: CommandLineTool
>
> baseCommand: [bwa, mem]
>
> inputs:
>   reference:
>     type: File
>     inputBinding:
>       position: 1
>   fastq_reads:
>     type: File[]
>     inputBinding:
>       position: 2
>
> stdout: output.sam
>
> outputs:
>   output:
>     type: File
>     outputBinding:
>       glob: output.sam
> ~~~
> {: .language-yaml }
>
>
> > ## Solution
> >
> > `output.sam` will be the only file outputted.
> Only files explicitly stated in the outputs field will be included
> in the output of the step.
> {: .solution }
{: .challenge }

> ## Exercise 2
>
> Your colleague tells you to run `fastqc`,
> which creates several files describing the quality of the data.
> For now, let's assume the tool creates three files:
>
> - `final_report_fastqc.html`
> - `final_figures_fastqc.zip`
> - `supplemental_figures_fastqc.html`
>
> Create a CWL `outputs` field using a `File` array that
> captures all the fastqc files in a single output variable.
>
> > ## Solution
> >
> > ~~~
> > outputs:
> >   output:
> >     type: File[]
> >     outputBinding:
> >       glob: "*fastqc*"
> > ~~~
> > {: .language-yaml }
> {: .solution }
>
> Actually, `fastqc` may create more than 3 of these files,
> depending on the input parameters you give it,
> it may create a `results` directory that contains additional files such as
> `results/fastqc_per_base_content.html` and
> `results/fastqc_per_base_gc_content.html`.
>
> Create a CWL `outputs` field that captures the
> `results/fastqc_per_base_content.html` and
> `results/fastqc_per_base_gc_content.html` in separate output variables.
>
> > ## Solution
> >
> > ~~~
> > outputs:
> >   per_base_content:
> >     type: File
> >     outputBinding:
> >       glob: "results/fastqc_per_base_content.html"
> >   per_base_gc_content:
> >     type: File
> >     outputBinding:
> >       glob: "results/fastqc_per_base_gc_content.html"
> > ~~~
> > {: .language-yaml }
> {: .solution }
>
> Finally, instead of explicitly defining each file to be captured,
> create a CWL `outputs` field that captures the entire `results` directory.
>
> > ## Solution
> >
> > ~~~
> > outputs:
> >   results:
> >     type: Directory
> >     outputBinding:
> >       glob: "results"
> > ~~~
> > {: .language-yaml }
> {: .solution }
{: .challenge }

> ## Exercise 3
>
> Since `fastqc` can be unpredictable in its outputs and file naming, create a CWL outputs field using a `Directory` that captures all the files in a single output variable.
>
> > ## Solution
> >
> > ~~~
> > outputs:
> >   output:
> >     type: Directory
> >     outputBinding:
> >       glob: .
> > ~~~
> > {: .language-yaml }
> {: .solution }
{: .challenge }

> ## Exercise 4
>
> Your colleague says that he is running `samtools index` in CWL,
> but the index is not being outputted.
> Fix the following CWL to have output the index along with
> the `bam` as a `secondaryFile`.
>
> ~~~
> cwlVersion: v1.0
> class: CommandLineTool
>
> requirements:
>   InitialWorkDirRequirement:
>     listing:
>       - $(inputs.bam)
>
> baseCommand: [samtools, index]
>
> inputs:
>   bam:
>     type: File
>     inputBinding:
>       position: 1
>       valueFrom: $(self.basename)
>
> outputs:
>   output_bam_and_index:
>     type: File
>     outputBinding:
>       glob: $(inputs.bam.basename)
> ~~~
> {: .language-yaml }
>
> > ## Solution
> >
> > ~~~
> > cwlVersion: v1.0
> > class: CommandLineTool
> >
> > requirements:
> >   InitialWorkDirRequirement:
> >     listing:
> >       - $(inputs.bam)
> >
> > baseCommand: [samtools, index]
> >
> > inputs:
> >   bam:
> >     type: File
> >     inputBinding:
> >       position: 1
> >       valueFrom: $(self.basename)
> >
> > outputs:
> >   output_bam_and_index:
> >     type: File
> >     secondaryFiles:
> >       - .bai
> >     outputBinding:
> >       glob: $(inputs.bam.basename)
> > ~~~
> > {: .language-yaml }
> {: .solution }
{: .challenge }

> ## Exercise 5
>
> What if `InitialWorkDirRequirement` was not used,
> and the index file was created where the input bam was located?
> How would you capture the output?
> Create the `outputs` field using the same CWL in exercise 4.
>
> > ## Solution
> >
> > ~~~
> > outputs:
> >   output_bam_and_index:
> >     type: File
> >     secondaryFile:
> >       - .bai
> >     outputBinding:
> >       glob: $(inputs.bam)
> > ~~~
> > {: .language-yaml }
> {: .solution }
{: .challenge }


{% include links.md %}




