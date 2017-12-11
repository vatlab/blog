---
type: "post"
draft: false
author: "Bo Peng"
title: "What's the big deal about backing SoS Notebook with a workflow engine?"
ghcommentid: 7
date: "2017-12-10"
tags: ["Notebook", "Workflow Engine"]
---

After I [announced the release of SoS Notebook](https://vatlab.github.io/blog/post/sos-notebook/) as a third-party
multi-language kernel for [Jupyter](http://jupyter.org/), I was asked repeatedly (e.g. On [HackerNews](https://news.ycombinator.com/item?id=15852821), 
[AzureNotebooks](https://github.com/Microsoft/AzureNotebooks/issues/254#issuecomment-349722523), and in reviews to our manuscript) the following question:

> **Why did not you use an existing multi-language notebooks 
(e.g. [Apache Zeppelin](https://zeppelin.apache.org/) and [BeakerX](https://github.com/twosigma/beakerx)) or
contribute a multi-language feature to the core of [Jupyter](http://jupyter.org/) or 
[JupyterLab](https://github.com/jupyterlab/jupyterlab)?**

There were several technical (e.g. architecture of the current Jupyter core not suitable for multi-language support) and
practical (e.g. JupyterLab was too unstable enough to work with) reasons for the decision, but the biggest reason 
was **our vision to create a multi-language working environment backed by a workflow engine**, which was too radical
and too ambitious for the Jupyter core so we had to develop SoS Notebook as a Jupyter kernel for our SoS workflow
engine.

### Why notebook environments are limited

Jupyter Notebooks, or notebook environments in generally, or any data analysis IDEs are limited in their abilities to
analyze big data. This is a difficult claim to make because there are many powerful IDEs, even big data IDEs, and there
are many ways to explore even analyze big data interactively, but the basic facts are that

1. It is intolerable to wait more than 5 minutes for a job to complete in an interactive data analysis environment
2. However powerful your machine is, a notebook environment is limited if it can only execute scripts on one machine

So basically **an interactive environment is good at prototyping and developing of data analysis steps, not 
at large-scale data processing**. An IDE can be used to analyze small datasets or analyze large datasets occasionally,
but analyzing a large amount of data should be performed on much larger (e.g. cluster) systems with 
assistance from pipeline systems.

### Why “pipelineitis is a nasty disease”?

Everyone knows how powerful pipelines can be and [awsome pipelines](https://github.com/pditommaso/awesome-pipeline) lists more than 100
scientific pipeline systems. However, allow me to quote [Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740) 
for this point:

> #### Pipelineitis is a nasty disease

> A pipeline is a series of steps, or software tools, run in sequence according to a predefined plan. Pipelines are great for running exactly the same set of steps in a repetitive fashion, and for sharing protocols with others, but they force you into a rigid way of thinking and can decrease creativity.

> Warning: don't pipeline too early. Get a method working before you turn it into a pipeline. And even then, does it need to be a pipeline? Have you saved time? Is your pipeline really of use to others? If those steps are only ever going to be run by you, then a simple script will suffice and any attempts at pipelining will simply waste time. Similarly, if those steps will only ever be run once, just run them once, document the fact you did so and move on.

The problem with all these pipeline systems is that they require you to leave your familiar data analysis environment
to "program" pipelines using a [different environment, description language, or even a rigirous programming language](https://academic.oup.com/bib/article/18/3/530/2562749),
so whereas they are powerful and friendly to workflow users, they are rarely friendly to workflow writers.
The status quo is so confusing to users to a point that people are asking questions such as 
[Does anyone use CWL? Does it actually help you get work done?](https://www.reddit.com/r/bioinformatics/comments/7gxsk0/does_anyone_use_cwl_does_it_actually_help_you_get/) 
My answer to this question is that **workflows are counter-productive if they require you to use another environment even language**.

### A big hurdle from interactive to batch data analysis

So we have two sets of very nice tools but **there is a big hurdle to apply scripts developed interactively to larger datasets** because
of the need to re-engineer the entire data analysis to a workflow using another environment or language.

![wall between notebook and workflow systems](https://vatlab.github.io/sos-docs/doc/media/interactive-vs-workflow.jpg)


### How SoS solves the transition problem

There are three figures on the [SoS Homepage](https://vatlab.github.io/sos-docs/), the [first](https://vatlab.github.io/sos-docs/doc/media/SoS_Notebook.gif)
showcases the multi-language features of SoS Notebook, the [third](https://vatlab.github.io/sos-docs/doc/media/example_dag.png)
shows a DAG of a SoS workflow, the [second](https://vatlab.github.io/sos-docs/doc/media/SoS_Workflow.gif) demonstrates
**smooth transition from interactive data analysis to batch data processing workflow**, with the following steps:

1. **Analyze data interactively** using multiple languages (with data exchange, line-by-line execution etc). 
2. **Convert scripts in subkernels to workflow steps** in SoS Notebook with minimal syntax change (simply change the cell kernel to SoS and add an action name).
3. **Annotate steps with execution control directives** by adding `input`, `output`, `depends`, `task` directives and section headers)
 to create workflows to analyze big data on local or remote systems (e.g. a cluster).


The entire process can be examplified by the following example where:

1. A shell script and a R script are executed with variables defined in SoS (figure 1-3),
2. The scipts are converted to SoS actions to be executed individually in SoS (figure 4-5),
3. Section headers are added to convert the steps to a workflow (figure 6-7),
4. The resulting workflow can be executed and displayed in SoS Notebook (figure 8-9)

![SoS notebook with SoS](https://vatlab.github.io/sos-docs/doc/media/SoS_Workflow.gif)

The SoS workflow engine has many features and deserves a separate post but the fact
that you can use SoS inside SoS Notebook has already provided several advantages:

1. SoS Notebook provides an interactive multi-language environment to develop and debug your workflow.
2. The Jupyter notebook format allows you to annotate workflows with markdown cells (tables, figures etc) and allows
  you to keep (sample) results along with the workflow.

The following video gives more details on how to use the SoS workflow engine within SoS Notebook:

{{< youtube H2Ca8QDIqQA >}} 


### Summary


The combination of SoS Notebook with the SoS workflow engine **provides a smooth transition and a single environment
for both interactive and batch data analysis**. The transition is designed to be as smooth as pososible by

1. Allowing interactive development of scripts in any language in SoS Notebook,
2. Using SoS syntaxes (e.g. `%expand` magic to interpolate scripts) in SoS Notebook,
3. Allowing interactive execution of SoS workflows in SoS Notebook, and
4. Ability to execute SoS Notebooks from command line.

This powerful marriage between a multi-language notebook and a workflow engine allows you to 
**grow your data analysis with more and more flexibility and ability to handle larger and larger datasets**.
Depending on the scale and complexity of your project, you can stop at any stage of "pipelining" your analysis
and there is no need to start from scratch if you ever need to apply your analysis to larger datasets. SoS Notebook
is currently implemented as a Jupyter kernel but we will certainly port it to JupyterLab after JupyterLab matures
([JupyterLab/2815](https://github.com/jupyterlab/jupyterlab/issues/2815)).


<small>
More details of SoS and SoS Notebook can be found at the [SoS website](https://vatlab.github.io/sos-docs/) where you can
find tons of documentations, tutorials, examples, and youtube videos. Please test SoS Notebook and send
your feedback and/or bug reports to our [github issue tracker](https://github.com/vatlab/sos-notebook/issues). 
If you find SoS Notebook useful, please support the project by starring the [SoS](https://github.com/vatlab/SoS) and
[SoS Notebook](https://github.com/vatlab/sos-notebook)
github projects, or spreading the word with [twitter](https://twitter.com/ScriptOfScripts). </small>
