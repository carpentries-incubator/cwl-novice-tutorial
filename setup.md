---
title: Setup
---

## Software

These lessons are best followed using VSCode, and the Benten extension (which is a language server for CWL). We will also be using the CWL reference runner. Instructions for installing these are given below.

### VSCode and Benten installation

Download and install [VSCode](https://code.visualstudio.com/).

Open VSCode and search for [Benten](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl) in the marketplace. The name of the client extension is Rabix/benten. Follow the usual method to install the extension.

The VSCode Benten extension will require the Benten server to be installed too. It will prompt you to do this the first time you activate the extension.

### Software installation
This tutorial requires three pieces of software to be installed: Docker, cwltool, and graphviz.
Please follow instructions for your OS by clicking on the relevant tab below.

{::options parse_block_html="true" /}
<div>
<ul class="nav nav-tabs nav-justified" role="tablist">
<li role="presentation" class="active"><a data-os="windows" href="#windows" aria-controls="Windows"
role="tab" data-toggle="tab">Windows</a></li>
<li role="presentation"><a data-os="macos" href="#macos" aria-controls="macOS" role="tab"
data-toggle="tab">macOS</a></li>
<li role="presentation"><a data-os="linux" href="#linux" aria-controls="Linux" role="tab"
data-toggle="tab">Linux</a></li>
</ul>

<div class="tab-content">
<article role="tabpanel" class="tab-pane active" id="windows">
__Windows users__ first need to install the "Windows Subsystem for Linux 2" (WSL2) and the Docker Desktop before installing the `cwltool`.
Follow the steps below, taken from the [offical setup guide][cwl-windows-install]:
1. Install [Windows Subsystem for Linux 2 (WSL2), and Docker Desktop][install-wsl2-and-docker]
2. Install [Debian from the Microsoft Store][install-debian]
3. Set Debian as your default WSL 2 distro: `wsl --set-default debian`
4. Return to the Docker Desktop, choose [Settings → Resources → WSL Integration][docker-screenshot] and under "Enable integration with additional distros" select "Debian"
5. Reboot
6. Launch Debian
7. Install cwltool using the instructions under the 'Linux' tab
</article>

<article role="tabpanel" class="tab-pane" id="linux">
__Linux users__ already have a Bash terminal and can start with following the steps below.

In this tutorial the latest version of `cwltool` is needed. To ensure this, a virtual environment using `pip` and `venv` is used.

~~~
python3 -m venv env			# Create a virtual environment named 'env' in the current directory
source env/bin/activate			# Activate the 'env' environment
~~~
{: .language-bash}

The virtual environment needs to be activated every time you start the terminal using the `source env/bin/activate` command.

Next, install `cwltool`.

~~~
pip install cwltool
~~~
{: .language-bash}


For the visualisation of the workflow, please install graphviz:

~~~
sudo apt install graphviz
~~~
{: .language-bash}

</article>

<article role="tabpanel" class="tab-pane" id="macos">

</article>
</div>
</div>




## Files

You will need to install some example files for this lesson. In this tutorial we will use RNA sequencing data.

### Setting up a practice repository
For this tutorial some existing tools are needed to build the workflow. These existing tools will be imported via GitHub.
First we need to create an empty git repository for all our files. To do this, use this command:
~~~
git init novice-tutorial-exercises
~~~
{: .language-bash}

Next, we need to move into our empty git repo:

~~~
cd novice-tutorial-exercises
~~~
{: .language-bash}

Then import bio-cwl-tools with this command:
~~~
git submodule add https://github.com/common-workflow-library/bio-cwl-tools.git
~~~
{: .language-bash}

### Downloading sample and reference data
Create a new directory inside the `novice-tutorial-exercises` directory and download the data:
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
[cwl-windows-install]: https://github.com/common-workflow-language/cwltool#ms-windows-users
[install-wsl2-and-docker]: https://docs.docker.com/docker-for-windows/wsl/#prerequisites
[install-debian]: https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6
[docker-screenshot]: https://docs.docker.com/desktop/windows/images/wsl2-choose-distro.png
