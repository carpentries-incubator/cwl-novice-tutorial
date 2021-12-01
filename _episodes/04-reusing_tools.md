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
When you start a CWL workflow, it is recommended to check if there is already a CWL document available for the tools to be used.
[Bio-cwl-tools](https://github.com/common-workflow-library/bio-cwl-tools) is a library of CWL documents for biology/life-sciences related application.

In this episode you will use the bio-cwl-tools library to add the last step of the workflow. 
All of the steps of this workflow are available in this library and we have used it in the previous episodes for the first 3 steps.

There are different methods to use the tools from the bio-cwl-tools. 
The easiest method is downloading the whole library, however this makes it harder to update tools the newer versions.

You can also use git to add the tool as a submodule to your directory. 
First you need to create a new Git repository. Then you can add the submodule.

~~~
git init
git submodule add GITHUB_URL 
~~~
{: .language-bash} 


## Adding new step in workflow
The last step of the workflow is counting of the reads. For this step the `featureCounts` step will be used.
To implement the last step the bio-cwl-tools library library will be used.


> ## Exercise
>
> Find the `featureCounts` tool in the [bio-cwl-tools library](https://github.com/common-workflow-library/bio-cwl-tools).
> Have a look at the CWL document.
> Which inputs does this tool need? And what are the outputs of this tool?
>
> > ## Solution
> > The `featureCounts` CWL document has 2 inputs: `annotations` and `mapped_reads`, both files. These inputs can be found on lines 6 and 9.
> > The output of this tool is a file called `featurecounts` (line 21).
> >
> {: .solution}
{: .challenge}
{% include links.md %}


