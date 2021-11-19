---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "What is Common Workflow Language?"
- "How are CWL workflows written?"
- "How do CWL workflows compare to shell workflows?"
- "What are the advantages of using CWL workflows?"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

Computational workflows are widely used for data analysis, enabling rapid innovation and decision making. _Workflow thinking_ is a form of "conceptualizing processes as recipes and protocols, structured as [work- or] dataflow graphs with computational steps, and subsequently developing tools and approaches for formalizing, analyzing and communicating these process descriptions" ([Gryk & Ludascher, 2017](https://doi.org/10.1353/lib.2017.0018)).

However as the rise in popularity of workflows has been matched by a rise in the number of dispirit workflow managers that are available, each with their own standards for describing the tools and workflows, reducing portability and interoperability of these workflows.

CWL is a free and open standard for describing command-line tool based workflows[^1]. These standards provide a common, but reduced, set of abstractions that are both used in practice and implemented in many popular workflow systems. The CWL language is declarative, enabling computational workflows to be constructed from diverse software tools, executing each through their command-line interface.

Previously researchers might write shell scripts to link together these command-line tools. Although these scripts might provide a direct means of accessing the tools, writing and maintaining them requires specific knowledge of the system that they will be used on. Shell scripts are not easily portable, and so researchers can easily end up spending more time maintaining the scripts than carrying out their research. The aim of CWL is to reduce that barrier of usage of these tools to researchers.

CWL workflows are written in a subset of YAML, with a syntax that does not restrict the amount of detail provided for a tool or workflow. The execution model is explicit, all required elements of a tool's runtime environment must be specified by the CWL tool-description author. On top of these basic requirements they can also add hints or requirements to the tool-description, helping to guide users (and workflow engines) on what resources are needed for a tool.

The CWL standards explicitly support the use of software container technologies, helping ensure that the execution of tools is reproducible. Data locations are explicitly defined, and working directories kept separate for each tool invocation. These standards ensure the portability of tools and workflows, allowing the same workflows to be run on your local machine, or in a HPC or cloud environment, with minimal changes required.

{% include links.md %}

[^1]: Michael R. Crusoe, Sanne Abeln, Alexandru Iosup, Peter Amstutz, John Chilton, Nebojša Tijanić, Hervé Ménager, Stian Soiland-Reyes, Bogdan Gavrilović, Carole Goble, The CWL Community (2021):
  Methods Included: Standardizing Computational Reuse and Portability with the Common Workflow Language.
  Communication of the ACM. https://doi.org/10.1145/3486897 Retrieved from arXiv 2105.07028 [cs.DC] https://arxiv.org/abs/2105.07028