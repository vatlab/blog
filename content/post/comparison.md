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
comparison to some of the most popular workflow systems similar to SoS, features and limitations of SoS. It should be seen as a check-list of basic workflow features, in addition to
the unique niche SoS places itself in the realm of workflow systems, as will be pointed out in the section below.

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

| Workflow                            | SoS                                    | NextFlow                       | Snakemake                       | Bpipe                      | Galaxy                                    |
|:------------------------------------|:---------------------------------------|:-------------------------------|:--------------------------------|:---------------------------|:------------------------------------------|
| [Language interface](#language-interface) | Python flavored                        | Groovy flavored                | GNU Make style, Python flavored | Groovy flavored            | JSON flavored                             |
| [User interface](#user-interface)   | CLI + Notebook (Jupyter)               | CLI                            | CLI                             | CLI                        | CLI + GUI                                 |
| [File format](#file-format)         | .sos (plain text) and Jupyter notebook | .nf (plain text)               | Snakefile (plain text)          | .pipe (groovy, plain text) | XML                                       |
| [IDE](#ide)                         | SoS Notebook (Jupyter)                 | No                             | No                              | No                         | Yes (only for building DAG)               |
| **Workflow features**               |      |         |         |              |             |
| [DAG Building](#dag-building)              | Explicit, implicit and dynamic         | Explicit                       | Implicit and dynamic            | Explicit                   | Explicit                                  |
| [Streaming processing](#streaming-processing)           | No                                     | Yes                            | No                              | No                         | No                                        |
| [Subworkflow](#subworkflow)         | Yes                                    | No                             | Yes                             | Yes                        | Yes                                       |
| [Remote task submission](#remote-task-submission) | Yes                                    | No                             | No                              | No                         | No                                        |
| [Task monitoring](#task-monitoring) | Command line and GUI (Notebook)        | Report traces and performances | Report traces                   | Event notification         | GUI to explore, share and reuse histories |
| [Heterogeneous executor](#heterogeneous-executor)   | Yes                                    | No                             | No                              | No                         | No                                        |
| [Process-oriented workflow](#process-oriented-workflow)  | Yes                                    | Yes                            | No                              | Yes                        | Yes                                       |
| [Output-oriented workflow](#output-oriented-workflow) | Yes                                    | No                             | Yes                             | No                         | No                                        |
| **Built-in support**                |      |         |         |              |             |  
| [Docker](#docker)                   | Yes                                    | Yes                            | Yes                             | No                         | Yes                                       |
| [Singularity](#singularity)         | No                                     | Yes                            | Yes                             | No                         | No                                        |
| [Multi-scale containers](#multi-scale-containers)  | No                                     | Yes                            | No                              | No                         | Yes                                       |
| [PBS/Torque/LSF/SLURM](pbs-torque-lsf-slurm)       | Require template                       | Yes                            | Require template                | Yes                        | Yes                                       |
| [HTCondor](#htcondor)               | No                                     | Yes                            | Require template                | No                         | Yes                                       |
| [Task Queue](#task-queue)           | Yes                                    | No                             | No                              | No                         | No                                        |
| [Distributed systems](#distributed-systems)            | No                                     | Yes                            | No                              | No                         | No                                        |
| [Cloud Storage](#cloud-storage)      | No                                     | Yes                            | No                              | No                         | Yes                                       |

Comparison of basic features, workflow features, and built-in support for external tools and services between SoS, NextFlow, Snakemake, Bpipe, and Galaxy. 

#### Language interface

Language interface refers to the scripting language for workflow specification. Because it is easier to pick up a workflow system with familiar syntax, people who are at home with Python would prefer `SoS` or other Python-based workflow systems such as [Luigi](https://github.com/spotify/luigi), and people who are familiar with Groovy might prefer `Nextflow` or `Bpipe`.

* SoS extends Python 3.6 with [a number of SoS-specific syntax extensions](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html) and [pre-defined functions](https://vatlab.github.io/sos-docs/doc/documentation/Targets_and_Actions.html). The `sos` command can run most Python scripts but you can not run sos workflow with Python.
* Nextflow is based on Groovy syntax with Nextflow-defined functions and objects. The `nextflow` command is used to execute Nextflow workflows.
* Snakemake?
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

#### Heterogeneous executor
DAG creation logics, see section 2.2. g) Integrated support for Docker/Singularity containers technology.

#### Process-oriented workflow

#### Output-oriented workflow

#### Docker

#### Singularity

#### Multi-scale containers

#### PBS/Torque/LSF/SLURM
Ability to manage the execution of multiple container instances in a distributed/HPC cluster or cloud. 

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
