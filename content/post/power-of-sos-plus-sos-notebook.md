---
type: "post"
draft: false
author: "Bo Peng"
title: "The combined power of SoS Notebook and SoS Workflow Engine"
ghcommentid: 7
date: "2017-11-29"
tags: ["Notebook", "Workflow Engine"]
---

After I announced the release of [SoS Notebook](https://github.com/vatlab/sos-notebook) as a third-party
multi-language kernel for [Jupyter](http://jupyter.org/), I was asked repeatedly (e.g. On [HackerNews](https://news.ycombinator.com/item?id=15852821), 
[AzureNotebooks](https://github.com/Microsoft/AzureNotebooks/issues/254#issuecomment-349722523), and reviews to our manuscript) the following question:

> **Why did not you use an existing multi-language notebooks 
(e.g. [Apache Zeppelin](https://zeppelin.apache.org/) and [BeakerX](https://github.com/twosigma/beakerx)) or
contribute a multi-language feature to the core of [Jupyter](http://jupyter.org/) or 
[JupyterLab](https://github.com/jupyterlab/jupyterlab)?**

There were technical (e.g. architecture of the current
Jupyter core not suitable for multi-language support), practical (e.g. Jupyter core was not active enough to accept
major new feature, JupyterLab was too instable (in alpha) to work with) reasons for the decision, but the biggest reason 
was **our vision to create a multi-language working environment backed by a workflow engine**, which led to
the development of SoS Notebook as a frontend to the SoS workflow engine.

### Why notebook environments are limited

Jupyter Notebooks, or notebook environments in generally, or any data analysis IDEs are limited in their abilities to
analyze big data. This is a difficult claim to make because there are many powerful IDEs, even big data IDEs, and there
are many ways to explore even analyze big data interactively, but the basic facts are that

1. It is intolerable to wait more than 5 minutes for a job to complete in interactive data analysis environment
2. However powerful your machine is, an IDE environment is limited if it can only execute scripts on one machine

So basically **an interactive environment is good at prototyping and developing of data analysis steps, not 
at large-scale data processing**. An IDE can be used to analyze small datasets or analyze data once if you are
willing to wait, but analyzing a large amount of data should be performed on much larger (e.g. cluster) systems with 
assistance from pipeline systems.

### Why “pipelineitis is a nasty disease”?

Everyone knows how powerful pipelines can be and [awsome pipelines](https://github.com/pditommaso/awesome-pipeline) lists more than 100
scientific pipeline systems. However, allow me to quote [Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740) 
for this point:

> #### Pipelineitis is a nasty disease

> A pipeline is a series of steps, or software tools, run in sequence according to a predefined plan. Pipelines are great for running exactly the same set of steps in a repetitive fashion, and for sharing protocols with others, but they force you into a rigid way of thinking and can decrease creativity.

> Warning: don't pipeline too early. Get a method working before you turn it into a pipeline. And even then, does it need to be a pipeline? Have you saved time? Is your pipeline really of use to others? If those steps are only ever going to be run by you, then a simple script will suffice and any attempts at pipelining will simply waste time. Similarly, if those steps will only ever be run once, just run them once, document the fact you did so and move on.

The problem with all these pipeline systems is that they require you to leave your familiar data analysis environment
to "program" pipeline using a [different environment, description language, or even a rigirous programming language](https://academic.oup.com/bib/article/18/3/530/2562749),
so whereas they are powerful and friendly to workflow users, they are not friendly to workflow writers.
The status quo is so confusing to users to a point that people are asking questions such as 
[Does anyone use CWL? Does it actually help you get work done?](https://www.reddit.com/r/bioinformatics/comments/7gxsk0/does_anyone_use_cwl_does_it_actually_help_you_get/). 
My answer to this question is **workflows are counter-productive if they require you to use another environment even language**.

### A thick wall between notebook and workflow systems

So we have two very nice sets of tools but there is a very thick wall between them. 
**It is a big hurdle to apply scripts developed interactively to large datasets** because
we need re-engineer the entire data analysis to a workflow using another environment or language.

![wall between notebook and workflow systems](https://vatlab.github.io/sos-docs/doc/media/interactive-vs-workflow.jpg)


### How SoS solves the transition problem

There are three figures on the [SoS Homepage](https://vatlab.github.io/sos-docs/), the [first](https://vatlab.github.io/sos-docs/doc/media/SoS_Notebook.gif)
showcases the multi-language features of SoS Notebook, the [third](https://vatlab.github.io/sos-docs/doc/media/example_dag.png)
shows a DAG of a SoS workflow, the [second](https://vatlab.github.io/sos-docs/doc/media/SoS_Workflow.gif) demonstrates
**smooth transition from interactive data analysis to batch data processing workflow**, with the following steps:

1. **Analyze data interactively** using multiple languages (with data exchange, line-by-line execution etc)
2. **Isolate scripts** (make them self-contained) and convert them to SoS functions with minimal syntax change 
(just move them from a subkernel to SoS kernel and add an action name).
3. **Annotate the scripts** with step headers and SoS directives to create workflows to analyze big data on local or remote systems (e.g. a cluster).


![SoS notebook with SoS](https://vatlab.github.io/sos-docs/doc/media/SoS_Workflow.gif)

The following video gives more details on how to use the SoS workflow engine within SoS Notebook:

{{< youtube H2Ca8QDIqQA >}} 


### Summary

[SoS](https://vatlab.github.io/sos-docs/) is a powerful workflow engine that supports both 
[forward-style (process-oriented)](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html#Forward-style_workflows)
and 
[makefile-style (outcome oriented)](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html#Makefile-style_workflow)
workflows and many advanced execution features (e.g. 
use [runtime signature](https://vatlab.github.io/sos-docs/doc/tutorials/Execution_of_Workflow.html#Runtime-signature-14)
to avoid re-execution of long commands, execute tasks on 
[remote workstation or cluster systems](https://vatlab.github.io/sos-docs/doc/tutorials/Remote_Execution.html)), but the **key feature 
of SOS is its simplicity, its easy transition from interactive to batch data processing, and from simple to advanced
workflows**. I will describe features of the SoS workflow engine in another post but the fact
that you can use SoS inside SoS Notebook already have many advantages:

1. SoS Notebook provides an interactive multi-language environment to develop and debug your workflow.
2. The Jupyter notebook format allows you to annotate workflows with markdown cells (tables, figures etc) and allows
  you to keep (sample) results along with the workflow.

The combination of SoS Notebook with the SoS workflow engine **provides a smooth transition and a single environment
for both interactive and batch data analysis**, which allows you to
**grow your data analysis with more and more flexibility and ability to handle larger and larger datasets**.
Depending on the scale and complexity of your project, you can stop at any stage of "pipelining" your analysis.

<small>
More details of SoS and SoS Notebook can be found at the [SoS website](https://vatlab.github.io/sos-docs/) where you can
find tons of documentations, tutorials, examples, and youtube videos. Please test SoS Notebook and send
your feedback and/or bug reports to our [github issue tracker](https://github.com/vatlab/sos-notebook/issues). 
If you find SoS Notebook useful, please support the project by starring the [SoS](https://github.com/vatlab/SoS) and
[SoS Notebook](https://github.com/vatlab/sos-notebook)
github projects, or spreading the word with [twitter](https://twitter.com/ScriptOfScripts). </small>
