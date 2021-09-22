---
title: Setup
---

## Files

You will need to install some example files for this lesson. These will be added below.

- VSCode + https://github.com/rabix/benten


## Software

These lessons are designed for running on a UNIX based system (either linux or OSX), they have not been tested on Windows.

Python3 and the CWL reference runner are required for this lesson. We recommend that you use [Anaconda](https://www.anaconda.com/distribution/) for installing these.
Once you have installed Anaconda you can create a virtual environment which includes the necessary libraries using the command:
~~~
$ conda create -n cwl -c conda-forge cwl_runner
~~~
{: .language-bash}

That virtual environment can be activated after creation using the command:
~~~
$ conda activate cwl
~~~
{: .language-bash}
After this the required tools should be available.




{% include links.md %}
