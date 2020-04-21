---
title: "Adding your own script to a workflow"
teaching: 0
exercises: 0
questions:
- "Key question (FIXME)"
objectives:
- "See episode body and [this issue](https://github.com/common-workflow-lab/cwl-novice-tutorial/issues/8)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__include their own script as a step in a workflow__

several options for this were identified during the lesson development sprint.
we should choose one or two to focus on in the tutorial
(add your thoughts at the corresponding issue [here](https://github.com/common-workflow-lab/cwl-novice-tutorial/issues/8)):
  - make script executable and add to path
      - advantage: quick
      - downside:
          - the scripts are not shipped with the CWL document itself
          - not portable to other execution environment without putting the script first
  - pack the scripts into a docker container and make them executable there:
      - advantage: portable
      - downside:
          - only works for people that are using containers
  - use InitialWorkDirRequirement to list script content directly in the CWLToolWrapper
      - example: https://github.com/CompEpigen/ATACseq_workflows/blob/master/CWL/tools/generate_atac_signal_tags.cwl (bad example as here the scripts are too long, only for short scripts)
      - advantage:
          - portable
          - can be used independent of the dependency management solution
      - disadvantage:
          - explodes the complexity of a CWL tool wrapper
          - having to deal with escapes and so on
  - distribute as seperate package via pip / cran / ...

{% include links.md %}
