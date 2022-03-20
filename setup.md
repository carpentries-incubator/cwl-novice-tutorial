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
### VSCode and Benten

1. Download and install [VSCode](https://code.visualstudio.com/).

2. [Open Benten in the marketplace][Benten] and click the `Install` button or follow the directions.


### Docker, cwltool, and graphviz

3. Install and configure Windows Subsystem for Linux 2 (WSL2), and Docker Desktop
   1. First complete all of the [WSL 2 "Prerequisites"][docker-desktop-wsl2-prereqs].
   2. Then install [Debian from the Microsoft Store][install-debian].
   3. Open the "Debian" app and setup your Debian user and password when prompted.

      You can now close the "Debian" window.
   4. Open PowerShell as Administrator ("Start menu" > "PowerShell" > right-click > "Run as Administrator")
      and run the following command to set Debian as your default WSL 2 distro:

      `wsl --set-default debian`
   5. Then continue to [download Docker Desktop][download-docker-desktop] and run the installer.
   6. Run Docker Desktop, from the top menu choose ["Settings" > "Resources" > "WSL Integration"][docker-screenshot]
      and under "Enable integration with additional distros" select "Debian"
4. Configure VS Code
   1. Open [this link][remote-wsl-extension] to install the "Remote - WSL" extension for VS Code by clicking the `Install` button or by following the directions.
   2. After installation, in VS Code choose "Open a Remote - WSL Window" and then "New WSL Window".

      If you don't see those option, then press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> and then type
      "WSL" and they should appear at the top of the screen.
   3. There should now be a second VS Code window that has "WSL: Debian" in green at the lower left corner.
      You can close the original VS Code window.
   4. Enable the Benten CWL extension in this "WSL : Debian" window
      * press <kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>X</kbd> to open the "Extensions" pane.
      * Look for "CWL (Rabix/Benten)" and click the blue "Install in WSL: Debian" button.
5. Open a terminal
   * Choose "Terminal" > "New Terminal" from the menu in the "WSL : Debian" VS Code window.
   * Copy the following `sudo apt-get update && sudo apt-get install -y python3-venv wget`
   * Paste it into the terminal window
   * Press <kbd>Return</kbd> to run it.
   > ## Note!
   > All references to a "terminal" for the rest of this tutorial are to this terminal window inside
   > the "WSL : Debian" Visual Studio Code windowd, and not Powershell, the Windows Command Prompt,
   > nor the "Debian" app.
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
7.  Later we will make visualisations of our workflows. To suport that we need to install graphviz:
    ~~~
    sudo apt-get install -y graphviz
    ~~~
    {: .language-bash}
</article>

<article role="tabpanel" class="tab-pane" id="linux">
### VSCode and Benten

1. Download and install [VSCode](https://code.visualstudio.com/).

2. [Open Benten in the marketplace][Benten] and click the `Install` button or follow the directions.


### Docker, cwltool, and graphviz

3. [Install docker][install-docker-linux]
4. [Enable docker usage as a non-root user][docker-postinstall-linux]
5. Install the latest version of cwltool.
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
6.  Later we will make visualisations of our workflows. To suport that we need to install graphviz:
    ~~~
    sudo apt-get install -y graphviz
    ~~~
    {: .language-bash}
</article>

<article role="tabpanel" class="tab-pane" id="macos">
### VSCode and Benten

1. Download and install [VSCode](https://code.visualstudio.com/).

2. [Open Benten in the marketplace][Benten] and click the `Install` button or follow the directions.

### Docker, cwltool, and graphviz
3. [Install docker][install-docker-macos]
4. [Install miniconda][miniconda-macos]
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

To confirm cwltool is installed, run the following command to display the version number:
~~~
cwltool --version
~~~
{: .language-bash}

You should see something similar to the output shown below.
~~~
/home/learner/env/bin/cwltool 3.1.20220312132609
~~~
{: .output}

To confirm graphviz is installed, run the following command to display the version number:
~~~
dot -V
~~~
{: .language-bash}

You should see something similar to the output shown below.
~~~
dot - graphviz version 2.40.1 (20161225.0304)
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
[cwl-windows-install]: https://github.com/common-workflow-language/cwltool#ms-windows-users
[docker-desktop-wsl2-prereqs]: https://docs.docker.com/desktop/windows/wsl/#prerequisites
[download-docker-desktop]: https://desktop.docker.com/win/main/amd64/Docker%20Desktop%20Installer.exe
[install-debian]: https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6
[docker-screenshot]: https://docs.docker.com/desktop/windows/images/wsl2-choose-distro.png
[install-docker-macos]: https://docs.docker.com/desktop/mac/install/
[miniconda-macos]: https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html
[install-docker-linux]: https://docs.docker.com/engine/install/
[docker-postinstall-linux]: https://docs.docker.com/engine/install/linux-postinstall/#manage-docker-as-a-non-root-user
[Benten]: https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl
[remote-wsl-extension]: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-wsl
