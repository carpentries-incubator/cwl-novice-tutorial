---
title: Setup
---

## Software Setup

These lessons assume that you are using the freely available Visual Studio Code application with the
Benten extension along with the CWL reference runner (`cwltool`).

This tutorial requires three pieces of software to run and visualize the workflows: Docker, cwltool, and graphviz.

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

1. Download and install [VSCode](https://code.visualstudio.com/){:target="_blank"}{:rel="noopener noreferrer"}.

2. [Open Benten in the marketplace](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl){:target="_blank"}{:rel="noopener noreferrer"} and click the `Install` button or follow the directions.

3. Install and configure Windows Subsystem for Linux 2 (WSL2), and Docker Desktop
   1. Confirm that you are running Windows 10, version 2004 or higher (Build 19041 and higher) or Windows 11.

      > ## Check your Windows version
      > To check your Windows version and build number, press the Windows logo key + <kbd>R</kbd>, type winver, select OK.
        You can update to the latest Windows version by selecting "Start" > "Settings" > "Windows Update" > "Check for updates".
      {: .callout }
   2. Open PowerShell as Administrator ("Start menu" > "PowerShell" > right-click > "Run as Administrator")
      and paste the following commands followed by <kbd>Enter</kbd> to install WSL 2:  
      `wsl --update`  
      `wsl --install --distribution Ubuntu`  
      To ensure that `Ubuntu` is the default subsystem instead of `docker-desktop-*`, you may need to use:  
      `wsl --set-default Ubuntu`  
      If you had previously installed WSL1 in Windows 10, upgrade to WSL2 with:  
      `wsl --set-version Ubuntu 2` 
   3. Reboot your computer. Ubuntu will set itself up after the reboot. Wait for Ubuntu to ask for a
      UNIX username and password. After you provide that information and the command prompt appears,
      then the Ubuntu window can be closed.
   4. Then continue to [download Docker Desktop](https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe){:target="_blank"}{:rel="noopener noreferrer"} and run the installer.
      1. Reboot after installing Docker Desktop.
      2. Run Docker Desktop
      3. Accept the terms and conditions, if prompted
      4. Wait for Docker Desktop to finish starting
      5. Skip the tutorial, if prompted
      6. From the top menu choose "Settings" > "Resources" > "WSL Integration"
      7. Under "Enable integration with additional distros" select "Ubuntu"
      8. Close the Docker Desktop window
4. Configure VS Code
   1. Open [this link](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl){:target="_blank"}{:rel="noopener noreferrer"}
      to install the **Remote - WSL** extension for VS Code by clicking the `Install` button or by following the directions.
   2. After installation, in VS Code choose _Open a Remote - WSL Window_ and then _New WSL Window_.

      If you don't see those option, then press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and then type
      <kbd>WSL</kbd> and they should appear at the top of the screen.
   3. There should now be a second VS Code window that has `WSL: Ubuntu` in green at the lower left corner.
      You can close the original VS Code window.
   4. To enable the **Benten CWL** extension in this "WSL : Ubuntu" window:
      * Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd> to open the "Extensions" pane.
      * Look for "CWL (Rabix/Benten)" and click the blue "Install in WSL: Ubuntu" button.
5. Open a terminal and install tutorial prerequisites
   * Choose "Terminal" > "New Terminal" from the menu in the "WSL : Ubuntu" VS Code window.
   * Copy the following `sudo apt-get update && sudo apt-get install -y python3-venv wget graphviz nodejs`
   * Paste it into the terminal window
   * Press <kbd>Return</kbd> to run it. You will need to use the UNIX password you set earlier.

   > ## What is the "terminal"?
   > All references to a "terminal" for the rest of this tutorial are to this terminal window inside
   > the "WSL : Ubuntu" Visual Studio Code window, and not Powershell, the Windows Command Prompt,
   > nor the "Ubuntu" app.
   {: .callout}
6. Install the latest version of cwltool.
   1. First we will make a Python virtual environment by running the following commands in the terminal.
      ~~~
      python3 -m venv env       # Create a virtual environment named 'env' in the current directory
      source env/bin/activate   # Activate the 'env' environment
      ~~~
      {: .language-bash}

      You will know that this worked as the terminal prompt will now have `(env)` at the beginning.

      > ## Reactivating the python virtual environment
      > Every time you launch VS Code or launch a new terminal, you must run `source env/bin/activate`
      > to re-enable access to this Python Virtual Environment.
      {: .callout}
    2. Next, install cwltool by running the following in the terminal:
       ~~~
       pip install cwltool
       ~~~
       {: .language-bash}
</article>

<article role="tabpanel" class="tab-pane" id="linux">

1. Download and install [VSCode](https://code.visualstudio.com/){:target="_blank"}{:rel="noopener noreferrer"}.
2. [Open Benten in the marketplace](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl){:target="_blank"}{:rel="noopener noreferrer"} and click the `Install` button or follow the directions.
3. [Install docker](https://docs.docker.com/engine/install/){:target="_blank"}{:rel="noopener noreferrer"}
4. [Enable docker usage as a non-root user](https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user){:target="_blank"}{:rel="noopener noreferrer"}
5. Install the latest version of `cwltool`.
   1. First we will make a Python virtual environment by running the following commands in the terminal.
      ~~~
      python3 -m venv env       # Create a virtual environment named 'env' in the current directory
      source env/bin/activate   # Activate the 'env' environment
      ~~~
      {: .language-bash}

      You will know that this worked as the terminal prompt will now have `(env)` at the beginning.

      > ## Reactivating the python virtual environment
      > Every time you launch VS Code or launch a new terminal, you must run `source env/bin/activate`
      > to re-enable access to this Python Virtual Environment.
      {: .callout}
    2. Next, install cwltool by running the following in the terminal:
       ~~~
       pip install cwltool
       ~~~
       {: .language-bash}
6.  Later we will make visualisations of our workflows. To support that we need to install `graphviz`.

       Here is the command for Debian-based Linux systems to install `graphviz` and other helpful programs:
       ~~~
       sudo apt-get install -y graphviz wget git nodejs
       ~~~
       {: .language-bash}

      For other Linux systems, check <https://graphviz.org/download/#linux>

> ## Extra action if you install Docker using Snap
> [Snap](https://snapcraft.io/) is an app management system for linux - which is popular on
> Ubuntu and other systems. Docker is available via Snap - if you have installed it using
> this service you will need to take the following steps, to ensure docker will work properly.
> ~~~
> mkdir ~/tmp
> export TMPDIR=~/tmp
> ~~~
> {: .language-bash}
> Each time you open a new terminal you will have to enter the `export TMPDIR=~/tmp` command,
> or you can add it to your `~/.profile` or `~/.bashrc` file.
{: .callout}

</article>

<article role="tabpanel" class="tab-pane" id="macos">

1. Download and install [VSCode](https://code.visualstudio.com/){:target="_blank"}{:rel="noopener noreferrer"}.
2. [Open Benten in the marketplace](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl){:target="_blank"}{:rel="noopener noreferrer"} and click the `Install` button or follow the directions.
3. [Install docker](https://docs.docker.com/desktop/mac/install){:target="_blank"}{:rel="noopener noreferrer"}
4. [Install miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html){:target="_blank"}{:rel="noopener noreferrer"}
5. Tell conda about which channels (sources) we will use
    ~~~
    conda config --add channels conda-forge
    ~~~
    {: .language-bash}
6. Create a virtual environment using conda
    ~~~
    conda create --name cwltutorial
    ~~~
    {: .language-bash}
7. Activate the virtual environment
    ~~~
    conda activate cwltutorial
    ~~~
    {: .language-bash}
8. Install the CWL reference runner (`cwltool`), and some helper programs using conda
    ~~~
    conda install cwltool graphviz wget git nodejs
    ~~~
    {: .language-bash}

> ## Reactivating the python virtual environment
> The virtual environment needs to be activated every time you start a terminal using
`conda activate cwltutorial`.
{: .callout}

</article>
</div>
</div>

### Confirm the software is installed correctly
To confirm docker is installed, run the following command to display the version number:
~~~
docker version
~~~
{: .language-bash}

You should see something similar to the output shown below.
~~~
Client: Docker Engine - Community
 Version:           20.10.13
 API version:       1.41
 Go version:        go1.16.15
 Git commit:        a224086
 Built:             Thu Mar 10 14:08:15 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server: Docker Engine - Community
 Engine:
  Version:          20.10.13
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.15
  Git commit:       906f57f
  Built:            Thu Mar 10 14:06:05 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.10
  GitCommit:        2a1d4dbdb2a1030dc5b01e96fb110a9d9f150ecc
 runc:
  Version:          1.0.3
  GitCommit:        v1.0.3-0-gf46b6ba
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
~~~
{: .output}

To confirm `cwltool` is installed, run the following command to display the version number:
~~~
cwltool --version
~~~
{: .language-bash}

You should see something similar to the output shown below.
~~~
/home/learner/env/bin/cwltool 3.1.20220224085855
~~~
{: .output}

To confirm `graphviz` is installed, run the following command to display the version number:
~~~
dot -V
~~~
{: .language-bash}

You should see something similar to the output shown below.
~~~
dot - graphviz version 2.43.0 (0)
~~~
{: .output}


## Containers

To avoid having to wait during the class, please run the following which will download all the
required software containers.

~~~
docker pull quay.io/biocontainers/star:2.7.5c--0
docker pull quay.io/biocontainers/fastqc:0.11.5--hdfd78af_5
docker pull quay.io/biocontainers/cutadapt:3.7--py39hbf8eff0_1
docker pull quay.io/biocontainers/samtools:1.14--hb421002_0
docker pull quay.io/biocontainers/subread:1.5.0p3--0
~~~
{: .language-bash}

## Files

You will need to download some example files for this lesson. In this tutorial we will use RNA sequencing data.

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
wget https://zenodo.org/record/4541751/files/GSM461177_1_subsampled.fastqsanger
wget https://zenodo.org/record/4541751/files/GSM461177_2_subsampled.fastqsanger
wget https://zenodo.org/record/4541751/files/GSM461180_1_subsampled.fastqsanger
wget https://zenodo.org/record/4541751/files/GSM461180_2_subsampled.fastqsanger
wget https://zenodo.org/record/4541751/files/Drosophila_melanogaster.BDGP6.87.gtf
wget https://hgdownload.soe.ucsc.edu/goldenPath/dm6/bigZips/dm6.fa.gz
gunzip dm6.fa.gz  # STAR index requires an uncompressed reference genome
~~~
{: .language-bash}

###  STAR Genome index
To run the STAR aligner tool, index files generated from the reference genome are needed.
The index files can be downloaded, or generated yourself from the unindexed reference genome.
These two options are detailed below -- choose the one most appropriate to your set-up.

1. Download the index

   At least 9 GB of memory is required to generate the index, which will occupy 3.3GB of disk.

   If your computer doesn't have that much memory, then you can download the directory
   by running the following in the `rnaseq` directory:
   ~~~
   wget https://dataverse.nl/api/access/datafile/266295 -O - | tar xJv
   ~~~
   {: .language-bash}

2. Generate the index yourself

   To generate the genome index yourself: create a new file named `dm6-star-index.yaml`
   in the the `novice-tutorial-exercises` directory with the following contents:
   ~~~
   InputFiles:
     - class: File
       location: rnaseq/dm6.fa
       format: https://edamontology.org/format_1929  # FASTA
   IndexName: 'dm6-STAR-index'
   Overhang: 36
   Gtf:
     class: File
     location: rnaseq/Drosophila_melanogaster.BDGP6.87.gtf
   ~~~
   {: .language-yaml}

   Next use the CWL reference runner `cwltool` that you installed above and the CWL description
   for the indexing mode of the STAR aligner that was downloaded in the `bio-cwl-tools` directory
   to index the genome and place the result in the `rnaseq` directory alongside the other files:
   ~~~
   cwltool --outdir rnaseq/ bio-cwl-tools/STAR/STAR-Index.cwl dm6-star-index.yaml
   ~~~
   {: .language-bash}
   It should take 10-15 minutes for the index to be generated.

> ## STAR Index Memory Requirements
> To generate the genome index you will need at least 9 GB of RAM.
>
> If you do not allocate enough RAM the tool will not crash, but the process will stick
> on the following step:
> ```
> ... sorting Suffix Array chunks and saving them to disk...
> ```
> If this step does not finish within 10 minutes then it is likely the process has failed,
> and should be cancelled.
>
> MacOS users can configure  Docker Desktop to allocate more memory
> from the menu "Preferences" and then selecting "Resources".
>
> Windows users can configure WSL 2 to allocate more memory by opening the PowerShell
> and entering the following:
> ```
> # turn off all wsl instances such as docker-desktop
> wsl --shutdown
>
> notepad "$env:USERPROFILE/.wslconfig"
> ```
> In `.wslconfig` add the following
> ```
> [wsl2]
> memory=9GB
> ```
>
> Save the file and right-click the Docker icon in the notifications area (or System tray)
> and then click "Restart Docker…"
{: .callout}

{% include links.md %}
