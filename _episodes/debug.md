---
title: "Debugging Workflows"
teaching: 0
exercises: 0
questions:
- "How can I check my CWL file for errors?"
- "How can I get more information to help with solving an error?"
- "What are some common error messages when using CWL?"
objectives:
- "Check a CWL file for errors"
- "Output debugging information"
- "Interpret and fix commonly encountered error messages"
keypoints:
- "Run the workflow with the `--validate` option to check for errors"
- "The `--debug` option will output more information"
- "'Wiring' errors won't necessarily yield an error message"
---

When working on a CWL workflow, you will probably encounter errors. There are many different errors possible.
It is always very important to check the error message in the terminal, because it will give you information on the error.
This error message will give you the type of error as well as the line of code that contains the error.
Some of these errors will be explained in this episode.

As a first step to check if your CWL script contains any errors, you can run the workflow with the `--validate` flag.
~~~
cwltool --validate CWL_SCRIPT.cwl
~~~
{: .language-bash}

It is possible that the script is validated, however, it still gets an error.
If you encounter an error, the best practice is to run the workflow with the `--debug` flag.
This will provide you with extensive information on the error you encounter.
~~~
cwltool --debug CWL_SCRIPT.cwl
~~~
{: .language-bash}

### YAML errors
First of all, errors in the YAML syntax. When writing a piece of code, it is very easy to make a mistake.

Some very common YAML errors are:

#### Tabs
Using tabs instead of spaces. In YAML files indentations are made using spaces, not tabs.
  Please download and run [this example][tab-error] which includes a tab character.

~~~
$ cwltool tab-error.cwl workflow_input.yml
~~~
{: .language-bash}

~~~
ERROR Tool definition failed validation:
while scanning for the next token
file:///tab-error.cwl:5:1:   found character '\t' that cannot start any token
~~~
{: .error}

#### Field Name Typos

Typos in field names. It is very easy to forget for example the capital letters in field names.
  Errors with typos in field names will show `invalid field`.

__rna_seq_workflow_fieldname_fail.cwl__
~~~
cwlVersion: v1.2
class: Workflow

inputs:
  rna_reads_fruitfly: File
  ref_fruitfly_genome: Directory

steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]

  mapping_reads:
    requirements:
      ResourceRequirement:
        ramMin: 5120
    run: bio-cwl-tools/STAR/STAR-Align.cwl
    in:
      RunThreadN: {default: 4}
      GenomeDir: ref_fruitfly_genome
      ForwardReads: rna_reads_fruitfly
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
    outputsource: quality_control/html_file
  bam_sorted_indexed:
    type: File
    outputSource: index_alignment/bam_sorted_indexed
~~~
{: .language-yaml}

__workflow_input_debug.yml__
~~~
rna_reads_fruitfly:
  class: File
  location: rnaseq/GSM461177_1_subsampled.fastqsanger
  format: http://edamontology.org/format_1930  # FASTQ
ref_fruitfly_genome:
  class: Directory
  location: rnaseq/dm6-STAR-index
~~~
{: .language-yaml}


~~~
$ cwltool rna_seq_workflow_fieldname_fail.cwl workflow_input_debug.yml
~~~
{: .language-bash}

~~~
ERROR Tool definition failed validation:
rna_seq_workflow_fieldname_fail.cwl:1:1:   Object `rna_seq_workflow_fieldname_fail.cwl` is not valid
                                          because
                                            tried `Workflow` but
rna_seq_workflow_fieldname_fail.cwl:35:1:     the `outputs` field is not valid because
rna_seq_workflow_fieldname_fail.cwl:36:3:       item is invalid because
rna_seq_workflow_fieldname_fail.cwl:38:5:         invalid field `outputsource`, expected one of:
                                                  'label', 'secondaryFiles', 'streamable', 'doc', 'id',
                                                  'format', 'outputSource', 'linkMerge', 'pickValue', 'type'
~~~
{: .error}

#### Variable Name Typos
 Typos in variable names. Similar to typos in field names, it is easy to make a mistake in referencing to a variable.
  These errors will show `Field references unknown identifier.`


__rna_seq_workflow_varname_fail.cwl__
~~~
cwlVersion: v1.2
class: Workflow

inputs:
  rna_reads_fruitfly: File
  ref_fruitfly_genome: Directory

steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]

  mapping_reads:
    requirements:
      ResourceRequirement:
        ramMin: 5120
    run: bio-cwl-tools/STAR/STAR-Align.cwl
    in:
      RunThreadN: {default: 4}
      GenomeDir: ref_fruitfly_genome
      ForwardReads: rna_reads_fruitfly
      OutSAMtype: {default: BAM}
      SortedByCoordinate: {default: true}
      OutSAMunmapped: {default: Within}
    out: [alignment]

  index_alignment:
    run: bio-cwl-tools/samtools/samtools_index.cwl
    in:
      bam_sorted: mapping_reads/alignments
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

