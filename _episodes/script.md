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
> Identify `requirements` for the `CommandLineTool`. HINT: `'ubuntu:latest'` can be used as docker container.
>
> > ## Solution
> > 
> > requirements:
> >    DockerRequirement:
> >      dockerPull: 'ubuntu:latest'
> >    InlineJavascriptRequirement: {}
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
> Define name and content of the script (in this exercise we will create a bash script that uses `echo` on the input `message`)
>
> > ## Solution:
> >
> > InitialWorkDirRequirement:
> >   listing:
> >     - entryname: myscript.sh
> >       entry: |
> >         echo "Start of message" && \
> >         echo "My message: $(inputs.message)" && \
> >         echo "End of message"
> > 
> {: .solution}
{: .challenge}

> ## Exercise 4:
> 
> Create the output variable for the `CommandLineTool` and identify an appropriate `type` for it.
>
> > ## Solution:
> >
> > outputs:
> >   message:
> >     type: stdout
> >
> {: .solution}
{: .challenge}

> ## Exercise 5:
>
> Specify the `baseCommand` required to execute the script.
>
> > ## Solution:
> >
> > baseCommand: ["sh", "myscript.sh"]
> > 
> {: .solution}
{: .challenge}

> ## Exercise 6:
>
> How can we capture script along with tool outputs using `outputs` section.
>
> > ## Solution:
> > outputs:
> >   bash_file:
> >     type: File
> >     outputBinding:
> >       glob: "*.sh"
> >
> {: .solution}
{: .challenge}


{% include links.md %}
