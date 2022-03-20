---
title: Setup
---

## Software Setup

These lessons assume that you are using the freely available Visual Studio Code application with the
Benten extension along with the CWL reference runner (`cwltool`).

Please follow the instructions for your computer's operating system by clicking on the relevant tab below.

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

      > ## Note
      > To check your Windows version and build number, press the Windows logo key + <kbd>R</kbd>, type winver, select OK.
        You can update to the latest Windows version by selecting "Start" > "Settings" > "Windows Update" > "Check for updates".
      {: .callout }
   2. Open PowerShell as Administrator ("Start menu" > "PowerShell" > right-click > "Run as Administrator")
      and paste the following command followed by <kbd>Enter</kbd> to install WSL 2:

      `wsl --install`
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
   1. Open [this link](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ws){:target="_blank"}{:rel="noopener noreferrer"}
      to install the "Remote - WSL" extension for VS Code by clicking the `Install` button or by following the directions.
   2. After installation, in VS Code choose "Open a Remote - WSL Window" and then "New WSL Window".

      If you don't see those option, then press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and then type
      "WSL" and they should appear at the top of the screen.
   3. There should now be a second VS Code window that has "WSL: Ubuntu" in green at the lower left corner.
      You can close the original VS Code window.
   4. To enable the Benten CWL extension in this "WSL : Ubuntu" window:
      * Press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd> to open the "Extensions" pane.
      * Look for "CWL (Rabix/Benten)" and click the blue "Install in WSL: Ubuntu" button.
5. Open a terminal and install tutorial prerequisites
   * Choose "Terminal" > "New Terminal" from the menu in the "WSL : Ubuntu" VS Code window.
   * Copy the following `sudo apt-get update && sudo apt-get install -y python3-venv wget graphviz`
   * Paste it into the terminal window
   * Press <kbd>Return</kbd> to run it. You will need to use the UNIX password you set earlier.

   > ## Note!
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

      > ## Note!
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

      > ## Note!
      > Every time you launch VS Code or launch a new terminal, you must run `source env/bin/activate`
      > to re-enable access to this Python Virtual Environment.
      {: .callout}
    2. Next, install cwltool by running the following in the terminal:
       ~~~
       pip install cwltool
       ~~~
       {: .language-bash}
6.  Later we will make visualisations of our workflows. To support that we need to install graphviz.
    Here is the command for Debian-based Linux systems:
    ~~~
    sudo apt-get install -y graphviz
    ~~~
    {: .language-bash}

    For other Linux systems, check <https://graphviz.org/download/#linux>
</article>

<article role="tabpanel" class="tab-pane" id="macos">
### VSCode and Benten

1. Download and install [VSCode](https://code.visualstudio.com/){:target="_blank"}{:rel="noopener noreferrer"}.
2. [Open Benten in the marketplace](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl){:target="_blank"}{:rel="noopener noreferrer"} and click the `Install` button or follow the directions.
3. [Install docker](https://docs.docker.com/desktop/mac/install){:target="_blank"}{:rel="noopener noreferrer"}
4. [Install miniconda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html){:target="_blank"}{:rel="noopener noreferrer"}
5. Create a virtual environment using conda
    ~~~
    conda create --name cwltutorial
    ~~~
    {: .language-bash}
6. Activate the virtual environment
    ~~~
    conda activate cwltutorial
    ~~~
    {: .language-bash}
7. Install cwltool and graphviz using conda
    ~~~
    conda install -c bioconda cwltool
    conda install -c anaconda graphviz
    ~~~
    {: .language-bash}

> ## Note!
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
