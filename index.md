---
layout: lesson
root: .  # Is the only page that doesn't follow the pattern /:path/index.html
permalink: index.html  # Is the only page that doesn't follow the pattern /:path/index.html
---
FIXME: home page introduction

<!-- this is an html comment -->

{% comment %} This is a comment in Liquid {% endcomment %}

> ## Prerequisites
>
> FIXME
{: .prereq}

#### Learner profile outlines

#### Researcher who expects to publish protocol

- who are they?
    - life sciences background
    - already learned essential computational research skills - attended a Software Carpentry workshop last year?
    - developing a new experimental protocol 
    - writing a pipeline/protocol to analyse the data generated
        - this combines multiple command line/bioinformatics tools in a few shell scripts that run on their research group's local server
- what problem are they having?
    - when they publish this research, they expect to be asked to include a reproducible description of the data analysis
    - the funding body requires them to publish details of their analyses in full
    - need to adapt existing script(s) to work in local HPC environment, as incoming experimental data requires upscaling of analysis
- how will the tutorial help them?
    - after applying what they've learned in the tutorial, their analysis pipeline will be 
        - more portable between compute environments
        - easy to share alongside publication of research findings (separate methods paper?)
    - after applying what they've learned in the tutorial, they will be able to provide information of provenance of research findings on request e.g. from reviewer/funder/collaborator
        - more robust to changes in tool/dependancy versions and adjustments to the protocol itself

#### RSE who inherited protocol and now needs to deploy/scale up

- who are they?
    - bioinformatics background
    - [some details about their training here...]
    - just joined a new lab and inherited several scripts from departing postdoc
    - leading development on a new tool
- what problem are they having?
    - all of these things will need to be deployed/deployable in a cloud environment soon
    - since the postdoc left, several key dependencies have been updated and the pipeline currently doesn't run
        - it feels like every time they manage to fix these problems, another update is released and everything breaks all over again
    - the group leader wants to change the short read alignment tool used in a key step in the existing workflow
- how will the tutorial help them?
    - after applying what they've learned in the tutorial, their analysis pipeline will be 
        - more portable between compute environments
        - more robust to changes in tool/dependancy versions
        - more maintainable
            - robust to adjustments to the protocol itself
            - quicker to make these adjustments

#### The Masters/PhD student who wants to learn workflows

- a lot of wet lab hours during Masters
- wants to learn computational research skills
- has been assigned task of implementing pipeline for variation of ChIP-seq analysis
- has never connected before to a remote machine from the command line
- supervisor is aware of some of the benefits of implementing this kind of workflow
    - pipeline will end up being run often and on large scale
    - why CWL? to ensure resulting workflow could be run anywhere, most flexible once workflow is described

{% include links.md %}
