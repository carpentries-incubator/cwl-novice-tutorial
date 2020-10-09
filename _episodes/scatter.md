---
title: "Scattering"
teaching: 0
exercises: 0
questions:
- "How can I run the same tool with multiple inputs?"
objectives:
- "explain what is meant by the _scatter_ pattern in workflow design, and how it differs from the similar concept of parallel execution"
- "identify when the scatter pattern appears in a workflow description"
- "(FIXME: does the above cover the intended meaning of the following two points from the lesson development sprint?); running the same program on each file; running the same program the same way except for one parameter"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__implement scattering of steps in a workflow__.

This workflow uses a real life example of open-source CWL CommandLineTools from Dockstore.

CWL Workflows can scatter array inputs to run in parallel within the same step and gather all the outputs together to be used in subsequent steps. It's similar to GNU parallel in that it will execute all jobs in parallel using a single script and multiple input arguments.

This workflow shows how to parallelize running a single bam into multiple VCFs.

~~~
cwlVersion: v1.0
class: Workflow
requirements:
   ScatterFeatureRequirement: {}
inputs:
   bam: File
   chromosomes: string[]
outputs:
  HaplotypeCaller_VCFs:
    type: File[]
    outputSource: GATK_HaplotypeCaller/vcf
steps:
  GATK_HaplotypeCaller:
    run: GATK_HaplotypeCaller.cwl
    scatter: [intervals]
    in:
      input_bam: bam
      intervals: chromosomes
    out: [vcf]
~~~

Use the ScatterFeatureRequirement in the requirements section of a workflow to begin enabling scattering in a single step.

~~~
requirements:
   ScatterFeatureRequirement: {}
~~~

In the steps section, use scatter and the input variable to scatter on, in this example its the intervals, which uses the chromosomes string array specified in the inputs section of the Workflow.

~~~
steps:
  GATK_HaplotypeCaller:
    run: GATK_HaplotypeCaller.cwl
    scatter: [intervals]
~~~

GATK will take the array of chromosome strings, and then using it's intervals option, create multiple VCFs, one for each chromosome. All these jobs will be run in parallel, and run on as maybe compute nodes available.

{% include links.md %}
