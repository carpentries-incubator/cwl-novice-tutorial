---
title: "Documentation and Citation in Workflows"
teaching: 0
exercises: 0
questions:
- "How to document your workflow?"
- "How to cite research software in your workflow?"
objectives:
- "Documentation Objectives:"
- "explain the importance of documenting a workflow"
- "use description fields to document purpose, intent, and other factors at multiple levels within their workflow"
- "recognise when it is appropriate to include this documentation"
- "Citation Objectives:"
- "explain the importance of correctly citing research software"
- "give credit for all the tools used in their workflow(s)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---
By the end of this episode,
learners should be able to
__document their workflows to increase reusability__ and __explain the importance of correctly citing research software__.

**TODO (CITE): define some specific objectives to capture the skills being taught in this section.**

See [this page](https://www.commonwl.org/v1.1/CommandLineTool.html#SoftwarePackage).



## Finding an identifier for the tool

(Something about permanent identifiers insert here)

When your workflow is using a pre-existing command line tool, it is good practice to provide
_citation_ for the tool, beyond which command line it is executed with.

The `SoftwareRequirement` hint can list named `packages` that should be installed in order to run the tool.
So for instance if you installed using the package management system with `apt install bamtools` the package `bamtools` can be
cited in CWL as:

```yaml
hints:
  SoftwareRequirement:
    packages:
      bamtools: {}
```


### Adding version

Q: `bamtools --version` prints out `blablabla 2.3.1` - how would you indicate in CWL that this is the version of BAMTools the workflow was tested against?

A:

```yaml
hints:
  SoftwareRequirement:
    packages:
      bamtools:
          version: ["2.3.1"]
```


### Adding Permanent identifiers

To help identify the tool across package management systems we can also add _permanent identifiers_ and URLs, for instance to:

 - RRID to SciCrunch
 - bio.tools registration
 - DOI to a publication
 - Homepage
 - source repository (e.g. GitHub)

These can be added to the `specs` list:

```yaml
hints:
  SoftwareRequirement:
    packages:
      interproscan:
        specs: [ "https://identifiers.org/rrid/RRID:SCR_005829" ]
        version: [ "5.21-60" ]
```

### How to find a RRID permanent identifier

[RRID](https://scicrunch.org/resources) provides identifiers for many commonly used resources tools in bioinformatics. For instance, a [search for BAMtools](https://scicrunch.org/resources/Any/search?q=bamtools) finds an [entry for BAMtools](https://scicrunch.org/resources/Any/record/nlx_144509-1/SCR_015987/resolver?q=bamtools&l=) with identifier `RRID:SCR_015987` and additional information.

We can transform the RRID into a _Permanent Identifier_ (PID) for use in CWL using [http://identifiers.org/](http://identifiers.org/) by appending the RRID to `https://identifiers.org/rrid/` - making the PID <https://identifiers.org/rrid/RRID:SCR_015987> which we see resolve to the same SciCrunch entry, and add to our `specs` list:

```
hints:
  SoftwareRequirement:
    packages:
      interproscan:
        specs: [ "https://identifiers.org/rrid/RRID:SCR_015987" ]
```

Note that as CWL is based on YAML we use `"quotes"` to escape these identifiers include the `:` character.

### Finding bio.tools identifiers

As an alternative to RRID we can add identifiers from the ELIXIR Tools Registry https://bio.tools/ - for instance <https://bio.tools/bamtools>

```yaml
hints:
  SoftwareRequirement:
    packages:
      bamtools:
        specs:
          - "https://identifiers.org/rrid/RRID:SCR_015987"
          - "https://bio.tools/bamtools"
```


* How to write a DOI as a PID URI
  https://www.nature.com/articles/nmeth.1923 -> https://doi.org/ + 10.1038/nmeth.1923 -> https://doi.org/10.1038/nmeth.1923

### Package manager identifiers

Q: You have used `apt install bamtools` in the Linux distribution Debian 10.8 "Buster". How would you in CWL `SoftwareRequirement` identify the [Debian package recipe](https://www.debian.org/distrib/packages), and with which `version`?

A:

```yaml
hints:
  SoftwareRequirement:
    packages:
      bamtools:
        specs:
          - "https://identifiers.org/rrid/RRID:SCR_015987"
          - "https://bio.tools/bamtools"
          - "https://packages.debian.org/buster/bamtools"
        version: ["2.5.1", "2.5.1+dfsg-3"]
```

This package repository has a URI for each installable package, depending on the distribution, we here pick `"buster"`. While the [upstream GitHub repository of bamtools](https://github.com/pezmaster31/bamtools/releases/tag/v2.5.1) has release version `v2.5.1`, the Debian packaging adds `+dfsg-3` to indicate the 3rd repackaging with additional [patches](https://sources.debian.org/patches/bamtools/2.5.1+dfsg-3/), in this case to make the software comply with [Debian Free Software Guidelines](https://www.debian.org/social_contract.html#guidelines) (`dfsg`).

Under `version` list in CWL we'll include `2.5.1` which is the upstream version, ignoring everything after `+` or `-` according to [semantic versioning](https://semver.org/) rules. As an optional extra you can also include the Debian-specific version `"2.5.1+dfsg-3"` to indicate which particular packaging we tested the workflow with at the time.



## Exercise: There is a "obvious" DOI

Q: You have a workflow using bowtie2, how would you add a citation?

A:
```yaml
hints:
  SoftwareRequirement:
    packages:
      bowtie2:
        specs: [ "https://doi.org/10.1038/nmeth.1923" ]
        version: [ "1.x.x" ]
```


RRID for bowtie2

RRID:SCR_005476 ->
https://scicrunch.org/resolver/RRID:SCR_005476 #bowtie not bowtie2
https://identifiers.org/rrid/ + RRID ->
https://identifiers.org/rrid/RRID:SCR_005476  PID


https://bio.tools/bowtie2

http://bioconda.github.io/recipes/bowtie2/README.html
vs.
https://anaconda.org/bioconda/bowtie2





Giving clues to reader

Authorship/citation of a tool vs the CWL file itself (particularly of a workflow)

Add identifiers under requirements?
https://www.commonwl.org/user_guide/20-software-requirements/index.html

SciCrunch - looking up RRID for Bowtie2
Then bio.tools

```yaml
hints:
  SoftwareRequirement:
    packages:
      interproscan:
        specs: [ "https://identifiers.org/rrid/RRID:SCR_005829",
                 "http://somethingelse"]
        version: [ "5.21-60" ]
```



## Trickier: Only Github and homepage
```yaml
s:codeRepository:
```

```yaml
hints:
  SoftwareRequirement:
    packages:
      interproscan:
        specs: [ "https://github.com/BenLangmead/bowtie2"]
        version: [ "fb688f7264daa09dd65fdfcb9d0f008a7817350f" ]
```

No version, add commit ID or date instead as `version`

--> (How to make Your own tool citable?)


## Getting credit for your CWL files


NOTE: Difference between credit for this CWL file vs credit for the tool it calls.


```yaml
s:author "Me"
s:dateModified: "2020-10-6"
s:version: "2.4.2"
s:license: https://spdx.org/licenses/GPL-3.0
```


https://www.commonwl.org/user_guide/17-metadata/index.html

Using `s:citation`?

something like..

```yaml
s:citation: https://dx.doi.org/10.1038/nmeth.1923

s:url: http://example.com/tools/

s:codeRepository: https://github.com/BenLangmead/bowtie2
$namespaces:
  s: https://schema.org/

$schemas:
 - http://schema.org/version/9.0/schemaorg-current-http.rdf
```

---> Need new guidance on how to publish workflows, making DOIs in Zenodo, Dockstore etc.
https://docs.bioexcel.eu/cwl-best-practice-guide/devpractice/publishing.html
https://guides.github.com/activities/citable-code/

How to do it properly to improve findability.

How to publisize CWL tools


# CWL workflow descriptions

About how to wire together CommandLineTool steps in a cwl Workflow file.


{% include links.md %}
