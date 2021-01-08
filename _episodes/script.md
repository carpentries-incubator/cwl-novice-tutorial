---
title: "Adding your own script to a step"
teaching: 0
exercises: 0
questions:
- "How to include and run a script in a step at runtime?"
- "Which requirements need to be specified?"
- "How to capture output of a script?"
objectives:
- "Include and run a script in a step at runtime"
- "Capture output of a script"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

By the end of this episode,
learners should be able to
__include and run their own script in a step at runtime__

> ## Exercise 1:
>
> Complete the `requirements` field for the `CommandLineTool` that creates a script at runtime. As a HINT, `DockerRequirement` has already been filled-in.
>
> class: CommandLineTool
> cwlVersion: v1.1
> requirements:
>   DockerRequirement:
>     dockerPull: 'debian:stable'
>   -----------------:
>
> > ## Solution
> > 
> > class: CommandLineTool
> > cwlVersion: v1.1
> > requirements:
> >    DockerRequirement:
> >      dockerPull: 'debian:stable'
> >    InitialWorkDirRequirement:
> > 
> {: .solution}
{: .challenge}

> ## Exercise 2:
> 
> Create an input field (with appropriate `type`) that is going to be referred within the script.
>
> > ## Solution:
> >
> > inputs:
> >   message:
> >     type: string
> > 
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> Identify `CWL` keywords for defining script name `script.sh` and the contents in this script.
>
> InitialWorkDirRequirement:
>   listing:
>     - ------: script.sh
>       ------: |
>         echo "*Documenting input*" && \
>         echo "Input received: $(inputs.message)" && \
>         echo "Exit"
>
> > ## Solution:
> >
> > InitialWorkDirRequirement:
> >   listing:
> >     - entryname: script.sh
> >       entry: |
> >         echo "*Documenting input*" && \
> >         echo "Input received: $(inputs.message)" && \
> >         echo "Exit"
> > 
> {: .solution}
{: .challenge}

> ## Exercise 5:
> 
> Since we are using `echo` in the script - what is the apprproiate `CWL` feature to capture standard output?
>
> > ## Solution:
> >
> > stdout: "message.txt"
> > 
> {: .solution}
{: .challenge}

> ## Exercise 6:
> 
> Create an output variable to capture this tools' output and identify an appropriate `type` for it.
>
> > ## Solution:
> >
> > outputs:
> >   doc:
> >     type: stdout
> >
> {: .solution}
{: .challenge}

> ## Exercise 7:
>
> Specify the `baseCommand` required to execute the script.
>
> > ## Solution:
> >
> > baseCommand: ["sh", "script.sh"]
> > 
> {: .solution}
{: .challenge}

> ## Exercise 8:
>
> How can we capture the script we have created along with tools' standard output using `outputs` section.
>
> > ## Solution:
> > outputs:
> >   script:
> >     type: File
> >     outputBinding:
> >       glob: "*.sh"
> >
> {: .solution}
{: .challenge}


{% include links.md %}
