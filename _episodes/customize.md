---
title: "Customizing workflows"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "customize a workflow at any of the many levels"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__customize a workflow at any of the many levels__:

- Change the input object
> ## Exercise 1:
> 
> Given this workflow, add an input reference to the workflow to be passed to the GATK_HaplotypeCaller step.
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>    chromosome: string
> outputs:
>   HaplotypeCaller_VCF:
>     type: File
>     outputSource: GATK_HaplotypeCaller/vcf
> steps:
>   GATK_HaplotypeCaller:
>     run: GATK_HaplotypeCaller.cwl
>     in:
>       input_bam: bam
>       intervals: chromosome
>     out: [vcf]
> 
> > ## Solution:
> >
> > cwlVersion: v1.0
> > class: Workflow
> > inputs:
> >    bam: File
> >    chromosome: string
> >    reference: File
> > outputs:
> >   HaplotypeCaller_VCF:
> >     type: File
> >     outputSource: GATK_HaplotypeCaller/vcf
> > steps:
> >   GATK_HaplotypeCaller:
> >     run: GATK_HaplotypeCaller.cwl
> >     in:
> >       input_bam: bam
> >       intervals: chromosome
> >       reference_fasta: reference
> >     out: [vcf]
> {: .solution}
{: .challenge}

- Change the default values at the workflow level
> ## Exercise 2:
>
> Default values in a workflow can be used at both the input object level and the step level. It is similar to how it is used in a CommandLineTool.
> 
> Given this workflow, add a default string value of "sample" for the sample_name variable and a default file value of "/home/user/sample.fa" for the reference input.
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>    chromosome: string
>    sample: string
>    reference: File
> outputs:
>   HaplotypeCaller_VCF:
>     type: File
>     outputSource: GATK_HaplotypeCaller/vcf
> steps:
>   GATK_HaplotypeCaller:
>     run: GATK_HaplotypeCaller.cwl
>     in:
>       input_bam: bam
>       intervals: chromosome
>       reference_fasta: reference
>     out: [vcf]
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > inputs:
> >    bam: File
> >    chromosome: string
> >    sample:
> >      type: string
> >      default: sample
> >    reference:
> >      type: File
> >      default: /home/user/sample.fa
> > outputs:
> >   HaplotypeCaller_VCF:
> >     type: File
> >     outputSource: GATK_HaplotypeCaller/vcf
> > steps:
> >   GATK_HaplotypeCaller:
> >     run: GATK_HaplotypeCaller.cwl
> >     in:
> >       input_bam: bam
> >       intervals: chromosome
> >       reference_fasta: reference
> >     out: [vcf]
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> In order to change the default value in a step input variable, the requirement StepInputExpressionRequirement must be declared. 
>
> In this workflow, add an input called "ERC" to the GATK_HaplotypeCaller step and give it a default value of true.
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>    chromosome: string
>    sample: string
>    reference: File
> outputs:
>   HaplotypeCaller_VCF:
>     type: File
>     outputSource: GATK_HaplotypeCaller/vcf
> steps:
>   GATK_HaplotypeCaller:
>     run: GATK_HaplotypeCaller.cwl
>     in:
>       input_bam: bam
>       intervals: chromosome
>       reference_fasta: reference
>     out: [vcf]
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > requirements:
> >   StepInputExpressionRequirement: {}
> > inputs:
> >    bam: File
> >    chromosome: string
> >    sample: string
> >    reference: File
> > outputs:
> >   HaplotypeCaller_VCF:
> >     type: File
> >     outputSource: GATK_HaplotypeCaller/vcf
> > steps:
> >   GATK_HaplotypeCaller:
> >     run: GATK_HaplotypeCaller.cwl
> >     in:
> >       input_bam: bam
> >       intervals: chromosome
> >       reference_fasta: reference
> >       ERC:
> >         default: true
> >     out: [vcf]
> {: .solution}
{: .challenge}


- Change hard coded values at the workflow level
> ## Exercise 4:
>
> 
> > ## Solution:
> {: .solution}
{: .challenge}

- Change default value at the Workflow step level
- Change hard coded values at the Workflow step level
- Change default values in the CLT description
- Change hard coded values in the CLT description
- Change the container

> ## Exercise 5:
>
> Using docker images is a good way of creating reproducible workflows. When specifying DockerRequirement in the hints section of a workflow, you can use your own local images or images from a URL. Using dockerPull will grab a docker image from your local repository. Using dockerLoad will grab a Docker image using an HTTP URL.
>
> In this workflow, use dockerPull to add a docker image called "broadinstitute/gatk4"
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>   bam: File
>   chromosome: string
>   sample: string
>   reference: File
> outputs:
>   HaplotypeCaller_VCF:
>     type: File
>     outputSource: GATK_HaplotypeCaller/vcf
> steps:
>   GATK_HaplotypeCaller:
>     run: GATK_HaplotypeCaller.cwl
>     in:
>       input_bam: bam
>       intervals: chromosome
>       reference_fasta: reference
>     out: [vcf]
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > hints:
> >   DockerRequirement:
> >     dockerPull: broadinstitute/gatk4
> > inputs:
> >   bam: File
> >   chromosome: string
> >   sample: string
> >   reference: File
> > outputs:
> >   HaplotypeCaller_VCF:
> >     type: File
> >     outputSource: GATK_HaplotypeCaller/vcf
> > steps:
> >   GATK_HaplotypeCaller:
> >     run: GATK_HaplotypeCaller.cwl
> >     in:
> >       input_bam: bam
> >       intervals: chromosome
> >       reference_fasta: reference
> >     out: [vcf]
> >
> > Now, every step of the workflow will use the same Docker container!
> {: .solution}
{: .challenge}
- Change the tool source itself

{% include links.md %}
