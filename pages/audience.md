---
permalink: /audience/
title: Learner Profiles
redirect_from:
  - /pages/audience.html
excerpt: This tutorial uses learner profiles to inform what we teach
---

## Martha

Early this year,
Martha successfully defended her PhD research into localized expression of
a transcription factor during mouse embryo development.
She recently began a post-doctoral position developing a new experimental method
building on approaches she used in her doctoral studies.
She knows that the new method will result in the generation of a lot of data,
which she is going to need to analyse in order to make sense of and publish her results.
She hopes that, as well as a high-impact paper describing the initial findings,
she'll be able to publish a methods paper describing the novel protocol.
Martha attended a [Software Carpentry](https://software-carpentry.org/) workshop
during her PhD studies and has used the skills she learned there
to write a collection of shell scripts that will perform most of the
analysis she will need. Each script combines multiple command line tools
and some R scripts for statistical analysis and plotting.

In the past, Martha was able to run scripts like these on her
research group's local analysis server. However, this server is
shared between all members of the group and she knows
this new, high-throughput protocol she's developing will produce data on
a scale much larger than she's ever had to deal with before.
Additionally, the funding body supporting Martha's research requires
that she and her collaborators publish the methods they use in full,
including a complete, reproducible description of the data analysis pipeline.

After following this tutorial,
Martha will be able to start converting her collection of shell scripts
into a workflow.
This will allow her to run the workflow on her university's HPC cluster,
which has a capacity far greater than the local server
she's been working with up to now.
She can also include the workflow description when she publishes her research findings,
allowing others to take and use her method in their own research.
The workflow runner can also provide provenance information
for all her results,
allowing her to trace the origin of her findings
and report this information to her collaborators and funders.

## RSE who inherited protocol and now needs to deploy/scale up

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

## The Masters/PhD student who wants to learn workflows

- a lot of wet lab hours during Masters
- wants to learn computational research skills
- has been assigned task of implementing pipeline for variation of ChIP-seq analysis
- has never connected before to a remote machine from the command line
- supervisor is aware of some of the benefits of implementing this kind of workflow
    - pipeline will end up being run often and on large scale
    - why CWL? to ensure resulting workflow could be run anywhere, most flexible once workflow is described
