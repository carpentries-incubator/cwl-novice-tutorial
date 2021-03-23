---
title: "Scatter/Gather in a CWL Workflow"
teaching: 5
exercises: 0
questions:
- "How can I parallelize running of tools in a workflow?"
objectives:
- "Learn how to scatter steps of a CWL workflow."
keypoints:
- "Use"
---
This workflow uses a real life example of open-source CWL CommandLineTools from Dockstore.

CWL Workflows can scatter array inputs to run in parallel within the same step and gather all the outputs together to be used in subsequent steps. It's similar to GNU parallel in that it will execute all jobs in parallel using a single script and multiple input arguments.

Use the ScatterFeatureRequirement in the requirements section of a workflow to begin enabling scattering in a single step.

~~~
requirements:
   ScatterFeatureRequirement: {}
~~~
{: .language-yaml }

> ## Exercise 1
>
> In this workflow, we have a job that uses GATK Haplotype Caller.
> Currently, the job is configured to run on a single chromosome.
> How would you parallelize `GATK_HaplotypeCaller` to run all chromosomes simultaneously?
>
> ~~~
> cwlVersion: v1.0
> class: Workflow
> requirements:
>    ScatterFeatureRequirement: {}
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
> ~~~
> {: .language-yaml }
>
> > ## Solution
> > Given that we need to scatter on an array of chromosomes,
> > we changed the chromosome input variable to become a string array.
> > Following that, in the steps section, the scatter function on the intervals
> > is added so GATK `HaplotypeCaller` will run on each chromosome.
> > Finally, in the outputs section, the final output will be an array of VCFs.
> >
> > ~~~
> > cwlVersion: v1.0
> > class: Workflow
> > requirements:
> >    ScatterFeatureRequirement: {}
> > inputs:
> >    bam: File
> >    chromosomes: string[]
> > outputs:
> >   HaplotypeCaller_VCFs:
> >     type: File[]
> >     outputSource: GATK_HaplotypeCaller/vcf
> > steps:
> >   GATK_HaplotypeCaller:
> >     run: GATK_HaplotypeCaller.cwl
> >     scatter: [intervals]
> >     in:
> >       input_bam: bam
> >       intervals: chromosomes
> >     out: [vcf]
> > ~~~
> > {: .language-yaml }
> >
> > GATK will take the array of chromosome strings,
> > and then using it's intervals input option,
> > create multiple VCFs, one for each chromosome.
> > All these jobs will be run in parallel,
> > and run on as maybe compute nodes available.
> >
> > TODO: Add diagram of how the job will run
> {: .solution}
{: .challenge}


{% include links.md %}
