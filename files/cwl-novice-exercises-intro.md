# Introduction to Workflows with CWL: Exercises

**Toby Hodges**

2020-05-12

Slides licensed CC-BY

---
## Acknowledgements

![](images/TheCarpentries.svg)

**Teaching Tech Together** (teachtogether.tech) by Greg Wilson

---
## Designing exercises

- we have draft learning objectives
- goal: design (at least) one exercise to assess learner progress towards each objective
- exercises for _formative assessment_ i.e. assessment of learning during the tutorial

---
## Designing exercises

- for example, if learning objective is

> describe essential job input parameters in a YAML file

a suitable "fill in the blanks" exercise might be:

> fill in the blanks in the input YAML below:
>
> ```yaml
> index_file:
>   class: ___
>   ___: mouse.idx
> ```

---
## Designing exercises

- exercises should help you and the learner assess whether they've learned what you've just taught them
  - the example on the previous page would check whether the learner understood that a file input must be of class `File` and have a `path`
- incorrect answers should have _diagnostic power_ i.e. should help you identify what the learner has misunderstood/missed, so you can fix it
- exercises should be regularly spaced throughout the material (at least one every ~15 minutes)

---
## Designing exercises - potentially suitable exercise formats

- Multiple choice questions
- Code & run
- Fill in the blanks
- Minimal fix
- Theme & variations
- Labeling a diagram

(Examples taken from Greg Wilson's [Teaching Tech Together](http://teachtogether.tech/#s:exercises))

---
## Potentially suitable exercise formats

**Multiple choice questions**, e.g.

> In what order do operations occur when the computer evaluates the expression `price = addTaxes(cost - discount)`?
>
> 1. subtraction, function call, assignment
> 2. function call, subtraction, assignment
> 3. function call, then assignment and subtraction simultaneously
> 4. none of the above

- each incorrect answer should be a _plausible distractor_

---
## Potentially suitable exercise formats

**Code & run**, e.g.

> The variable `picture` contains a full-color image read from a file. Using one function, create a black and white version of the image and assign it to a new variable called `monochrome`.

- be careful! it's easy to make code & run exercises too difficult for novices

---
## Potentially suitable exercise formats

**Fill in the blanks**, e.g.

> Fill in the blanks so that the code below prints the string ’hat’.
>
> ```python
> text = 'all that it is'
> slice = text[____:____]
> print(slice)
> ```

- filling in the blanks is much less intimidating for novices and helps them to focus only on the specific thing you'd like to assess

---
## Potentially suitable exercise formats

**Minimal fix**, e.g.

> This function is supposed to test whether a number lies within a range. Make one small change so that it actually does so.
>
> ```python
> def inside(point, lower, higher):
>   if (point <= lower):
>     return false
>   elif (point <= higher):
>     return false
>   else:
>     return true
> ```

- helps learners to develop debugging skills.

---
## Potentially suitable exercise formats

**Theme & variations**, e.g.

> Change the inner loop in the function below so that it fills the upper left triangle of an image with a specified color.
>
> ```bash
> function fillTriangle(picture, color) is
>   for x := 1 to picture.width do
>     for y := 1 to picture.height do
>       picture[x, y] = color
>     end
>   end
> end
> ```

- helps learners develop a key skill: adapting existing code for a new purpose

---
## Potentially suitable exercise formats

**Labeling a diagram**, e.g.

> Figure 1 shows how a small fragment of HTML is represented in memory. Put the labels 1–9 on the elements of the tree to show the order in which they are reached in a depth-first traversal.

-  probably better suited to a physical classroom than online/self-directed learning

---
## Lesson infrastructure

- the lesson template is built with Jekyll
  - hopefully, you won't need to care about this very much
  - but you will need to install a few things to run local test builds
- lesson content is written with Markdown
  - you will need to care about this...

---
## Lesson infrastructure

- an aside regarding terminology:
  - _lesson_ = the whole tutorial
  - _episodes_ = individual sections (should constitute ~30 mins of teaching time)
  - _extras_ = supplementary material e.g. glossary of terms, instructor notes, etc
- episode files are stored in the `_episodes` directory
  - I've made one for each cluster of learning objectives
  - new `.md` files added to this directory will automatically be added to the site & menu
- add exercises to these episode files

---
## Lesson infrastructure

- exercises should be written as tagged blockquotes, e.g.

```markdown
> ## Example rendered exercise
>
> This is the body of the challenge.
>
> > ## Solution
> >
> > This is the body of the solution.
> {: .solution}
{: .challenge}
```

- this will take care of the styling and the dropdown to reveal the exercise solution

---
## Lesson infrastructure

- more info about site infrastructure here:
  - [The Carpentries Curriculum Development Handbook - 8: Technological Introductions](https://carpentries.github.io/curriculum-development/technological-introductions.html#using-the-lesson-template)

---
## How to contribute

- head to [the GitHub repository](https://github.com/common-workflow-lab/cwl-novice-tutorial/)
- post in [the relevant issue](https://github.com/common-workflow-lab/cwl-novice-tutorial/issues/7) to let others know which objective you're working on
- submit one pull request per exercise

---
## Q&A
