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

Exercise 1:

For each of these variables, is it required in a CommandLineTool?

A. inputs
B. outputs
C. arguments
D. baseCommand
E. class
F. requirements
G. hints

Exercise 2:

What ResourceRequirements can you specify in a CommandLineTool?

Answer:

cores, ram, tmpdir disk space, outdir disk space

coresMin
coresMax
ramMin
ramMax
tmpdirMin
tmpdirMax
outdirMin
outdirMax

Exercise 3:

What input types would you use for these inputs?

A. BAM (File)
B. VCF (File)
C. sample name (String)
D. 23 chromosomes (String Array)
E. Flag for turning on paired-end mode in bwa mem (boolean)
F. Cohort of sample FASTQs (Array of File Arrays)

Exercise 4:

Fix this code for capturing the correct file in this CommandLineTool. Samtools index will take a BAM and create an index (.bai) file. The output should be the bam and index together.

Hint?: secondaryFiles will help group files together such as a bam and its index.

~~~
baseCommand: [samtools, index]
inputs:
  bam: File
outputs:
  bam_output_and_index:
    type:
    outputBinding:
      glob:
~~~

Answer:

~~~
baseCommand: [samtools, index]
inputs:
  bam: File
outputs:
  bam_output_and_index:
    type: File
    outputBinding:
      glob: $(inputs.input.basename)
    secondaryFiles:
      - .bai
~~~

Exercise 5:

Name ways to put arguments on the command line (TODO: fix wording)

Answer:

inputBinding in inputs
hard code in baseCommand
hard code in arguments section, and using javascript to call the input variable


{% include links.md %}
