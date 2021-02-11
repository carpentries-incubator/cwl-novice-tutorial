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
> ~~~
> class: CommandLineTool
> cwlVersion: v1.1
> requirements:
>   DockerRequirement:
>     dockerPull: 'debian:stable'
>   -----------------:
> ~~~
> {: .language-yaml }
>
> > ## Solution
> >
> > ~~~
> > class: CommandLineTool
> > cwlVersion: v1.1
> > requirements:
> >    DockerRequirement:
> >      dockerPull: 'debian:stable'
> >    InitialWorkDirRequirement:
> > ~~~
> > {: .language-yaml }
> >
> {: .solution}
{: .challenge}

> ## Exercise 2:
>
> Identify `CWL` keywords for defining script name `script.sh` and the contents of this script under `InitialWorkDirRequirement`.
>
> ~~~
> InitialWorkDirRequirement:
>   listing:
>     - ------: script.sh
>       ------: |
>         echo "*Documenting input*" && \
>         echo "Input received: $(inputs.message)" && \
>         echo "Exit"
>
> inputs:
>   message:
>     type: string
> ~~~
> {: .language-yaml }
>
> > ## Solution:
> >
> > ~~~
> > class: CommandLineTool
> > cwlVersion: v1.1
> > requirements:
> >    DockerRequirement:
> >
> > InitialWorkDirRequirement:
> >   listing:
> >     - entryname: script.sh
> >       entry: |
> >         echo "*Documenting input*" && \
> >         echo "Input received: $(inputs.message)" && \
> >         echo "Exit"
> >
> > inputs:
> >   message:
> >     type: string
> > ~~~
> > {: .language-yaml }
> >
> {: .solution}
{: .challenge}

> ## Exercise 3:
>
> Since we are using `echo` in the script (as shown below) - what is the appropriate `type` in the `outputs` section of following code block to capture standard output?
>
> ~~~
> class: CommandLineTool
> cwlVersion: v1.1
> requirements:
>   DockerRequirement:
>     dockerPull: 'debian:stable'
>   InitialWorkDirRequirement:
>     listing:
>       - entryname: script.sh
>         entry: |
>           echo "*Documenting input*" && \
>           echo "Input received: $(inputs.message)" && \
>           echo "Exit"
>
> inputs:
>   message:
>     type: string
>
> stdout: "message.txt"
>
> outputs:
>  message:
>    type: ----
> ~~~
> {: .language-yaml }
>
> Your options are:
> A. File
> B. Directory
> C. stdout
> D. string
>
> > ## Solution:
> >
> > C. stdout
> >
> {: .solution}
{: .challenge}

> ## Exercise 4:
>
> Fix the `baseCommand` in following code snippet to execute the script we have created in previous exercises.
>
> ~~~
> baseCommand: []
> ~~~
> {: .language-yaml }
>
> > ## Solution:
> >
> > ~~~
> > baseCommand: ["sh", "script.sh"]
> > ~~~
> > {: .language-yaml }
> >
> {: .solution}
{: .challenge}

> ## Exercise 5:
>
> CHALLENGE question. Extend the `outputs` section of the following CWLtool definition to capture the script we have created along with tools' standard output.
>
> ~~~
> class: CommandLineTool
> cwlVersion: v1.1
> requirements:
>   DockerRequirement:
>     dockerPull: 'debian:stable'
>   InitialWorkDirRequirement:
>     listing:
>       - entryname: script.sh
>         entry: |
>           echo "*Documenting input*" && \
>           echo "Input received: $(inputs.message)" && \
>           echo "Exit"
>
> inputs:
>   message:
>     type: string
>
> stdout: "message.txt"
> baseCommand: ["sh", "script.sh"]
>
> outputs:
>   message:
>     type: stdout
> ~~~
> {: .language-yaml }
>
> > ## Solution:
> >
> > ~~~
> > class: CommandLineTool
> > cwlVersion: v1.1
> > requirements:
> >   DockerRequirement:
> >     dockerPull: 'debian:stable'
> >   InitialWorkDirRequirement:
> >     listing:
> >       - entryname: script.sh
> >         entry: |
> >           echo "*Documenting input*" && \
> >           echo "Input received: $(inputs.message)" && \
> >           echo "Exit"
> >
> > inputs:
> >   message:
> >     type: string
> >
> > stdout: "message.txt"
> > baseCommand: ["sh", "script.sh"]
> >
> > outputs:
> >   message:
> >     type: stdout
> >   script:
> >     type: File
> >     outputBinding:
> >       glob: "script.sh"
> > ~~~
> > {: .language-yaml }
> >
> {: .solution}
{: .challenge}


{% include links.md %}
