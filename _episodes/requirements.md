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

Use https://github.com/bcosc/fast_genome_variants/blob/main/README.md to create a CommandLineTool

> ## Exercise 1:
>
> Create the baseCommand for running the joint_haplotype caller using the fast_genome_variants README.
>
> > ## Solution
> > The base command should use the path to the binary and the type of variants you're calling.
> > 
> >  baseCommand: [fgv, joint_haplotype]
> > 
> {: .solution}
{: .challenge}

> ## Exercise 2:
> 
> When working in a cloud environment, you need to specify what machine type you would like to run on. Which means the job has to have specific parameters describing the RAM, Cores and Disk space (for both temporary and output files) it requires.
>
> Create the ResourceRequirements field for running 2 BAMs for the fgv joint_haplotype command.
>
> > ## Solution:
> >
> > requirements:
> >   ResourceRequirement:
> >     ramMin: 4000
> >     coresMin: 2
> > 
> > FGV requires 2 GiB of memory for each bam input, and the unit for ramMin is in MiB, so we need approximately 4000 MiB to meet the requirement. FGV also requires 1 core for each BAM, so here we ask for at least 2 cores.
> >
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> Create the input field for running fgv_joint_haplotype and also add an optional flag for calling a GVCF output. Also add a string input for intervals chr2:1-10000 and an output name.
>
> > ## Solution:
> >
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
> > 
> {: .solution}
{: .challenge}

> ## Exercise 4:
> 
> Create the output variable for the CommandLineTool and name it output_vcf.
>
> > ## Solution:
> >
> > outputs:
> >   output_vcf:
> >     type: File
> >     outputBinding:
> >       glob: $(inputs.output_name)
> >
> {: .solution}
{: .challenge}

> ## Exercise 5:
>
>
>
> > ## Solution:
> >
> > 
> {: .solution}
{: .challenge}


{% include links.md %}
