---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS: a cure to pipelineitis (under development)"
ghcommentid: 8
date: "2017-12-15"
tags: ["SoS", "Workflow"]
---


A phenomenon has puzzled me for quite some time when I started my career as a bioinformatician, that is to say,
**everyone knows the benefits of workflow systems, everyone has heard of and talks about the power and popularity of
some workflow systems, but yet no one I know actually uses a workflow system for daily computational research.**
People programm in different scripting languages using their favoriate langauges and IDEs, execute them locally or
submit them to cluster system with shell and other scripts, all without the use of a workflow system.

## Why workflow systems are not used in daily computational research

When I started to analyze more and more bioinformatic datasets and wrote more and more scripts
for different projects, I became increasingly annoyed by many problems with developing, running, managing,
sharing, and reproducing bioinformatic data analyses. Notably,

1. **Scale to large datasets:** It is tedious and error-prone to write scripts to analyze larger and larger
  amount of data as more and more repetitive work is needed for the "execution" part of the analysis.
2. **Ability to share and reproduce:** With more code on the execution side, the analysis becomes
  less and less readable, and more and more difficult to share and reproduce.
3. **Remote execution**: Executing parts of the analysis on different systems (e.g. clusters)
  causes extra work (e.g. wrapper script to submit jobs) and leads to the fragmentation of analysis
  workflows, worsen the readability and reproducibility problem.

