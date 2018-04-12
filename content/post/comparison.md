---
type: "post"
draft: false
author: "Bo Peng"
title: "How does SoS compare with other workflow engines (Under development)"
ghcommentid: 10
date: "2018-03-29"
tags: ["SoS"]
---

Over [200 workflow systems](https://github.com/common-workflow-language/common-workflow-language/wiki/Existing-Workflow-systems) have been developed to date.
Like any other software tools, many workflow systems are actively evolving with new features added from time to time. The goal of this document is to illustrate, by means of
comparison to some of the most popular workflow systems similar to SoS, **features and limitations of SoS as a conventional workflow system**.
It should be seen as a check-list of basic workflow features, in addition to the unique niche SoS places itself in the realm of workflow systems, as will be pointed out in the section below.
We would very much appreciate it if you could [send us your comments](https://github.com/vatlab/blog/issues/10) if you believe more workflow engines or features should be compared, or if some comparisons are wrong or obsolete.


## How does SoS compare with SnakeMake, Nextflow, and Galaxy

In comparison to most workflow systems that are designed for "consumers" of workflows with emphases on efficient execution of well-crafted workflows with hidden details,
**SoS is designed for "developers" of workflows for ad hoc data processing with emphases on lowering the barrier of using workflows in daily computational research**.
The following tables compare basic features, workflow features, and built-in support for external tools and services between SoS, NextFlow, Snakemake, Bpipe, and Galaxy,
**with references to corresponding documentation** (click the header of each column for details).

### Basic information

<div class="table ">
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
<td align="left">
<a href="#" class="toggle-detail" data-detail="lan-detail">Language interface</a>
<td align="left">Python based</td>
<td align="left">Groovy flavored</td>
<td align="left">GNU Make style, Python flavored</td>
<td align="left">Groovy flavored</td>
<td align="left">JSON flavored</td>
</tr>

<tr class="detail lan-detail">
<td>
Language interface refers to the scripting language for workflow specification. Because it is easier to pick up a workflow system with familiar syntax, people who are at home with Python would prefer `SoS` or other Python-based workflow systems such as
<a href="https://github.com/spotify/luigi">Luigi</a>, and people who are familiar with Groovy might prefer
<code>Nextflow</code> or <code>Bpipe</code>.
</td>
<td>
	SoS extends Python 3.6 with
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html">a number of SoS-specific syntax extensions</a> and
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/Targets_and_Actions.html">pre-defined functions</a>.
  The <code>sos</code> command can run most Python scripts but you can not run sos workflow with Python.

</td>
<td>Nextflow is based on Groovy syntax with Nextflow-defined functions and objects. The `nextflow` command is used to execute Nextflow workflows.</td>
<td>Snakemake is written in Python and has the flavor of `Make` system in syntax and execution. By default `snakemake` command is used to execute workflow in a `Snakefile` under the same directory though other script filenames can be specified.
</td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ui-detail">User interface</a></td>

<tr class="detail ui-detail">
<td>Command line interface (CLI), graphical user interface (GUI) or others.
</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<td align="left">CLI + Notebook (Jupyter)</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI + GUI</td>
</tr>

<tr class="detail ui-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ff-detail">File format</a></td>
<td align="left">.sos (plain text) and Jupyter notebook</td>
<td align="left">.nf (plain text)</td>
<td align="left">Snakefile (plain text)</td>
<td align="left">.pipe (groovy, plain text)</td>
<td align="left">XML</td>
</tr>

<tr class="detail ff-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ide-detail">IDE</a></td>
<td align="left">SoS Notebook (Jupyter)</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes (only for building DAG)</td>
</tr>

<tr class="detail ide-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
</div>

### Workflow features

<div class="table ">
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
<td align="left"><a href="#" class="toggle-detail" data-detail="dag-detail">DAG Building</a></td>
<td align="left">Explicit, implicit and dynamic</td>
<td align="left">Explicit</td>
<td align="left">Implicit and dynamic</td>
<td align="left">Explicit</td>
<td align="left">Explicit</td>
</tr>

<tr class="detail dag-detail">
<td>Methods and logic to construct dependency graphs connecting tasks in a workflow.
</td>
<td></td>
<td></td>
<td>Relies on <a href="http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html">filename (pattern) matching</a> to determine execution sequence.</td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="sp-detail">Streaming processing</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail sp-detail">
<td>Ability to process tasks inputs/outputs as a stream of data.
</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="sw-detail">Subworkflow</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail sw-detail">
<td>Support for executing subworkflows, potentially loaded from another pipeline file.
</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="rt-detail">Remote task submission</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail rt-detail">
<td>mapping paths and synchronize files isolated file systems so executing tasks on remote hosts with distinct file systems.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="tm-detail">Task monitoring</a></td>
<td align="left">Command line and GUI (Notebook)</td>
<td align="left">Report traces and performances</td>
<td align="left">Report traces and performance</td>
<td align="left">Event notification</td>
<td align="left">GUI to explore, share and reuse histories</td>
</tr>

<tr class="detail tm-detail">
<td>Ability to send tasks to multiple isolated computing environment and manage them from local host.
"Report traces and performance" means that benchmarking commands and outputs are logged, along with resources usage such as CPU hours
and memory consumption.</td>
<td></td>
<td></td>
<td><a href="http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#benchmarking">Benchmarking</a> and <a href="http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#log-files">logging</a></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="het-detail">Heterogeneous executor</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail het-detail">
<td>DAG creation logics, see section 2.2. g) Integrated support for Docker/Singularity containers technology.
</td>
<td></td>
<td></td>
<td><a href='http://snakemake.readthedocs.io/en/stable/executable.html#cloud-support'>Experimental cloud support</a> which can be used to schedule and run Docker containers with Kybernetes.</td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="pow-detail">Process-oriented workflow</a></td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail pow-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="oow-detail">Output-oriented workflow</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail oow-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
</div>


### Built-in support

<div class="table ">
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
<td align="left"><a href="#" class="toggle-detail" data-detail="do-detail">Docker</a></td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail do-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="si-detail">Singularity</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail si-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>


<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="msc-detail">Multi-scale containers</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail msc-detail">
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="pbs-detail">PBS/Torque/LSF/SLURM</a></td>
<td align="left">Partial</td>
<td align="left">Yes</td>
<td align="left">Partial</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail pbs-detail">
<td>Ability to manage the execution of multiple container instances in a distributed/HPC cluster or cloud. "Partial" means that users need to provide templates for some of these systems.
</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ht-detail">HTCondor</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Require template</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail ht-detail">
<td>Ability to spawn the executions of pipeline tasks through a cluster batch scheduler without the need of custom scripts or commands.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a  href="#" class="toggle-detail" data-detail="tq-detail">Task Queue</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail tq-detail">
<td>Ability to send tasks to RQ task queues.
</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ds-detail">Distributed systems</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail ds-detail">
<td>Ability to spawn the executions of pipeline tasks through a distributed cluster such as Apache Spark, Apache Ignite, Apache Mesos, and Kubernetes.</td>
<td></td>
<td></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="cs-detail">Cloud Storage</a></td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail cs-detail">
<td>Ability to make use of cloud storage (such as AWS).
</td>
<td></td>
<td></td>
<td><a href='http://snakemake.readthedocs.io/en/stable/snakefiles/remote_files.html'>Link to relevant Snakemake documentation</a></td>
<td></td>
<td></td>
</tr>

</tbody>
</table>
</div>


### What about CWL?

We exclude [CWL](https://github.com/common-workflow-language/common-workflow-language) from the comparison table because CWL is a specification, not a workflow engine. It is designed to create portable workflows that can be executed by multiple workflow engines, and although its verbosity is almost in direct contrast with SoS’ conciseness, it is required for its multi-engine design.

### Why XXX is not in the list?

There are more than 200 workflow systems and we cannot possible compare them all here. Here are however a few sources that provides meaningful comparison between some of the more popular systems.

* [The return of workflows](https://stackstorm.com/2015/04/10/the-return-of-workflows/）compares: Pinball, Spiff, Luigi, Ansible, Dray, Score, ActionChain, Mistral
*

Please feel free to send us links to similar resources that comparison workflow systems and we will be happy to add them here. We can even add your favoriate workflow engine to the big table if you can provide us enough details.


## Is SoS for you?

SoS is not for everyone. As a workflow system:

* **If you are looking for a robust workflow system for huge projects**, the anser is likely no, at least for now.
* **If you are looking for a script-less GUI-based workflow system**, the answer is no.

As an interactive analysis platform:

* **If you are a RStudio user**, the anser is likely no.
* **If you are a Jupyter user**, the answer is most likely yes because SoS is embedded into SoS Notebook, which is by itself a polyglot notebook. You can enjoy all features of SoS Notebook and step into SoS only when needed.
* **If you use Python for daily data analysis**, the answer is most likely yes.
