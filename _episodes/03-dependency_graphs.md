---
title: "Developing Multi-Step Workflows"
teaching: 0
exercises: 0
questions:
- "How can we expand to a multi-step workflow?"
- "Iterative workflow development"
- "Workflows as dependency graphs"
- "How to use sketches for workflow design?"
objectives:
- "explain that a workflow is a dependency graph"
- "use cwlviewer online"
- "generate Graphviz diagram using cwltool"
- "exercise with the printout of a simple workflow; draw arrows on code; hand draw a graph on another sheet of paper"
- "recognise that workflow development can be iterative i.e. that it doesn't have to happen all at once"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
> ## Learning objectives
>By the end of this episode, learners should be able to __understand the flow of data between tools__ 
> and __explain that a workflow is a dependency graph__
>and __sketch their workflow, both by hand, and with an automated visualizer__
>and __recognise that workflow development can be iterative i.e. that it doesn't have to happen all at once__.
{: .callout}


## Multi-Step Workflow
In the previous episode a single step workflow was shown. To make a multi-step workflow, you extend the `steps` field.
In this episode, the workflow is extended with the next 2 steps of the RNA-sequencing analysis.
The next 2 steps are alingment of the reads and indexing the alignment. In this example `STAR` and `samtools` are used for these tasks.

__rna_seq_workflow.cwl__
~~~
clwVersion: v1.2
class: Workflow

inputs:
  rna_reads_human: File
  ref_genome: Directory
  
steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
	in:
	  reads_file: rna_reads_human
    out: [html_file]
	
  mapping_reads:
    requirements:
	  ResourceRequirement:
	    ramMin: 9000
    run: bio-cwl-tools/STAR/STAR-Align.cwl
    in:
      RunThreadN: {default: 4}
      GenomeDir: ref_genome
      ForwardReads: rna_reads_human
      OutSAMtype: {default: BAM}
      SortedByCoordinate: {default: true}
      OutSAMunmapped: {default: Within}
    out: [alignment]

  index_alignment:
    run: bio-cwl-tools/samtools/samtools_index.cwl
    in:
      bam_sorted: mapping_reads/alignment
    out: [bam_sorted_indexed]

outputs: 
  qc_html:
    type: File
	outputSource: quality_control/html_file
  bam_sorted_indexed:
    type: File
	outputSource: index_alignment/bam_sorted_indexed
~~~
{: .language-yaml}

The workflow file shows the first 3 steps of the RNA-seq analysis: `quality_control`, `mapping_reads` and `index_alignment`.
The `index_alignment` step uses the `alignment` output of the `mapping_reads` step. 
You do this by referencing the output of the `mapping_reads` step in the `in` field of the `index_alignment` step.
This is similar to referencing the outputs of the different steps in the `outputs` section. 

The `mapping_reads` step needs some extra information which are command line arguments if you run the STAR aligner on the command line.
This information is provided in the `in` field. To run the tool better, it needs more RAM than the default.
In the `requirements` field the `ResourceRequirement` field allocates a minimum of 9000 MB of RAM.

The newly added steps also need more inputs. The `mapping_reads` step needs a directory that contains the reference genome necessary for the mapping.
This is added in the `inputs` field and in the YAML input file, `workflow_input.yml`.

__workflow_input.yml__
~~~
rna_reads_human:
  class: File
  location: rnaseq/raw_fastq/Mov10_oe_1.subset.fq
  format: http://edamontology.org/format_1930
ref_genome:
  class: Directory
  location: hg19-chr1-STAR-index
~~~
{: .language-yaml}

> ## Exercise
>
> Draw the connecting arrows in the following graph of our workflow. 
> You can use for example Paint or print out the graph.
> 
> ![]({{page.root}}/fig/Ep3_empty_graph.png){: height="300px"}
> 
> > ## Solution
> > 
> > To find out how the inputs and the steps are connected to each other, you look at the `in` field of the different steps.
> >
> > ![]({{page.root}}/fig/Ep3_graph_answer.png){: height="300px"}
> {: .solution}
{: .challenge}


> ## Iterative working
> Working on a workflow is often not something that happens all at once. 
> Sometimes you already have a shell script ready that can be converted to a CWL workflow. 
> Other times it is similar to this tutorial, you start with a single-step workflow and extend it to a multi-step workflow.
> This is all iterative working, a continuous work in progress.
{: . callout}

## Visualising a workflow

A workflow is actually a directed acyclic graph (DAG). This means that the workflow has a certain direction, from inputs to outputs, and has no cycles.
A CWL workflow is a dependency graph. Each step in the workflow depends on either an input or another step.

To visualise a workflow, a graph can be used. This can be done before a CWL script is written to visualise how the different steps connect to eachother.
It is also possible to make a graph after the CWL script has been written. This graph can be generated using online tools or the build-in function in `cwltool`.
When a graph is generated, it can be used to visualise the steps taken and could make it easier to explain a workflow to other researchers.

### From CWL script to graph

In this example the workflow is already made, so the graph can be generated using [cwlviewer](https://view.commonwl.org/) online or using `cwltool`.
First, let's have a look at [cwlviewer](https://view.commonwl.org/). To use this tool, the workflow has to be put in a GitHub, GitLab or Git repository.
To view the graph of the workflow enter the URL and click `Parse Workflow`. The cwlviewer displays the workflow as a graph, starting with the input.
Then the different steps are shown, each with their input(s) and output(s). The steps are linked to eachother using arrows accompanied by the input of the next step.
The graph ends with the workflow outputs.

The graph of the RNA-seq workflow looks a follows:

![]({{page.root}}/fig/Ep3_graph_answer.png){: height="400px"}

It is also possible to generate the graph in the command line. `cwltool` has a function that makes a graph. 
The `--print-dot` option will print a file suitable for Graphviz `dot` program. This is the command to generate a Scalable Vector Graphic (SVG) file:

~~~
cwltool --print-dot rna_seq_workflow.cwl | dot -Tsvg > workflow_graph.svg
~~~
{: .language-bash}

The resulting SVG file displays the same graph as the one in the cwlviewer. The SVG file can be opened in a internetbrowser and in [Inkscape](https://inkscape.org/)

### Visualisation in VSCode
[__Benten__](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl) is an extension in Visual Studio Code (VSCode) that among other things visualises 
a workflow in a graph. When Benten is installed in VSCode, the tool can be used to visualise the workflow.
In the top-right corner of the VSCode window the CWL viewer can be opened, see the screenshot below.

![]({{page.root}}/fig/VSCode_CWL_Preview_(step1).png){: height="400px"}

In VSCode/Benten the inputs are shown in green, the steps in blue and the outputs in yellow. This graph looks a little bit different from the graph made with cwlviewer or `cwltool`.
The graph by VSCode/Benten doesn't show the output-input names between the different steps.

![]({{page.root}}/fig/VSCode_CWL_Preview_(step2).png){: height="400px"}


{% include links.md %}
