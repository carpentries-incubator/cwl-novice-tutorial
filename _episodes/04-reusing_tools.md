---
title: "Resources for Reusing Tools and Scripts"
teaching: 0
exercises: 0
questions:
- "How to find other tools/solutions for awkward problems?"
objectives:
- "tools objectives:"
- "know good resources for finding solutions to common problems"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

> ## Learning objectives
> By the end of this episode,
> learners should __be aware of where they can look for CWL recipes and more help for common, but awkward, tasks__.
{: .callout}


## Pre-written tool descriptions
Starting a CWL workflow, it is recommended to check if there is already a CWL description available for the tools to be used.

[Bio-cwl-tools](https://github.com/common-workflow-library/bio-cwl-tools): library of CWL descriptions for biology/life-sciences related application.

In this tutorial the bio-cwl-tools library is used. The RNA-sequencing analysis workflow uses the fastqc, STAR and samtools CWL descriptions of this library.

### Bio-cwl-tools
There are 4 different methods to use tools from the [bio-cwl-tools](https://github.com/common-workflow-library/bio-cwl-tools):
1. Make a git submodule of the repository to the Git repository of the workflow. This enables control of the version used.
2. Copy the contents of the repository to the repository of the workflow. This might make it harder to managing updates.
3. Use the in-development [CWL Dependency Manager](https://github.com/common-workflow-language/cwldep).
4. Use the URL of the repository to refer to the tool.


## Adding new step in workflow

TODO: Adding the featureCounts step

> ## Exercise
>
>
> > ## Solution
> > 
> >
> {: .solution}
{: .challenge}
{% include links.md %}