These problems are right in the domain of workflow systems so I set out to look for a suitable workflow system.
However, after surveying all [100+ workflow systems](https://github.com/pditommaso/awesome-pipeline) and trying
more than 10 of them, I ended up doing what other people are doing, writing more disposable scripts. The reason
was simple, **the overhead of applying a workflow system in daily data analysis is too high to help productivity**.
As a matter of fact, as pointed out by [Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740),
using a workflow system for analyses that will be run only once, or not meant to be shared with or repeated
by others can decrease creativity and productivity.

> #### Pipelineitis is a nasty disease
>
> A pipeline is a series of steps, or software tools, run in sequence according to a predefined plan.
> Pipelines are great for running exactly the same set of steps in a repetitive fashion, and for 
> sharing protocols with others, but **they force you into a rigid way of thinking and can decrease creativity**.
>
> Warning: **don’t pipeline too early**. Get a method working before you turn it into a pipeline. And
> even then, does it need to be a pipeline? Have you saved time? Is your pipeline really of use to
> others? If those steps are only ever going to be run by you, then a simple script will suffice and 
> **any attempts at pipelining will simply waste time**. Similarly, if those steps will only ever be run
> once, just run them once, document the fact you did so and move on.

## Pipelineitis is a treatable

Pipelineitis is a nasty disease but admiting the facts does not save us from all the aforementioned problems
we have in daily data analysis. Looking over and over at the key obstacles that prevent the application of
workflow systems in daily computational research, we developed [SoS (Script of Scripts)](https://vatlab.github.io/sos-docs/),
which is a workflow engine with a multi-language notebook frontend, to treat pipelineitis. 
The following table summarizes the major features of SoS and how they can help the application of
workflow systems in daily computational research:

Symptom | Cause | Treatment
---|---| ---|
**Lower productivity** | Re-create data analysis using another language or environment | <ul><li>A <a href="https://vatlab.github.io/blog/post/sos-notebook/">single environment</a> for both interactrive data analysis and batch data processing</li><li><a href="https://vatlab.github.io/blog/post/power-of-sos-plus-sos-notebook/">Easy transition</a> from notebook to workflow</li></ul>
**Steep learnin curve** | Workflow language difficult to learn and use, making it difficult to adopt | <ul><li>Workflow as "annotations" to Python scripts</li><li>Trivial to get started, grow as needed</li></ul>
**Difficult to read and share** | Data analysis logics (core steps)<ul><li>buried in execution logics</li><li>or written in foreign workflow language</li><li>or hidden behind workflow GUI</li></ul>|<ul><li>Almost verbatim inclusion of scripts</li><li>Simple workflow directives</li><li>Single multi-language notebook with annotated workflow</li></ul>
**Difficult to reproduce** | Data analysis scattered in multiple scripts for multiple systems | <ul><li>Ability to execute entire or parts of workflows remotely</li><li>Keep local and remote tasks in one notebook</li></ul>

## An IDE for both interactive data analysis and workflow development

A complex workflow inevitably involves scripts in multiple languages. Existing workflow systems generally
do not provide an interactive environment for the development of these scripts. Even for workflow systems
that provide nice GUIs, the graphical interfaces are used to construct workflows from mature scripts, not
for the development of workflow steps. Consequently, users are required to develop workflow steps in
other environments before wrapping them to a workflow system.

**SoS Notebook is a multi-language IDE for the SoS workflow engine** that provides an interactive
environment for all stages of workflow development. As demonstrated in blog
posts [SoS Notebook: One Notebook, Multiple Kernels](https://vatlab.github.io/blog/post/sos-notebook/) and
[Combined power of SoS and SoS Notebook](https://vatlab.github.io/blog/post/power-of-sos-plus-sos-notebook/),
SoS Notebook allows you to:

* Develop scripts in different languages using multiple kernels,
  with added features such as string interpolation 
  (magic [%expand](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html#magic-expand))
  and line-by-line execution. 

![step through](https://vatlab.github.io/sos-docs/doc/media/step_through.gif)

* Convert scripts to SoS "actions" to be executed as (scratch) workflow steps. The "actions"
  are Python functions and can be debugged in the SoS kernell

* "Chain" the scratch workflow steps into SoS workflows by adding `input`, `output`, `depends`
  statements and section headers.

![SoS notebook with SoS](https://vatlab.github.io/sos-docs/doc/media/SoS_Workflow.gif)

**The SoS environment makes it easy to convert scripts developed in interactive
data analysis to a workflow** which removes the biggest obstacle in the utilization 
of workflows in daily computational reserach.

## Workflow syntax as "annotations" to Python scripts

SoS is extended from Python 3.6 so **a SoS script is essentially a Python script with
additional workflow specifications**. The SoS syntax is very simple and can be summarized
as follows:

Syntax | Example | Usage |
---|---| ---|
Script format of function call | <pre>sh:<br>  echo "I am sh"</pre> | Calling a Python function with multi-line script as first parameter |  
Section header | <pre>[step_10]</pre> | Define workflow steps |
Parameter definition | <pre>parameter: cutoff=5</pre> | Define command line argument |
Step input, output, and depends | <pre>input: "a.txt"</pre> | Define input, output, and dependent targets of steps |
Task | <pre>task: walltime='24h'</pre> | Define external tasks | 

As shown in the following figure, a SoS workflow can be created as verbatim inclusion of
scripts in different languages. The script can be annotated with additional workflow 
syntax to make use of more and more workflow features such as runtime signature and
external execution of tasks, but the workflow remains readable because the key parts
of the workflow, namely the included scripts, remain largely untouched.

![SoS notebook with SoS](https://vatlab.github.io/sos-docs/doc/media/sos_syntax.gif)

The following videos explains these steps in detail

{{< youtube E5Eh7BVbbTM >}} 

{{< youtube roTDvSXSPgU >}} 

It worth mentioning that SoS supports both forward-style procedure-oriented and
makefile-style outcome-oriented workflows. The forward-style workflows are specified
as numerically ordered steps that will be executed sequentially (logically speaking),
and makefile-style workflows are specified as steps that provides targets that are
used by others. 

## Remote execution made easy

Showing task list.

Point to single notebook with local and remote scripts

<small>
SoS Notebook is currently implemented as a Jupyter kernel but we will certainly port it to JupyterLab after JupyterLab matures
([JupyterLab/2815](https://github.com/jupyterlab/jupyterlab/issues/2815)).
More details of SoS and SoS Notebook can be found at the [SoS website](https://vatlab.github.io/sos-docs/) where you can
find tons of documentations, tutorials, examples, and youtube videos. Please test SoS Notebook and send
your feedback and/or bug reports to our [github issue tracker](https://github.com/vatlab/sos-notebook/issues). 
If you find SoS Notebook useful, please support the project by starring the [SoS](https://github.com/vatlab/SoS) and
[SoS Notebook](https://github.com/vatlab/sos-notebook)
github projects, or spreading the word with [twitter](https://twitter.com/ScriptOfScripts). </small>
