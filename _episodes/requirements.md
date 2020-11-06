---
title: "Describing requirements"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "identify all the requirements of a tool and define them in the tool description"
- "use `runtime` parameters to access information about the runtime environment"
- "define environment variables necessary for execution"
- "use `secondaryFiles` or `InitialWorkDirRequirement` to access files in the same directory as another referenced file"
- "use `$(runtime.cores)` to define the number of cores to be used"
- "use `type: File`, instead of a string, to reference a filepath"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__describe all the requirements for running a tool__


> ## Exercise 1:
>
> For each of these variables, is it required in a CommandLineTool?
>
> A. inputs
> B. outputs
> C. arguments
> D. baseCommand
> E. requirements
> F. hints
> > ## Solution
> > Just like a normal shell script, you need inputs, outputs, and a command to run. So A, B, C are definitely needed. D is sometimes needed, this is where you put the binary invocation of the tool. E and F are necessary for adding environment help, such as machine and docker requirements.
> {: .solution}
{: .challenge}

> ## Exercise 2:
> 
> When working in a cloud environment, you need to specify what machine type you would like to run on. Which means the job has to have specific RAM, Cores and Disk space parameters.
>
> What ResourceRequirements can you specify in a CommandLineTool to satisfy the machine requirements?
>
> > ## Solution:
> >
> > coresMin and coresMax
> > ramMin and ramMax
> > tmpdirMin and tmpdirMax
> > outdirMin andoutdirMax
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> Input arguments for a command in a shell script are typically strings that can be classified as file paths, booleans, integers, etc. When you have multiple inputs of a same type, you classify that as an array.
>
> What input types would you use for these inputs?
>
> A. BAM
> B. VCF
> C. Sample name
> D. 23 Chromosomes
> E. Flag for turning on paired-end mode in bwa mem
> F. Cohort of sample paired-end FASTQs
>
> > ## Solution:
> > A. BAM (File)
> > B. VCF (File)
> > C. Sample name (String)
> > D. 23 Chromosomes (String Array)
> > E. Flag for turning on paired-end mode in bwa mem (boolean)
> > F. Cohort of sample paired-end FASTQs (Array of File Arrays)
> >
> > The trickiest one is the paired FASTQs, we need to be able to distinguish each sample pair, so using a nested array works best. # TODO Add picture example
> {: .solution}
{: .challenge}

> ## Exercise 4:
> 
> In a typical linux environment, when you go into a directory, given that you have the correct permissions, you can read every file. When using CWL, each file given to a CommandLineTool must be explicitly stated, or it will not be found by the command. When grouping files together, such as a bam and its index, you should use the secondaryFiles variable to pass the index along with the bam variable.
>
> Fix this code for capturing the correct file in this CommandLineTool. Samtools index will take a BAM and create an index (.bai) file. The output should be the bam and index together.
>
>
> baseCommand: [samtools, index]
> inputs:
>   bam: File
> outputs:
>   bam_output_and_index:
>     type:
>     outputBinding:
>       glob:
>
> > ## Solution:
> > baseCommand: [samtools, index]
> > inputs:
> >   bam: File
> > outputs:
> >   bam_output_and_index:
> >     type: File
> >     outputBinding:
> >       glob: $(inputs.input.basename)
> >     secondaryFiles:
> >       - .bai
> {: .solution}
{: .challenge}


{% include links.md %}
