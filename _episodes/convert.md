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

Exercise: Find existing CWL tool descripts for all the tools in the above script

https://github.com/common-workflow-library/bio-cwl-tools

Answers
* https://github.com/common-workflow-library/bio-cwl-tools/blob/release/bwa/BWA-Index.cwl
* https://github.com/common-workflow-library/bio-cwl-tools/blob/release/bwa/BWA-Mem.cwl

Exercise: Which of these commands will work

Answers
bwa index /data/reference
bwa index data/reference
bwa index $reference_genome 
bwa index $1

