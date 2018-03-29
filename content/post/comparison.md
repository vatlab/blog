---
type: "post"
draft: false
author: "Bo Peng"
title: "How does SoS compare with other workflow engines"
ghcommentid: 10
date: "2018-03-29"
tags: ["SoS"]
---

There are about 200 workflow systems ... All workflow systems are evolving with new features added from time to time. Please let us know if our comparison is incorrect.


## SoS is unique


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
| [Task monitoring](#task-monitoring) | Comand line and GUI (Notebook)         | Report traces and performances | Report traces                   | Event notification         | GUI to explore, share and reuse histories |
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

#### Cloud storage
Ability to make use of cloud storage (AWS).

## Is SoS for you?



