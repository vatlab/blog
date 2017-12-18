---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS: a cure to pipelineitis"
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
which is a workflow engine with a multi-language notebook frontend, to treat this nasty disease
called pipelineitis:

Symptom | Cause | Treatment
---|---| ---|
**Lower productivity** | Re-create data analysis using another language or environment | <ul><li>A <a href="https://vatlab.github.io/blog/post/sos-notebook/">single environment</a> for both interactrive data analysis and batch data processing</li><li><a href="https://vatlab.github.io/blog/post/power-of-sos-plus-sos-notebook/">Easy transition</a> from notebook to workflow</li></ul>
**Steep learnin curve** | Workflow language difficult to learn and use, making it difficult to adopt | <ul><li>Workflow as "annotations" to Python scripts</li><li>Trivial to get started, grow as needed</li></ul>
**Difficult to read and share** | Data analysis logics (core steps)<ul><li>buried in execution logics</li><li>or written in foreign workflow language</li><li>or hidden behind workflow GUI</li></ul>|<ul><li>Almost verbatim inclusion of scripts</li><li>Simple workflow directives</li><li>Single multi-language notebook with annotated workflow</li></ul>
**Difficult to reproduce** | Data analysis scattered in multiple scripts for multiple systems | <ul><li>Ability to execute entire or parts of workflows remotely</li><li>Keep local and remote tasks in one notebook</li></ul>

This post demonstrates the key features of SoS and explains how it can be applied to daily
computational research and increases your productivity.

## A single environment for both interactive and large-scale data processing

As shown in [this post](https://vatlab.github.io/blog/post/power-of-sos-plus-sos-notebook/), the SoS
environment allows you to develop your scripts and workflow steps interactively before you convert
them to formal workflows.

## A single environment for local and remote data processing

## A single notebook for reproducibility

