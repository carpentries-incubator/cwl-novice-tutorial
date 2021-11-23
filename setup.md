---
title: Setup
---

## Software

These lessons are best followed using VSCode, and the Benten extension (which is a language server for CWL). We will also be using the CWL reference runner. Instructions for installing these are given below.

### VSCode and Benten installation

Download and install [VSCode](https://code.visualstudio.com/).

Open VSCode and search for [Benten](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl) in the marketplace. The name of the client extension is Rabix/benten. Follow the usual method to install the extension.

The VSCode Benten extension will require the Benten server to be installed too. It will prompt you to do this the first time you activate the extension.

### CWL installation

To install the cwltool on Linux or Windows, follow these [instructions](https://github.com/common-workflow-language/cwltool#install).


## Files

You will need to install some example files for this lesson. In this tutorial we will use RNA sequencing data.

### Setting up a practice repository
For this tutorial some existing tools are needed to build the workflow. These existing tools will be imported via GitHub. 
First we need to create an empty git repository for all our files. To do this, use this command:
~~~
git init novice-tutorial-exercises
~~~
{: .language-bash}

Next, we need to import bio-cwl-tools with this command:
~~~
git submodule add https://github.com/common-workflow-library/bio-cwl-tools.git
~~~
{: .language-bash}

### Downloading sample and reference data
Move inside the `novice-tutorial-exercises` directory and download the data:
~~~
mkdir rnaseq
cd rnaseq
wget --mirror --no-parent --no-host --cut-dirs=1 https://download.jutro.arvadosapi.com/c=9178fe1b80a08a422dbe02adfd439764+925/
~~~
{: .language-bash}

### Downloading or generating STAR index
To run the STAR tool index files generated from the reference files are needed.
This is a large download (4 GB), so it is also possible to generate these files yourself.

#### Downloading
~~~
mkdir hg19-chr1-STAR-index
cd hg19-chr1-STAR-index
wget --mirror --no-parent --no-host --cut-dirs=1 https://download.jutro.arvadosapi.com/c=02a12ce9e2707610991bd29d38796b57+2912/
~~~
{: .language-bash}

#### Generating 
Create `chr1-star-index.yaml` in the the `novice-tutorial-exercises` directory:
~~~
InputFiles:
  - class: File
    location: rnaseq/reference_data/chr1.fa
	format: http://edamontology.org/format_1930
IndexName: 'hg19-chr1-STAR-index'
Gtf:
  class: File
  location: rnaseq/reference_data/chr1-hg19_genes.gtf
Overhang: 99
~~~
{: .language-yaml}

Generate the index files with `cwltool`:
~~~
cwltool bio-cwl-tools/STAR/STAR-Index.cwl chr1-star-index.yaml
~~~
{: .language-bash}

{% include links.md %}
