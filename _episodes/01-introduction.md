---
title: "Introduction"
teaching: 0
exercises: 0
questions:
- "What are computational workflows, and why are they useful?"
- "What is Common Workflow Language?"
- "How are CWL workflows written?"
- "How do CWL workflows compare to shell workflows?"
- "What are the advantages of using CWL workflows?"
objectives:
- "Recognise the advantages of using a workflow"
- "Understand why you might use CWL instead of a shell script"
keypoints:
- "CWL is a standard for describing workflows based on command-line tools"
- "CWL workflows are written in a subset of YAML"
- "A CWL workflow is more portable than a shell script"
- "CWL supports software containers, supporting reproducibility on different machines"
---

# Computational Workflows

Computational workflows are widely used for data analysis, enabling rapid innovation and decision making.
_Workflow thinking_ is a form of "conceptualizing processes as recipes and protocols, structured as workflow or dataflow graphs with computational steps, and subsequently developing tools and approaches for formalizing, analyzing and communicating these process descriptions"[^1].
Distinct from general business workflows, computational workflows have their primary focus on data and dataflows.
The workflow managers which are designed for such computational workflows should aid their users with this focus.
The helpful properties that an ideal computational workflow would possess, to make this possible, are summarised below.

![Summary of handy properties of computational workflows. Categories of properties covered are: composition and abstraction; sharing and adaptability; automation; reporting and accreditation; scalability and infrastructure access; and portability.]({{page.root}}/fig/PropsComputeWF.png){: height="400px"}
Handy properties for Computational Workflows[^2]

Computational workflows, and their managers, should abstracting away complexities of the underlying computational environment.
Doing so enables the use of heterogeneous tools provided by multiple third parties, as well as the scalability and portability of the workflows, so users can make use of the resources available to them.
The workflow manager should make easy the automation, monitoring and provenance tracking of the dataflow.
In addition the computational workflow should be in a shareable format, published with open access.
These features all will contribute to any data produced using the computational workflow being FAIR (Findable, Accessible, Interoperable, and Reusable)[^3].

However as the rise in popularity of workflows has been matched by a rise in the number of disparate workflow managers that are available,
each with their own standards for describing the tools and workflows, reducing portability and interoperability of these workflows.
The Common Workflow Language (CWL) standard has been developed to address these problems, and to serve the general computational workflow needs described above.

# Common Workflow Language

CWL is a free and open standard for describing command-line tool based workflows[^3].
These standards provide a common, but reduced, set of abstractions that are both used in practice and implemented in many popular workflow managers.
The CWL language is declarative, enabling computational workflows to be constructed from diverse software tools, executing each through their command-line interface.

Previously researchers might write shell scripts to link together these command-line tools.
Although these scripts might provide a direct means of accessing the tools, writing and maintaining them requires specific knowledge of the system that they will be used on.
Shell scripts are not easily portable, and so researchers can easily end up spending more time maintaining the scripts than carrying out their research.
The aim of CWL is to reduce that barrier of usage of these tools to researchers.

CWL workflows are written in a subset of [YAML], with a syntax that does not restrict the amount of detail provided for a tool or workflow.
The execution model is explicit, all required elements of a tool's runtime environment must be specified by the CWL tool-description author.
On top of these basic requirements they can also add hints or requirements to the tool-description, helping to guide users (and workflow engines) on what resources are needed for a tool.

The CWL standards explicitly support the use of software container technologies, helping ensure that the execution of tools is reproducible.
Data locations are explicitly defined, and working directories kept separate for each tool invocation.
These standards ensure the portability of tools and workflows, allowing the same workflows to be run on your local machine, or in a HPC or cloud environment, with minimal changes required.

# RNA sequencing example

In this tutorial a bio-informatics RNA-sequencing analysis is used as an example. However, there is no specific knowledge needed for this tutorial.
RNA-sequencing is a technique which examines the quantity and sequences of RNA in a sample using next-generation sequencing.
The RNA reads are analyzed to measure the relative numbers of different RNA molecules in the sample. This analysis is differential gene expression.

The process looks like this:

![Diagram showing a typical RNA sequencing workflow. The workflow is linear, starting from taking biological samples and sequence reads, through quality control and trimming steps, to mapping to a genome and counting gene reads, to finally carrying out statistical analysis to identify differentially expressed genes.]({{page.root}}/fig/RNAseqWorkflow.png){: height="400px"}

During this tutorial, only the middle analytical steps will be performed. The adapter trimming is skipped.
These steps will be done:
- Quality control (FASTQC)
- Alignment (mapping)
- Counting reads associated with genes

The different tools necessary for this analysis are already available. In this tutorial a workflow will be set up to connect these tools and generate the desired output files.

{% include links.md %}

[^1]: M. R. Gryk and B. Ludäscher (2017): Workflows and Provenance: Toward Information Science Solutions for the Natural Sciences. Library Trends. [https://doi.org/10.1353/lib.2017.0018](https://doi.org/10.1353/lib.2017.0018)
[^2]: C. Goble (2021): FAIR Computational Workflows. JOBIM Proceedings. [https://www.slideshare.net/carolegoble/fair-computational-workflows-249721518](https://www.slideshare.net/carolegoble/fair-computational-workflows-249721518)
[^3]: M. Wilkinson, M. Dumontier, I. Aalbersberg, et al. (2016): The FAIR Guiding Principles for scientific data management and stewardship. Scientific Data. [https://doi.org/10.1038/sdata.2016.18](https://doi.org/10.1038/sdata.2016.18)
[^4]: M. R. Crusoe, S. Abeln, A. Iosup, P. Amstutz, J. Chilton, N. Tijanić, H. Ménager, S. Soiland-Reyes, B. Gavrilović, C. Goble, The CWL Community (2021): Methods Included: Standardizing Computational Reuse and Portability with the Common Workflow Language. Communication of the ACM. [https://doi.org/10.1145/3486897](https://doi.org/10.1145/3486897)
[YAML]: http://www.commonwl.org/user_guide/yaml/
