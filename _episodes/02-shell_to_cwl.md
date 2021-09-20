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
By the end of this episode,
learners should be able to
__explain how a workflow document describes the input and output of a workflow and the flow of data between tools__
and __describe all the requirements for running a tool__
and __define the files that will be included as output of a workflow__
and __convert a shell script into a CWL workflow__.


## Tool Descriptors

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




