---
type: "post"
draft: false
author: "Bo Peng"
title: "How does SoS compare with other workflow engines"
ghcommentid: 10
date: "2018-03-29"
tags: ["SoS"]
---

Over [200 workflow systems](https://github.com/common-workflow-language/common-workflow-language/wiki/Existing-Workflow-systems) have been developed to date.
Like any other software tools, many workflow systems are actively evolving with new features added from time to time. The goal of this document is to illustrate, by means of
comparison to some of the most popular workflow systems similar to SoS, features and limitations of SoS as a conventional workflow system.
It should be seen as a check-list of basic workflow features, in addition to the unique niche SoS places itself in the realm of workflow systems, as will be pointed out in the section below.
Still, we would very much appreciate your pointer in the comment section
at the end of this page if you believe more features should be compared, or point to us if some comparisons are wrong or obsolete.

## SoS is unique

SoS was designed with a clear aim: to bridge the gap between interactive analysis and workflow executions. As a workflow engine (`sos`):

* It features a Jupyter Notebook interface that consolidates various scripts and narratives in one document
* It allows executing interactive analysis and batch jobs under the same user interface
* It provides multiple workflow styles that smooths the process of converting sequentially executed scripts to robust workflows on dependency graphs

As a cross-language data analysis tool (`sos-notebook`):

* It allows data communications between codes written in different languages
* It enhances Jupyter Notebook GUI with cell-level language switcher and a scratch pad
* It implements new magics tailored for interactive analysis in bioinformatics

## How does SoS compare with SnakeMake, Nextflow, and Galaxy


<div class="table table-hover">
<table>
<thead>
<tr>
<th align="left">Workflow</th>
<th align="left">SoS</th>
<th align="left">NextFlow</th>
<th align="left">Snakemake</th>
<th align="left">Bpipe</th>
<th align="left">Galaxy</th>
</tr>
</thead>

<tbody>
<tr>
<td align="left"><a href="#language-interface">Language interface</a></td>
<td align="left">Python flavored</td>
<td align="left">Groovy flavored</td>
<td align="left">GNU Make style, Python flavored</td>
<td align="left">Groovy flavored</td>
<td align="left">JSON flavored</td>
</tr>

<tr>
<td align="left"><a href="#user-interface">User interface</a></td>
<td align="left">CLI + Notebook (Jupyter)</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI + GUI</td>
</tr>

<tr>
<td align="left"><a href="#file-format">File format</a></td>
<td align="left">.sos (plain text) and Jupyter notebook</td>
<td align="left">.nf (plain text)</td>
<td align="left">Snakefile (plain text)</td>
<td align="left">.pipe (groovy, plain text)</td>
<td align="left">XML</td>
</tr>

<tr>
<td align="left"><a href="#ide">IDE</a></td>
<td align="left">SoS Notebook (Jupyter)</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes (only for building DAG)</td>
</tr>

<tr>
<td align="left"><strong>Workflow features</strong></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>

<tr>
<td align="left"><a href="#dag-building">DAG Building</a></td>
<td align="left">Explicit, implicit and dynamic</td>
<td align="left">Explicit</td>
<td align="left">Implicit and dynamic</td>
<td align="left">Explicit</td>
<td align="left">Explicit</td>
</tr>

<tr>
<td align="left"><a href="#streaming-processing">Streaming processing</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#subworkflow">Subworkflow</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#remote-task-submission">Remote task submission</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#task-monitoring">Task monitoring</a></td>
<td align="left">Command line and GUI (Notebook)</td>
<td align="left">Report traces and performances</td>
<td align="left">Report traces and performance</td>
<td align="left">Event notification</td>
<td align="left">GUI to explore, share and reuse histories</td>
</tr>

<tr>
<td align="left"><a href="#heterogeneous-executor">Heterogeneous executor</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#process-oriented-workflow">Process-oriented workflow</a></td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#output-oriented-workflow">Output-oriented workflow</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><strong>Built-in support</strong></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
<td align="left"></td>
</tr>

<tr>
<td align="left"><a href="#docker">Docker</a></td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#singularity">Singularity</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#multi-scale-containers">Multi-scale containers</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#pbs-torque-lsf-slurm">PBS/Torque/LSF/SLURM</a></td>
<td align="left">Partial</td>
<td align="left">Yes</td>
<td align="left">Partial</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#htcondor">HTCondor</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Require template</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr>
<td align="left"><a href="#task-queue">Task Queue</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#distributed-systems">Distributed systems</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr>
<td align="left"><a href="#cloud-storage">Cloud Storage</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>
</tbody>
</table>
</div>

Comparison of basic features, workflow features, and built-in support for external tools and services between SoS, NextFlow, Snakemake, Bpipe, and Galaxy.

#### Language interface

Language interface refers to the scripting language for workflow specification. Because it is easier to pick up a workflow system with familiar syntax, people who are at home with Python would prefer `SoS` or other Python-based workflow systems such as [Luigi](https://github.com/spotify/luigi), and people who are familiar with Groovy might prefer `Nextflow` or `Bpipe`.

* SoS extends Python 3.6 with [a number of SoS-specific syntax extensions](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html) and [pre-defined functions](https://vatlab.github.io/sos-docs/doc/documentation/Targets_and_Actions.html). The `sos` command can run most Python scripts but you can not run sos workflow with Python.
* Nextflow is based on Groovy syntax with Nextflow-defined functions and objects. The `nextflow` command is used to execute Nextflow workflows.
* Snakemake is written in Python and has the flavor of `Make` system in syntax and execution. By default `snakemake` command is used to execute workflow in a `Snakefile` under the same directory though other script filenames can be specified.
* Bpipe?
* Galaxy


#### User interface

#### FIle format

#### IDE

#### DAG Building
Methods and logic to construct dependency graphs connecting tasks in a workflow.

#### Streaming processing
Ability to process tasks inputs/outputs as a stream of data.

#### Subworkflow

Support for executing subworkflows, potentially loaded from another pipeline file.

#### Remote task submission

mapping paths and synchronize files isolated file systems so executing tasks on remote hosts with distinct file systems.

#### Task monitoring
Ability to send tasks to multiple isolated computing environment and manage them from local host.
"Report traces and performance" means that benchmarking commands and outputs are logged, along with resources usage such as CPU hours
and memory consumption.

#### Heterogeneous executor
DAG creation logics, see section 2.2. g) Integrated support for Docker/Singularity containers technology.

#### Process-oriented workflow

#### Output-oriented workflow

#### Docker

#### Singularity

#### Multi-scale containers

#### PBS/Torque/LSF/SLURM
Ability to manage the execution of multiple container instances in a distributed/HPC cluster or cloud. "Partial" means that users need to provide templates for some of these systems.

#### HTCondor
Ability to spawn the executions of pipeline tasks through a cluster batch scheduler without the need of custom scripts or commands.

#### Task queue
Ability to send tasks to RQ task queues.

#### Distributed systems
Ability to spawn the executions of pipeline tasks through a distributed cluster such as Apache Spark, Apache Ignite, Apache Mesos, and Kubernetes.


### What about CWL?

We exclude [CWL](https://github.com/common-workflow-language/common-workflow-language) from the comparison table because CWL is a specification, not a workflow engine. It is designed to create portable workflows that can be executed by multiple workflow engines, and although its verbosity is almost in direct contrast with SoS’ conciseness, it is required for its multi-engine design.

### Why XXX is not in the list?

There are more than 200 workflow systems and we cannot possible compare them all here. Here are however a few sources that provides meaningful comparison between some of the more popular systems.

* [The return of workflows](https://stackstorm.com/2015/04/10/the-return-of-workflows/）compares: Pinball, Spiff, Luigi, Ansible, Dray, Score, ActionChain, Mistral
*

Please feel free to send us links to similar resources that comparison workflow systems and we will be happy to add them here. We can even add your favoriate workflow engine to the big table if you can provide us enough details.

#### Cloud storage
Ability to make use of cloud storage (AWS).

## Is SoS for you?

SoS is not for everyone. As a workflow system:

* **If you are looking for a robust workflow system for huge projects**, the anser is likely no, at least for now.
* **If you are looking for a script-less GUI-based workflow system**, the answer is no.

As an interactive analysis platform:

* **If you are a RStudio user**, the anser is likely no.
* **If you are a Jupyter user**, the answer is most likely yes because SoS is embedded into SoS Notebook, which is by itself a polyglot notebook. You can enjoy all features of SoS Notebook and step into SoS only when needed.
* **If you use Python for daily data analysis**, the answer is most likely yes.
