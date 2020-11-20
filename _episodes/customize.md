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

You've been given a workflow by your colleague that runs GATK HaplotypeCaller and must change various points to fix your needs.

- Change the input object
> ## Exercise 1:
> 
> In the input yaml file, your colleague adds an input for a bam input. You decide that you want to add a reference file input and a chromosome string input. Add those to this yaml file.
> 
> bam:
>   class: File
>   location: ftp:/­/­ftp.­1000genomes.­ebi.­ac.­uk/­vol1/­ftp/­phase3/­data/­HG00133/­alignment/­HG00133.­unmapped.­ILLUMINA.­bwa.­GBR.­low_coverage.­20120522.­bam
>
> > ## Solution:
> >
> > bam:
> >   class: File
> >   location: ftp:/­/­ftp.­1000genomes.­ebi.­ac.­uk/­vol1/­ftp/­phase3/­data/­HG00133/­alignment/­HG00133.­unmapped.­ILLUMINA.­bwa.­GBR.­low_coverage.­20120522.­bam
> > reference:
> >   class: File
> >   location: https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
> > chromosome: chr1
> >
> {: .solution}
{: .challenge}

- Change the default values at the workflow level
> ## Exercise 2:
>
> Default values in a workflow can be used at both the input object level and the step level. Add the new reference and chromosome inputs to the workflow
> 
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > inputs:
> >    bam: File
> >    chromosome: string
> >    reference: File
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> In this workflow, add a default value for the reference in the inputs section.
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>    chromosome: string
>    reference: File
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > inputs:
> >    bam: File
> >    chromosome: string
> >    reference:
> >      class: File
> >      default:
> >        type: File
> >        location: https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
> {: .solution}
{: .challenge}


- Change hard coded values at the workflow level
> ## Exercise 4:
>
> In this workflow, add the new inputs to the GATK_HaplotypeCaller step. Assume that the inputs to GATK_HaplotypeCaller.cwl are the same variables as what you stated before.
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>    bam: File
>    chromosome: string
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
>     out: [vcf]
>
> > ## Solution:
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
> >       chromosome: chromosome
> >       reference: reference
> >     out: [vcf]
> {: .solution}
{: .challenge}


- Change default value at the Workflow step level

> ## Exercise 5:
>
> Default values in a workflow can be used at both the input object level and the step level. Add a default reference and chromosome inputs to the steps portion of the workflow. The requirement StepInputExpressionRequrirement must be declared in the requirements section to be able to add default values at the step level.
>
> cwlVersion: v1.0
> class: Workflow
> inputs:
>   bam: File
>   chromosome: string
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
>       chromosome: chromosome
>       reference: reference
>     out: [vcf]
>
> > ## Solution:
> > cwlVersion: v1.0
> > class: Workflow
> > requirements:
> >   StepInputExpressionRequirement: {}
> > inputs:
> >   bam: File
> >   chromosome: string
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
> >       chromosome:
> >         default: chr1
> >       reference:
> >         default:
> >           type: File
> >           location: https://hgdownload.soe.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz
> >     out: [vcf]
> {: .solution}
{: .challenge}


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
