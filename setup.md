---
title: Setup
---

## Files

You will need to install some example files for this lesson. __These could be downloaded as a git repository (perhaps first by copying a template repository?)__



## Software

These lessons are best followed using VSCode, and the Benten extension (which is a language server for CWL). We will also be using the CWL reference runner. Instructions for installing these are given below.

__These lessons are designed for running on a UNIX based system (either linux or OSX), they have not been tested on Windows.__

### VSCode and Benten installation

Download and install VSCode binaries from [https://code.visualstudio.com/download](https://code.visualstudio.com/download)

Open VSCode and search for [Benten](https://marketplace.visualstudio.com/items?itemName=sbg-rabix.benten-cwl) in the marketplace. The name of the client extension is Rabix/benten. Follow the usual method to install the extension.

The VSCode Benten extension will require the Benten server to be installed too. It will prompt you to do this the first time you activate the extension.


### CWL installation

Python3 and the CWL reference runner are required for this lesson. We recommend that you use [Anaconda](https://www.anaconda.com/distribution/) for installing these.
Once you have installed Anaconda you can create a virtual environment which includes the necessary libraries using the command:
~~~
$ conda create -n cwl -c conda-forge cwl_runner
~~~
{: .language-bash}

That virtual environment can be activated after creation using the command (within the terminal of VSCode):
~~~
$ conda activate cwl
~~~
{: .language-bash}
After this the required tools should be available.




{% include links.md %}
