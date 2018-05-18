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
Like any other software tools, many workflow systems are actively evolving with new features added from time to time. The goal of this blog post is to illustrate, by means of
comparison to some of the most popular workflow systems similar to SoS, **features and limitations of SoS as a conventional workflow system**.
It should be seen as a check-list of basic workflow features, in addition to the unique niche SoS places itself in the realm of workflow systems as explained
in the next section and in [other posts](https://vatlab.github.io/blog/).
We would very much appreciate it if you could [send us your comments](https://github.com/vatlab/blog/issues/10) if you believe more workflow engines or features should be compared, or if some comparisons are wrong or obsolete.


## How does SoS compare with SnakeMake, Nextflow, and Galaxy

In comparison to most workflow systems that are designed for "consumers" of workflows with emphases on efficient execution of well-crafted workflows with hidden details,
**SoS is designed for "developers" of workflows for ad hoc data processing with emphases on lowering the barrier of using workflows in daily computational research**.
The following tables compare basic features, workflow features, and built-in support for external tools and services between [SoS](https://vatlab.github.io/sos-docs/),
[NextFlow](https://www.nextflow.io/), [Snakemake](https://snakemake.readthedocs.io/en/stable/), [Bpipe](https://github.com/ssadedin/bpipe), and [Galaxy](https://usegalaxy.org/). We exclude [CWL](https://github.com/common-workflow-language/common-workflow-language) from the comparison table because CWL is a specification, not a workflow engine. It is designed to create portable workflows that can be executed by multiple workflow engines, and although its verbosity is almost in direct contrast with SoS’ conciseness, it is required for its multi-engine design.

### Basic information

<div class="comparison">
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
The scripting language for workflow specification</td>
<td>
	SoS extends Python 3.6 with
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html">a number of SoS-specific syntax extensions</a> and
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/Targets_and_Actions.html">pre-defined functions</a>.
</td>
<td>Nextflow is based on Groovy syntax with Nextflow-defined functions and objects. See <a href="https://www.nextflow.io/docs/latest/script.html">here</a> for details.</td>
<td>Snakemake is written in Python and has the flavor of <code>Make</code> system in syntax and execution.</td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ui-detail">User interface</a></td>
<td align="left">CLI + Notebook (Jupyter)</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI + GUI</td>
</tr>

<tr class="detail ui-detail">
<td>Primary methods for users to interact with the workflow engine</td>
<td>SoS provides two sets of user interface: <a href="https://vatlab.github.io/sos-docs/doc/documentation/User_Interface.html">command line</a> (<code>sos</code> command) and <a href="https://vatlab.github.io/sos-docs/doc/documentation/Notebook_Interface.html#Execution-of-Workflows--15">Jupyter magics</a> (<code>%run</code>, <code>%sorun</code> etc)</td>
<td>Nextflow workflows are executed with a <code>nextflow</code> command.</td>
<td>Snakemake workflows are executed with a <code>snakemake</code> command.</td>
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
<td>Format(s) to save workflows</td>
<td>SoS workflows can be saved in a plain text <code>.sos</code> format, or be embedded in a Jupyter Notebook with SoS kernel.</td>
<td>Plain text file with <code>.nf</code> extension.</td>
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
<td>Integrated Development Environment</td>
<td>SoS uses SoS Notebook, a companion Polyglot notebook environmnet based on Jupyter, as its IDE.</td>
<td>No dedicated IDE is available, but users can IDEs that support groovy (e.g. Eclipse, Netbeans) to edit (but not execute) nextflow workflows.</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
</div>

### Workflow features

<div class="comparison">
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
<td>SoS supports several ways to build its DAG, including explicit forward-style (sequential numbered steps), makefile-style (dependency), and dynamic creation of subworkflows (<code>sos_run</code> function)</td>
<td>Nextflow specifies process with input and output, and creates DAG from the processes.</td>
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
<td>SoS "data" are passed around as files</td>
<td>Processes in nextflow can communicate via asynchronous FIFO queues, called channels in the Nextflow lingo. </td>
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
<td>SoS provides a <code>sos_run(name)</code> function to dynamically execute a subworkflow.</td>
<td>Nextflow does not seem to support the dynamic creation of subworkflows</td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="rt-detail">Modify and resume</a></td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">?</td>
<td align="left">?</td>
<td align="left">?</td>
</tr>

<tr class="detail rt-detail">
<td>Able to resume interrupted or modified workflow and ignore parts of the workflow that have been successfully executed</td>
<td>SoS automatically keeps signatures of steps and tasks and can ignore steps and tasks that have already
been executed, even if they were executed by a different workflow.</td>
<td>Nextflow keeps track of all the processes executed in your pipeline. If you modify some parts of your script, only the processes that are actually changed will be re-executed. The execution of the processes that are not changed will be skipped and the cached result used instead.</td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="rt-detail">Remote execution</a></td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail rt-detail">
<td>Send tasks to remote hosts for execution.</td>
<td>SoS can execute entire workflows or individual tasks on multiple remote hosts, with file synchronization between
heterogeneous file systems.</td>
<td>Nextflow can be executed on a variety of environments but it has to be started within the environments</td>
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
"Report traces and performance" means that benchmarking commands and outputs are logged, along with resources usage such as CPU hours and memory consumption.</td>
<td>SoS can monitor tasks through the Jupyter Notebook interface with magics (e.g <code>%taskinfo</code>) to retrieve details about the tasks. It can also monitor status of tasks through a command line interface (e.g. <code>sos status</code>)</td>
<td>Nextflow can generate <a href="https://www.nextflow.io/docs/latest/tracing.html#">complete reports</a> with details on CPU/task usage etc.</td>
<td><a href="http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#benchmarking">Benchmarking</a> and <a href="http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#log-files">logging</a></td>
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
<td>Workflows that are constructed and executed by steps to execute.</td>
<td>SoS' "forward-style" workflow specifies steps of workflows through sequencial numbering although a DAG could be constructed with target dependencies.</td>
<td>Nextflow executes specified workflow with specified input and parameters.</td>
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
<td>Workflows that are constructed and executed by the "outcome" of the workflow.</td>
<td>SoS' auxiliary steps specifies outcomes of steps and will be called when the target is needed.</td>
<td>Nextflow executes specified workflow with specified input and parameters.</td>
<td></td>
<td></td>
<td></td>
</tr>
</tbody>
</table>
</div>


### Built-in support

<div class="comparison">
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
<td>Support for docker</td>
<td>A <code>docker_image</code> option can execute scripts inside specified docker images.</td>
<td>Nextflow support <a href="https://www.nextflow.io/docs/latest/docker.html">docker containers. You can
run all scripts in the specified docker image, or specify a docker image for each step.</a></td>
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
<td>Support for singularity</td>
<td>Pending</td>
<td>Nextflow <a href="https://www.nextflow.io/docs/latest/singularity.html">supports singularity containers</a>.
It works similar to docker but with options such as <code>singularty.enabled=true</code>.</td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="pbs-detail">PBS/Torque/LSF/SLURM</a></td>
<td align="left">Needs template</td>
<td align="left">Yes</td>
<td align="left">Direct or via template</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail pbs-detail">
<td>Ability to execute workflows on a PBS-style computer cluster
</td>
<td>SoS interact with clusters through pre-configured templates and commands. It has been tested to work on
Torque, LSF, SLURM, PBS, and Torque</td>
<td>Nextflow supports Open grid, Univa grid, LSF, SLURM, PBS Works, Torque</td>
<td>Snakemake can interact with clusters through templates, or directly if the cluster supports <a href="http://www.drmaa.org/http://www.drmaa.org/">DRMAA</a>.</td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a href="#" class="toggle-detail" data-detail="ht-detail">HTCondor</a></td>
<td align="left">Require template (?)</td>
<td align="left">Yes</td>
<td align="left">Require template</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail ht-detail">
<td>Ability to use <a href="https://research.cs.wisc.edu/htcondor/">HTCondor</a> to execute workflows on large collections of distributively owned computing resources.</td>
<td>Do not know because we have not had a chance to configure SoS to run on a HT Condor system.</td>
<td>Nextflow <a href="https://www.nextflow.io/docs/latest/executor.html#htcondor">supports HTCondor</a></td>
<td></td>
<td></td>
<td></td>
</tr>

<tr>
<td align="left"><a  href="#" class="toggle-detail" data-detail="tq-detail">Distributed Task Queue</a></td>
<td align="left">Yes</td>
<td align="left">RQ</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail tq-detail">
<td>Ability to send tasks to distributed task queues such as <a href="http://python-rq.org/">RQ</a> and <a href="http://www.celeryproject.org/">Celery</a>.
</td>
<td>SoS supports RQ, Celery support is likely broken due to lack of maitainence.</td>
<td>Nextflow cannot submit tasks to task queues</td>
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
<td>No</td>
<td>Nextflow supports distributed systems such as <a href="https://www.nextflow.io/docs/latest/ignite.html">Apache Ignite</a> and <a href="https://www.nextflow.io/docs/latest/kubernetes.html">Kubernetes</a></td>
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
<td>Not currently </td>
<td>Snakemake can <a href="https://www.nextflow.io/docs/latest/amazons3.html">access S3 storage</a></td>
<td>Snakemake can access files on <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/remote_files.html'>cloud storage</a></td>
<td></td>
<td></td>
</tr>

</tbody>
</table>
</div>


## Is SoS for you?

SoS is not for everyone. As a workflow system:

* If you are looking for a robust workflow system for large projects, the anser is likely no, at least for now.
* If you are looking for a script-less GUI-based workflow system, the answer is no.
* If you are a Jupyter user, the answer is most likely yes because SoS is embedded into SoS Notebook, which is by itself a polyglot notebook. You can enjoy all features of SoS Notebook and step into SoS only when needed.
* If you use multiple languages for daily data analysis, the answer is most likely yes.
