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
Like any other software tools, many workflow systems are actively evolving with new features added from time to time. The goal of this blog post is to illustrate, by means of
comparison to some of the most popular workflow systems similar to SoS, **features and limitations of SoS as a conventional workflow system**.
It should be seen as a check-list of basic workflow features, in addition to the unique niche SoS places itself in the realm of workflow systems as explained
in the next section and in [other posts](https://vatlab.github.io/blog/).

## How does SoS compare with Nextflow, SnakeMake, Bpipe, CWL, and Galaxy

In comparison to most workflow systems that are designed for "consumers" of workflows with emphases on efficient execution of well-crafted workflows with hidden details,
**SoS is designed for "developers" of workflows for ad hoc data processing with emphases on lowering the barrier of using workflows in daily computational research**.
The following tables compare basic features, workflow features, and built-in support for external tools and services between [SoS](https://vatlab.github.io/sos-docs/),
[NextFlow](https://www.nextflow.io/), [Snakemake](https://snakemake.readthedocs.io/en/stable/), [Bpipe](https://github.com/ssadedin/bpipe), [CWL](https://github.com/common-workflow-language/common-workflow-language), and [Galaxy](https://usegalaxy.org/).


<div class="alert alert-success" role="alert">
<span class="alert-heading"><h5>Hint:</h5></span>
<ol>
<li><strong>Click on the rows of the table to expand/collapse detailed explanations.</strong></li>
<li>"(?)" indicates uncertain comparisons due to lack of information.</li>
<li>Information presented here can be inaccurate or obsolete due to rapid evolution of workflow engines.</li>
</ol>
We would very much appreciate it if you could <a href="https://github.com/vatlab/blog/issues/10" class="alert-link" target="_blank">send us your comments</a> or <a href="https://github.com/vatlab/blog" class="alert-link" target="_blank">pull requests</a> if you notice any problems with the table, or if you believe more workflow engines or features should be compared.
</div>

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
<th align="left">CWL</th>
<th align="left">Galaxy</th>
</tr>
</thead>

<tbody>
<tr class="result">
<th align="left">Language</th>
<td align="left">Python based</td>
<td align="left">Groovy flavored</td>
<td align="left">GNU Make style, Python flavored</td>
<td align="left">Groovy flavored</td>
<td align="left">na</td>
<td align="left">na</td>
</tr>

<tr class="detail">
<td>
The scripting language for workflow specification</td>
<td>
	SoS extends Python 3.6 with
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/SoS_Syntax.html">a number of SoS-specific syntax extensions</a> and
  <a href="https://vatlab.github.io/sos-docs/doc/documentation/Targets_and_Actions.html">pre-defined functions</a>.
</td>
<td>Nextflow is based on Groovy syntax with Nextflow-defined functions and objects. See <a href="https://www.nextflow.io/docs/latest/script.html">here</a> for details.</td>
<td>Snakemake is written in Python and has the flavor of <code>Make</code> system in syntax and execution.</td>
<td>Bpipe is implemented in Groovy. Its syntax departs as little as possible from the simplicity of the shell script.</td>
<td>CWL workflows are specified in JSON or YAML format.</td>
<td>Galaxy's workflows are stored in JSON files together with GUI-related meta information</td>
</tr>

<tr class="result">
<th align="left">User interface</td>
<td align="left">CLI + Notebook (Jupyter)</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI</td>
<td align="left">CLI (cwltool)</td>
<td align="left">CLI + GUI</td>
</tr>

<tr class="detail">
<td>Primary methods for users to interact with the workflow engine</td>
<td>SoS provides two sets of user interface: <a href="https://vatlab.github.io/sos-docs/doc/documentation/User_Interface.html">command line</a> (<code>sos</code> command) and <a href="https://vatlab.github.io/sos-docs/doc/documentation/Notebook_Interface.html#Execution-of-Workflows--15">Jupyter magics</a> (<code>%run</code>, <code>%sorun</code> etc)</td>
<td>Nextflow workflows are executed with a <code>nextflow</code> command.</td>
<td>Snakemake workflows are executed with a <code>snakemake</code> command.</td>
<td>Bpipe workflows are executed with a <code>bpipe</code> command.</td>
<td>cwltool has a CLI, but other workflow engines could provide a GUI</td>
<td>Galaxy workflows are mostly executed using a web interface, but it can also be executed using a CLI.</td>
</tr>

<tr class="result">
<th align="left">File format</th>
<td align="left">.sos (plain text) and Jupyter notebook</td>
<td align="left">.nf (plain text)</td>
<td align="left">Snakefile (plain text)</td>
<td align="left">.pipe (groovy, plain text)</td>
<td align="left">.cwl and .yml (JSON/YAML)</td>
<td align="left">XML</td>
</tr>

<tr class="detail">
<td>Format(s) to save workflows</td>
<td>SoS workflows can be saved in a plain text <code>.sos</code> format, or be embedded in a Jupyter Notebook with SoS kernel.</td>
<td>Plain text file with <code>.nf</code> extension.</td>
<td>Plain text file named <code>Snakefile</code>, or with <code>*.rules</code> extension for rules from another file.</td>
<td>Plain text file with <code>.pipe</code>, or <code>*.groovy</code> extensions. </td>
<td>CWL documents are written in JSON or YAML, or a mix of the two</td>
<td>Galaxy files are saved by the framework and are not supposted to be edited directly.</td>
</tr>

<tr class="result">
<th align="left">IDE</th>
<td align="left">SoS Notebook (Jupyter)</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No (cwltools)</td>
<td align="left">Yes (only for building DAG)</td>
</tr>

<tr class="detail">
<td>Integrated Development Environment</td>
<td>SoS uses SoS Notebook, a companion Polyglot notebook environmnet based on Jupyter, as its IDE.</td>
<td>No dedicated IDE is available, but users can IDEs that support groovy (e.g. Eclipse, Netbeans) to edit (but not execute) nextflow workflows.</td>
<td>No dedicated IDE is available but syntax highlighter plugin are provided for some text editors.</td>
<td>No dedicated IDE is available but editors supporting Groovy syntax can be use to facilicate pipeline development.</td>
<td>No IDE is provided for cwltools, but other task engines might provide one</td>
<td>A web interface is provided to create steps and connect them</td>
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
<th align="left">CWL</th>
<th align="left">Galaxy</th>
</tr>
</thead>
<tbody>


<tr class="result">
<th align="left">DAG Building</th>
<td align="left">Explicit DAG of steps by connecting steps, implicit by target matching</td>
<td align="left">Implicit DAG of steps from input/output</td>
<td align="left">Implicit DAG by files from pattern matching input/output</td>
<td align="left">Implicit DAG of steps from input/output</td>
<td align="left">Implicit DAG of steps from input/output</td>
<td align="left">Explicit DAG of steps by connecting steps</td>
</tr>

<tr class="detail">
<td>Methods and logic to construct dependency graphs connecting tasks in a workflow.
</td>
<td>SoS supports explicit forward-style (sequential numbered steps), makefile-style (dependency), and mixed-style of subworkflows, and steps can be explicitly dependent upon.</td>
<td>Nextflow specifies process with input and output, and creates DAG of steps (the processes).</td>
<td>Relies on <a href="http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html">filename (pattern) matching</a> to determine execution sequence.</td>
<td>Bpipe specifies stages with input and output, and creates DAG from the stages.</td>
<td>DAG is constructed from source of steps</td>
<td>DAG of galaxy is built explicitly using its web interface.</td>
</tr>


<tr class="result">
<th align="left">Streaming processing</th>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Optional</td>
<td align="left">No</td>
</tr>

<tr class="detail">
<td>Ability to process tasks inputs/outputs as a stream of data.
</td>
<td>SoS "data" are passed around as files.</td>
<td>Processes in nextflow can communicate via asynchronous FIFO queues, called channels in the Nextflow lingo. </td>
<td>From Snakemake 5.0 on, it is possible to <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html?highlight=pipe#piped-output'>mark output files as pipes</a>. </td>
<td>
	input and output variables are files.</td>
<td>The input files can be "streamable" and may be handled by pipes</td>
<td>Galaxy does not support streaming between steps.</td>
</tr>

<tr class="result">
<th align="left">Subworkflow</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Support for executing subworkflows, potentially loaded from another pipeline file.
</td>
<td>SoS provides a <code>sos_run(name)</code> function to dynamically execute a subworkflow.</td>
<td>Nextflow supports subworkflows through the use of </a href="https://www.nextflow.io/docs/edge/dsl2.html#modules">submodules</a></td>
<td>Rules can be loaded from other text files. Subworkflows can be achieved by setting input of one workflow explicitly as output of another workflow.</td>
<td>Bpipe <code>run</code> keyword uses <code>+</code> operator to connect selected stages to pipeline. The <code>Load</code> statement can be used to import variable and pipeline stages from other files.</td>
<td>A CWL workflow can be used in place of a regular CWL step</td>
<td>Subworkflows are supported, although they cannot be generated dynamically as other workflow tools.</td>
</tr>

<tr class="result">
<th align="left">Atomic Write</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Implementation dependent</td>
<td align="left">Likely yes</td>
</tr>

<tr class="detail">
<td>Generate output only when the step completes so that failed steps do not leave incomplete output files.
</td>
<td>SoS uses step signature to track the output of steps and will remove partial output when the step fails.</td>
<td>Nextflow steps are executed in a stage area so all outputs are complete.</td>
<td>Snakemake uses .snakemake/incomplete_files to track paritial output files from failed runs.</td>
<td>Output from failed steps got cleaned up so failed steps will not get in the way during re-execution</td>
<td>The CWL specification does not require atomic write but individual workflow engine will likely implement it in some way</td>
<td>We could not find any information related to how galaxy recovers from failed steps. It is likely that its steps are staged so writes are atomic.</td>
</tr>

<tr class="result">
<th align="left">Named input/output</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Label step input and output and use the labels to connect steps as flow of data
</td>
<td>SoS supports named input and output through keyword arguments in input and output statements and refer to them with functions <code>named_output</code></td>
<td>The "from" part of input essentially names the input</td>
<td>Snakemake support <a href="https://snakemake.readthedocs.io/en/stable/snakefiles/rules.html">named input</a> through keyword arguments in input and output statements.</td>
<td>There seems to be no way to group input by names in bpipe</td>
<td>CWL supports named output and the creation of data flow</td>
<td>Galaxy workflows explicitly lables input and outputs</td>
</tr>




<tr class="result">
<th align="left">Modify and resume</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes (Optional for other engines)</td>
<td align="left">No (?)</td>
</tr>

<tr class="detail">
<td>Able to resume interrupted or modified workflow and ignore parts of the workflow that have been successfully executed</td>
<td>SoS automatically keeps signatures of steps and tasks and can ignore steps and tasks that have already
been executed, even if they were executed by a different workflow.</td>
<td>Nextflow keeps track of all the processes executed in your pipeline. If you modify some parts of your script, only the processes that are actually changed will be re-executed. The execution of the processes that are not changed will be skipped and the cached result used instead.</td>
<td>Similar to Make, Snakemake uses timestamps to determine modification status and resume points.</td>
<td>Uses customized timestamp signature (at millisecond resolution) of input / output to determine modification. By default <a href='https://github.com/ssadedin/bpipe/issues/157'>it does not</a> check status of command or script changes.</td>
<td>Pausing and resuming workflow is not part of the specification and is not required </td>
<td>No information on runtime signature or restart of failed jobs could be found.</td>
</tr>

<tr class="result">
<th align="left">Buit-in remote execution</th>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail">
<td>Send tasks to remote hosts for execution.</td>
<td>SoS can execute entire workflows or individual tasks on multiple remote hosts, with file synchronization between
heterogeneous file systems.</td>
<td>Nextflow can be executed on a variety of environments but it has to be started within the environments</td>
<td>Snakemake can be executed on a variety of environments but it has to be started within the environments</td>
<td>Bpipe can be executed on a variety of environments but it has to be started within the environments</td>
<td>The CWL specification does not contain any feature for remote execution.</td>
<td>Galaxy can be executed on a variety of environments but it has to be started within the environments</td>
</tr>

<tr class="result">
<th align="left">Task monitoring</th>
<td align="left">Command line and GUI (Notebook), with summary report</td>
<td align="left">Report traces and performances</td>
<td align="left">Report traces and performance</td>
<td align="left">Event notification</td>
<td align="left">Implementation dependent</td>
<td align="left">GUI to explore, share and reuse histories</td>
</tr>

<tr class="detail">
<td>Ability to send tasks to multiple isolated computing environment and manage them from local host.
"Report traces and performance" means that benchmarking commands and outputs are logged, along with resources usage such as CPU hours and memory consumption.</td>
<td>SoS can monitor tasks through the Jupyter Notebook interface with magics (e.g <code>%taskinfo</code>) to retrieve details about the tasks. It can also monitor status of tasks through a command line interface (e.g. <code>sos status</code>).
A summary report could be generated with <a href="https://vatlab.github.io/sos-docs/doc/tutorials/Execution_of_Workflow.html"  target="_blank">option <code>-p</code></a>.</td>
<td>Nextflow can generate <a href="https://www.nextflow.io/docs/latest/tracing.html#"  target="_blank">complete reports</a> with details on CPU/task usage etc.</td>
<td><a href="http://snakemake.readthedocs.io/en/stable/tutorial/additional_features.html#benchmarking"  target="_blank">Benchmarking</a> and <a href="http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html#log-files">logging</a></td>
<td><a href='http://docs.bpipe.org/Guides/Notifications/'  target="_blank">Notification in Bpipe</a> can be configured by Gmail, or genetric SMTP / XMPP protocols. It also provides commands such as <code>send, succeed, fail</code> for arbitrary notifications.</td>
<td>There is no mentioning of job monitoring of jobs in CWL specification, but workflow engines should provide their own facilities for job monitoring</td>
<td>The GUI shows the status of each step with colors.</td>
</tr>

<tr class="result">
<th align="left">Process-oriented workflow</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Workflows that are constructed and executed by steps to execute.</td>
<td>SoS' "forward-style" workflow specifies steps of workflows through sequencial numbering although a DAG could be constructed with target dependencies.</td>
<td>Nextflow executes specified workflow with specified input and parameters.</td>
<td>Snakemake workflow depends on filename wildcard pattern matching, not rule names, although <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html?highlight=rules#handling-ambiguous-rules'>rule order</a> and <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/rules.html?highlight=rules#priorities'>rule priorities</a> can be configured to change execution ordering.</td>
<td>Bpipe workflow is <a href='http://docs.bpipe.org/Guides/ParallelTasks/'>process-oriented and executed in parallel</a>. </td>
<td>CWL execute workflows from specified steps and inputs, not from desired output</td>
<td>Galaxy construct and execute workflows as connected steps.</td>
</tr>

<tr class="result">
<th align="left">Output-oriented workflow</th>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail">
<td>Workflows that are constructed and executed by the "outcome" of the workflow.</td>
<td>SoS' auxiliary steps specifies outcomes of steps and will be called when the target is needed.</td>
<td>Nextflow executes specified workflow with specified input and parameters.</td>
<td>Snakemake workflows are output-oriented: execution ordering relies on filename patterns (with exceptions).</td>
<td>Bpipe does not use implicit file name pattern matching to construct pipelines, although it supports input file wildcards for running multiple stages simultaneously on different data.</td>
<td>CWL does not use implicit workflow construction to execute workflow to generate specified outcomes</td>
<td>Galaxy does not automatically build workflows from intended outcomes</td>
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
<th align="left">CWL</th>
<th align="left">Galaxy</th>
</tr>
</thead>
<tbody>


<tr class="result">
<th align="left">Docker</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Support for docker</td>
<td>A <code>docker_image</code> option can execute scripts inside specified docker images.</td>
<td>Nextflow support <a href="https://www.nextflow.io/docs/latest/docker.html">docker containers. You can
run all scripts in the specified docker image, or specify a docker image for each step.</a></td>
<td>Snakemake supports the use of <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html?highlight=container#running-jobs-in-containers'>rule level containers</a>.</td>
<td>Bpipe does not have build-in support for containers.</td>
<td>CWL specification supports docker</td>
<td>Galaxy steps can execute <code>docker run</code> command with docker-flavored images.</td>
</tr>

<tr class="result">
<th align="left">Singularity</th>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">No</td>
</tr>

<tr class="detail">
<td>Support for singularity</td>
<td>SoS supports singularity with action options <code>container</code> and <code>engine</code>m see <a href="https://vatlab.github.io/sos-docs/doc/tutorials/Singularity.html">SoS Singularity Guide</a> for details.</td>
<td>Nextflow <a href="https://www.nextflow.io/docs/latest/singularity.html">supports singularity containers</a>.
It works similar to docker but with options such as <code>singularty.enabled=true</code>.</td>
<td>Snakemake supports the use of <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/deployment.html?highlight=container#running-jobs-in-containers'>rule level containers</a>.</td>
<td>Bpipe does not have build-in support for containers.</td>
<td>Not mentioned in CWL specification but cwltool supports it</td>
<td>Galaxy supports Singularity containers.</td>
</tr>

<tr class="result">
<th align="left">PBS/Torque/LSF/SLURM</th>
<td align="left">Needs template</td>
<td align="left">Yes</td>
<td align="left">Direct or via template</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Ability to execute workflows on a PBS-style computer cluster
</td>
<td>SoS interact with clusters through pre-configured templates and commands. It has been tested to work on
Torque, LSF, SLURM, PBS, and Torque</td>
<td>Nextflow supports Open grid, Univa grid, LSF, SLURM, PBS Works, Torque</td>
<td>Snakemake can interact with clusters through templates, or directly if the cluster supports <a href="http://www.drmaa.org/http://www.drmaa.org/">DRMAA</a>.</td>
<td>Bpipe provides <a href='http://docs.bpipe.org/Guides/ResourceManagers/'>build-in support for some resource manager systems</a>, and a template-based system (<a href='http://docs.bpipe.org/Guides/ImplementingAResourceManager/'>adapter script</a>) to support implementing resource managers.</td>
<td>cwltool and other implementations supports cluster</td>
<td>Galaxy can be deployed on clusters with steps executed on computing nodes.</td>
</tr>

<tr class="result">
<th align="left">HTCondor</th>
<td align="left">Require template (?)</td>
<td align="left">Yes</td>
<td align="left">Require template (?)</td>
<td align="left">Require template</td>
<td align="left">No (?)</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Ability to use <a href="https://research.cs.wisc.edu/htcondor/">HTCondor</a> to execute workflows on large collections of distributively owned computing resources.</td>
<td>Do not know because we have not had a chance to configure SoS to run on a HT Condor system.</td>
<td>Nextflow <a href="https://www.nextflow.io/docs/latest/executor.html#htcondor">supports HTCondor</a></td>
<td>There is no built-in support for HTCondor, however we cannot find existing Snakemake HTCondor job templates either.</td>
<td>There is no built-in support for HTCondor, however there seems to be <a href='https://github.com/GenomicParisCentre/eoulsan/blob/3b171735888f728b6804abcdfd7e7fce80b5218a/src/main/bin/bpipe-htcondor.sh'>third-party adapter scripts</a> for HTCondor job scheduler.</td>
<td>There seems to be no built-in support for HTCondor</td>
<td>Galaxy supports HTCondor as described <a href="https://galaxyproject.org/cloudman/ht-condor/">here</a>.</td>
</tr>

<tr class="result">
<th align="left">Distributed Task Queue</th>
<td align="left">Yes (RQ)</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">No</td>
</tr>

<tr class="detail">
<td>Ability to send tasks to distributed task queues such as <a href="http://python-rq.org/">RQ</a> and <a href="http://www.celeryproject.org/">Celery</a>.
</td>
<td>SoS supports RQ, Celery support is likely broken due to lack of maitainence.</td>
<td>Nextflow cannot submit tasks to external task queues</td>
<td>Snakemake cannot submit tasks to external task queues</td>
<td>Bpipe does not provide build-in support for external task queues</td>
<td>cwltools does not support external task queues</td>
<td>Galaxy does not support external task queues.</td>
</tr>

<tr class="result">
<th align="left">Distributed systems</th>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Experimental</td>
<td align="left">No</td>
<td align="left">Implementation dependent</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Ability to spawn the executions of pipeline tasks through a distributed cluster such as Apache Spark, Apache Ignite, Apache Mesos, and Kubernetes.</td>
<td>No</td>
<td>Nextflow supports distributed systems such as <a href="https://www.nextflow.io/docs/latest/ignite.html">Apache Ignite</a> and <a href="https://www.nextflow.io/docs/latest/kubernetes.html">Kubernetes</a></td>
<td>Snakemake 4.0 and later <a href='http://snakemake.readthedocs.io/en/stable/executable.html?highlight=Kubernetes'>supports experimental execution</a> in the cloud via Kubernetes.</td>
<td>No</td>
<td>No trace of support from cwltool but other workflow engines might support it</td>
<td>Galaxy could be delopyed on top of Kubernetes as described <a href="https://github.com/galaxyproject/galaxy-kubernetes">here</a> </td>
</tr>

<tr class="result">
<th align="left">Cloud Storage</th>
<td align="left">No</td>
<td align="left">Yes</td>
<td align="left">Yes</td>
<td align="left">No</td>
<td align="left">No</td>
<td align="left">Yes</td>
</tr>

<tr class="detail">
<td>Ability to make use of cloud storage (such as AWS).
</td>
<td>Not currently </td>
<td>Nexflow can <a href="https://www.nextflow.io/docs/latest/amazons3.html">access S3 storage</a></td>
<td>Snakemake can access files on <a href='http://snakemake.readthedocs.io/en/stable/snakefiles/remote_files.html'>cloud storage</a></td>
<td>No</td>
<td>No information could be found for support for cloud storage. This should again be implementation/engine specific.</td>
<td>Galaxy objects could be stored on distributed store or Amazon S3 (c.f. <a href="https://galaxyproject.org/object-store/">Galaxy Object Store</a>)</td>
</tr>

</tbody>
</table>
</div>


## Is SoS for you?

SoS is not for everyone. As a workflow system:

* If you are looking for a industrial-grade workflow system for the handling of millions of large jobs, you should look for proven solutions such as [Luigi](https://github.com/spotify/luigi).
* If you are aiming at the creation of "portable" workflows that can be executed in various cluster and cloud environments, [NextFlow](https://www.nextflow.io/) can be the first to try. [Snakemake](https://snakemake.readthedocs.io/en/stable/) also has a wide user base and is a close draw with NextFlow in many aspects. [Bpipe](https://github.com/ssadedin/bpipe) is also popular but seems to be less popular then NextFlow and SnakeMake.
* If you are aiming at the creation of "general" workflows with no specific workflow engine in mind, [CWL](https://github.com/common-workflow-language/common-workflow-language) is currently the best bet as CWL workflows can be executed by multiple workflow engines in different environments.
* If you are looking for a script-less GUI-based workflow system with the need for writing scripts, the answer is no because SoS is script based. [Galaxy](https://usegalaxy.org/) can be a good choice at least for bioinformatic applications.
* If you are a **Jupyter** or **JupyterLab** user, the answer is most likely yes because SoS is embedded into SoS Notebook, which is by itself a polyglot notebook. You can enjoy all features of SoS Notebook and step into SoS only when needed.
* If you would like to use **a workflow system for daily exploratory data analysis and computaional research**, SoS should be most usable since it is designed for interaction data analysis and execution of tasks on remote systems.
