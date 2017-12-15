---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS: a workflow system designed for daily computational research (still writing)"
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
sharing, and reproduction of bioinformatic data analyses. Notably,

1. It is tedious and error-prone to apply a multi-step workflow to a large amount of data, especially if you
  need to execute them on different systems. 
2. It becomes difficult to share your analysis with others and to reproduce it later on if it
  consists of multiple scripts in different languages. 

However, although workflow systems are designed to solve these kind of problems, after surveying all
[100+ workflow systems](https://github.com/pditommaso/awesome-pipeline) and trying
more than 10 of them, I ended up doing what other people are doing, writing more disposable scripts. The reason
was simple, **the overhead of applying a workflow system in daily data analysis is too high to help productivity**.
As a matter of fact, as pointed out by [Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740),
using a workflow system for analyses that will be run only once, or not meant to be shared with or repeated
by others can decrease creativity and productivity.

> #### Pipelineitis is a nasty disease
>
> A pipeline is a series of steps, or software tools, run in sequence according to a predefined plan.
> Pipelines are great for running exactly the same set of steps in a repetitive fashion, and for 
> sharing protocols with others, but they force you into a rigid way of thinking and can decrease creativity.
>
> **Warning**: don’t pipeline too early. Get a method working before you turn it into a pipeline. And
> even then, does it need to be a pipeline? Have you saved time? Is your pipeline really of use to
> others? If those steps are only ever going to be run by you, then a simple script will suffice and 
> any attempts at pipelining will simply waste time. Similarly, if those steps will only ever be run
> once, just run them once, document the fact you did so and move on.


## Pipelineitis does not have to be a nasty disease

Looking over and over at the key obstacles that prevent the application of workflow systems in daily
computational research, namely:

1. A workflow system uses a different environment from interactive data analysis, thus require users
  to re-create cripts for a workflow system.
2. Many workflow systems require the use of different workflow languages that are quite difficult
  to learn. Nice GUIs can alleviate the problem but on the other hand hide the details of workflows.

We developed a [SoS (Script of Scripts)](https://vatlab.github.io/sos-docs/) as a workflow engine
with a multi-language notebook frontend. This system solves the aforementioned problem as follows:

1. It provides a single environment for both interactive data analysis and batch data processing so
  users do not have to re-create workflows in another environment.
2. The workflow engine is designed to be as easy to use as possible with minimal change to scripts
  to be executed.
3. The workflow engine is extended from the Python programming language so it is instantly familiar
  to a large number of users.
4. The workflow engine does not force users to choose between forward-style (process-oriented)
  and makefile-style (outcome-oriented) workflows and support both styles.
5. The workflows can be annotated with markdown cells and results.

This post  demonstrates the key features of SoS and explains how it can be applied to daily
computational research and increases your productivity.

## A single environment for both interactive and large-scale data processing

## Smooth transition from 

## A single environment for local and remote data processing

## A single notebook for reproducibility