~~~
$ cwltool rna_seq_workflow_varname_fail.cwl workflow_input_debug.yml
~~~
{: .language-bash}

~~~
ERROR Tool definition failed validation:
rna_seq_workflow_varname_fail.cwl:8:1:  checking field `steps`
rna_seq_workflow_varname_fail.cwl:29:3:   checking object
                                          `rna_seq_workflow_varname_fail.cwl#index_alignment`
rna_seq_workflow_varname_fail.cwl:31:5:     checking field `in`
rna_seq_workflow_varname_fail.cwl:32:7:       checking object
                                              `rna_seq_workflow_varname_fail.cwl#index_alignment/bam_sorted`
                                                Field `source` references unknown identifier
                                                `mapping_reads/alignments`, tried
                                                file:///.../rna_seq_workflow_varname_fail.cwl#mapping_reads/alignments
~~~
{: .error}

### Wiring error
Wiring errors often occur when you forget to add an output from a workflow's step to the `outputs` section.
This doesn't cause an error message, but there won't be any output in your directory.
To get the desired output you have to run the workflow again.
Best practice is to check your `outputs` section before running your script to make sure all the outputs you want are there.

### Type mismatch
Type errors take place when there is a mismatch in type between variables.
When you declare a variable in the `inputs` section, the type of this variable has to match the type in the YAML inputs file
and the type used in one of the workflows steps.
The error message that is shown when this error occurs will tell you on which line the mismatch happens.

__rna_seq_workflow_type_fail.cwl__
~~~
cwlVersion: v1.2
class: Workflow

inputs:
  rna_reads_fruitfly: int
  ref_fruitfly_genome: Directory

steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]

  mapping_reads:
    requirements:
      ResourceRequirement:
        ramMin: 5120
    run: bio-cwl-tools/STAR/STAR-Align.cwl
    in:
      RunThreadN: {default: 4}
      GenomeDir: ref_fruitfly_genome
      ForwardReads: rna_reads_fruitfly
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

~~~
$ cwltool rna_seq_workflow_type_fail.cwl workflow_input_debug.yml
~~~
{: .language-bash}

~~~
ERROR Tool definition failed validation:

rna_seq_workflow_type_fail.cwl:5:3:   Source 'rna_reads_fruitfly' of type "int" is incompatible
rna_seq_workflow_type_fail.cwl:12:7:   with sink 'reads_file' of type "File"
rna_seq_workflow_type_fail.cwl:5:3:   Source 'rna_reads_fruitfly' of type "int" is incompatible
rna_seq_workflow_type_fail.cwl:23:7:   with sink 'ForwardReads' of type ["File", {"type":
                                       "array", "items": "File"}]
~~~
{: .error}

### Format error
Some files need a specific format that needs to be specified in the YAML inputs file, for example the fastq file in the RNA-seq analysis.
When you don't specify a format, an error will occur. You can for example use the [EDAM](https://www.ebi.ac.uk/ols/ontologies/edam) ontology.

__rna_seq_workflow_debug.cwl__
~~~
cwlVersion: v1.2
class: Workflow

inputs:
  rna_reads_fruitfly: File
  ref_fruitfly_genome: Directory

steps:
  quality_control:
    run: bio-cwl-tools/fastqc/fastqc_2.cwl
    in:
      reads_file: rna_reads_fruitfly
    out: [html_file]

  mapping_reads:
    requirements:
      ResourceRequirement:
        ramMin: 5120
    run: bio-cwl-tools/STAR/STAR-Align.cwl
    in:
      RunThreadN: {default: 4}
      GenomeDir: ref_fruitfly_genome
      ForwardReads: rna_reads_fruitfly
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


__workflow_input_undefined.yml__
~~~
rna_reads_fruitfly:
  class: File
  location: rnaseq/GSM461177_1_subsampled.fastqsanger
ref_fruitfly_genome:
  class: Directory
  location: rnaseq/dm6-STAR-index
~~~
{: .language-yaml}

~~~
$ cwltool rna_seq_workflow_debug.cwl workflow_input_undefined.yml
~~~
{: .language-bash}

~~~
ERROR Exception on step 'mapping_reads'
ERROR [step mapping_reads] Cannot make job: Expected value of 'ForwardReads' to have format http://edamontology.org/format_1930 but
  File has no 'format' defined: {
    "class": "File",
    "location": "file:///.../rnaseq/GSM461177_1_subsampled.fastqsanger",
    "size": 142867948,
    "basename": "GSM461177_1_subsampled.fastqsanger",
    "nameroot": "GSM461177_1_subsampled",
    "nameext": ".fastqsanger"
}
~~~
{: .error}
{% include links.md %}
[tab-error]: {{ page.root }}/code/tab-error.cwl
