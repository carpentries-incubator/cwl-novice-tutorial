---
title: "Turning a shell script into a workflow"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "identify tasks, and data links in a script"
- "recognize loops that can be converted into scatters"
- "finding and reusing existing CWL command line tool descriptions "
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__convert a shell script into a CWL workflow__

{% include links.md %}

Below is a script that indexes as a reference genome and maps some sequencing reads (in FASTQ format)
against it. In this lesson we will learn how to convert this shell script to a Common Workflow Language workflow.

```bash=
#!/usr/bin/env bash

if ! [ -f data/reference.fa.sa ]; then
   bwa index data/reference.fa
fi

for sample in samples; do
   bwa mem data/reference.fa $sample > outputs/$sample.sam
done

# we have limited storage space so we
# cleanup indices once we are done
rm -f data/reference.fa.*

# TODO: more commands, to demonstrate data linkages

```

## Finding existing CWL tool descriptors for all the tools in the above script

The starting point in converting a shell script to a CWL workflow is to examine the script and identify the tools used in the
script. CWL workflows are built up from *CWL tool descriptors* and there might already be a tool descriptor for the tool that
that you want to use. Look [here](https://www.commonwl.org/#Repositories_of_CWL_Tools_and_Workflows) for some recommended places
to look for CWL tool descriptors.

Question: What bioinformatics tools are used in the above script? And where can we find tool descriptors for them?

Answers
* https://github.com/common-workflow-library/bio-cwl-tools/blob/release/bwa/BWA-Index.cwl
* https://github.com/common-workflow-library/bio-cwl-tools/blob/release/bwa/BWA-Mem.cwl


## Making tool invocation portable


Question: Which of these commands will refer to a user-supplied reference genome?

Answers
1. bwa index /data/reference
2. bwa index data/reference
3. bwa index $reference_genome 
4. bwa index $1

## Identifying workflow dependencies

Question: what files does the bwa mem command use?

Answer:
1. The genome reference FASTA and the FASTQ format sequence file
2. The FASTQ format sequence file
3. The genome reference FASTA, the BWA index for the reference and the FASTQ format sequence file

## Identifying workflow inputs and outputs

Question: What files does the workflow as a whole require as input? 

Answer:
1. The genome reference FASTA and the FASTQ format sequence file
2. The FASTQ format sequence file
3. The genome reference FASTA, the BWA index for the reference and the FASTQ format sequence file

Question: What files does the workflow output?

Answer:
1. The BWA index of the genome reference FASTA
2. The BAM format mapped reads
3. The BAM format mapped reads and the BWA index of the genome reference FASTA

## Cleanup of working directories

Question: in CWL, do we need to cleanup "temporary" data?

Answer: No, CWL runners run each workflow in a temporary working area and clean up any temporary files automatically.
