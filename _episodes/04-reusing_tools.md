---
title: "Resources for Reusing Tools and Scripts"
teaching: 0
exercises: 0
questions:
- "How to find other solutions/CWL recipes for awkward problems?"
objectives:
- "Know good resources for finding solutions to common problems"
keypoints:
- "[bio-cwl-tools](https://github.com/common-workflow-library/bio-cwl-tools) is a library of CWL documents for biology/life-sciences related tools"
---

## Pre-written tool descriptions
When you start a CWL workflow, it is recommended to check if there is already a CWL document available for the tools you want to use.
[Bio-cwl-tools][bio-cwl-tools] is a library of CWL documents for biology/life-sciences related tools.

The CWL documents of the previous steps were already provided for you, however, you can also find them in this library.
In this episode you will use the bio-cwl-tools library to add the last step to the workflow.

## Adding new step in workflow
The last step of our workflow is counting the RNA-seq reads for which we will use the [`featureCounts`](https://bio.tools/featurecounts) tool.

> ## Exercise
>
> Find the `featureCounts` tool in the [bio-cwl-tools library][bio-cwl-tools].
> Have a look at the CWL document. Which inputs does this tool need? And what are the outputs of this tool?
>
> > ## Solution
> > The `featureCounts` CWL document can be found in the [GitHub repo][featurecounts-cwl]; it has 2 inputs: `annotations` (line 6) and `mapped_reads`, both files. These inputs can be found on lines 6 and 9.
> > The output of this tool is a file called `featurecounts` (line 21).
> >
> {: .solution}
{: .challenge}
{% include links.md %}

We need a local copy of `featureCounts` in order to use it in our workflow.
We already imported this as a git submodule during setup,
so the tool should be located at `bio-cwl-tools/subread/featureCounts.cwl`.


> ## Exercise
> Add the `featureCounts` tool to the workflow.
> Similar to the `STAR` tool, this tool also needs more RAM than the default.
> To run the tool a minimum of 500 MiB of RAM is needed.
> Use a `requirements` entry with `ResourceRequirement` to allocate a `ramMin` of 500.
> Use the inputs and output of the previous exercise to connect this step to previous steps.
>
> > ## Solution
> >
> > ~~~
> > cwlVersion: v1.2
> > class: Workflow
> >
> > inputs:
> >   rna_reads_fruitfly_forward:
> >     type: File
> >     format: https://edamontology.org/format_1930  # FASTQ
> >   rna_reads_fruitfly_reverse:
> >     type: File
> >     format: https://edamontology.org/format_1930  # FASTQ
> >   ref_fruitfly_genome: Directory
> >   fruitfly_gene_model: File
> >
> > steps:
> >   quality_control_forward:
> >     run: bio-cwl-tools/fastqc/fastqc_2.cwl
> >     in:
> >       reads_file: rna_reads_fruitfly_forward
> >     out: [html_file]
> >
> >   quality_control_reverse:
> >     run: bio-cwl-tools/fastqc/fastqc_2.cwl
> >     in:
> >       reads_file: rna_reads_fruitfly_reverse
> >     out: [html_file]
> >
> >   trim_low_quality_bases:
> >     run: bio-cwl-tools/cutadapt/cutadapt-paired.cwl
> >     in:
> >       reads_1: rna_reads_fruitfly_forward
> >       reads_2: rna_reads_fruitfly_reverse
> >       minimum_length: { default: 20 }
> >       quality_cutoff: { default: 20 }
> >     out: [ trimmed_reads_1, trimmed_reads_2, report ]
> >
> >   mapping_reads:
> >     requirements:
> >       ResourceRequirement:
> >         ramMin: 5120
> >     run: bio-cwl-tools/STAR/STAR-Align.cwl
> >     in:
> >       RunThreadN: {default: 4}
> >       GenomeDir: ref_fruitfly_genome
> >       ForwardReads: trim_low_quality_bases/trimmed_reads_1
> >       ReverseReads: trim_low_quality_bases/trimmed_reads_2
> >       OutSAMtype: {default: BAM}
> >       SortedByCoordinate: {default: true}
> >       OutSAMunmapped: {default: Within}
> >       Overhang: { default: 36 }  # the length of the reads - 1
> >       Gtf: fruitfly_gene_model
> >     out: [alignment]
> >
> >   index_alignment:
> >     run: bio-cwl-tools/samtools/samtools_index.cwl
> >     in:
> >       bam_sorted: mapping_reads/alignment
> >     out: [bam_sorted_indexed]
> >
> >   count_reads:
> >     requirements:
> >       ResourceRequirement:
> >         ramMin: 500
> >     run: bio-cwl-tools/subread/featureCounts.cwl
> >     in:
> >       mapped_reads: index_alignment/bam_sorted_indexed
> >       annotations: gene_model
> >     out: [featurecounts]
> >
> > outputs:
> >   quality_report_forward:
> >     type: File
> >     outputSource: quality_control_forward/html_file
> >   quality_report_reverse:
> >     type: File
> >     outputSource: quality_control_reverse/html_file
> >   bam_sorted_indexed:
> >     type: File
> >     outputSource: index_alignment/bam_sorted_indexed
> >   featurecounts:
> >     type: File
> >     outputSource: count_reads/featurecounts
~~~
> > {: .language-yaml}
> {: .solution}
{: .challenge}

The workflow is complete and we only need to complete the YAML input file.
Please copy the `workflow_input_2.yml` file to `workflow_input_3.yml`, and
add the last entry in the input file, which is the `fruitfly_gene_model` file.

__workflow_input_3.yml__
~~~
rna_reads_fruitfly_forward:
  class: File
  location: rnaseq/GSM461177_1_subsampled.fastqsanger
  format: https://edamontology.org/format_1930  # FASTQ
rna_reads_fruitfly_reverse:
  class: File
  location: rnaseq/GSM461177_2_subsampled.fastqsanger
  format: https://edamontology.org/format_1930  # FASTQ
ref_fruitfly_genome:
  class: Directory
  location: rnaseq/dm6-STAR-index
fruitfly_gene_model:
  class: File
  location: rnaseq/Drosophila_melanogaster.BDGP6.87.gtf
  format: https://edamontology.org/format_2306
~~~
{: .language-yaml}

You have finished the workflow and the input file and now you can run the whole workflow.
~~~
cwltool --cachedir cache rna_seq_workflow_3.cwl workflow_input_3.yml
~~~
{: .language-bash}

[bio-cwl-tools]: https://github.com/common-workflow-library/bio-cwl-tools
[featurecounts-cwl]: https://github.com/common-workflow-library/bio-cwl-tools/blob/release/subread/featureCounts.cwl

{% include links.md %}

