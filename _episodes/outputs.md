---
title: "Capturing output"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "explain that only files explicitly mentioned in a description will be included in the output of a step/workflow"
- "implement bulk capturing of all files produced by a step/workflow for debugging purposes"
- "use STDIN and STDOUT as input and output"
- "capture output written to a specific directory, the working directory, or the same directory where input is located"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__define the files that will be included as output of a workflow__.

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
