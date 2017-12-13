---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS: a workflow system designed for daily computational research"
ghcommentid: 8
date: "2017-12-12"
tags: ["SoS", "Workflow"]
---


A phenomenon has puzzled me for quite some time when I started my career as a bioinformatician, that is to say,
**everyone knows the benefits of workflow systems, everyone has heard of and talks about the power and popularity of
some workflow systems, but yet no one I know actually uses a workflow system for daily computational research.**
People programm in different scripting languages using their favoriate langauges and IDEs, execute them locally or
submit them to cluster system with shell and other scripts, all without the use of a workflow system.

## Pipelineitis is not necessarily a nasty disease

When I started to analyze bioinformatic datasets, I wrote numerous scripts for different projects and 
encountered many problems with developing, running, managing, sharing, and reproduction of bioinformatic
data analyses. Notably,

1. It is tedious and error-prone to apply a multi-step workflow to a large amount of data, especially if you
  need to execute them on different systems. 
2. It becomes difficult to share your analysis with others and to reproduce it later on if it
  consists of multiple scripts in different languages. 

Since a workflow system is meant to address these problems, I started to look for a good workflow system for daily
bioinformatic research. There are [more than 100 workflow systems](https://github.com/pditommaso/awesome-pipeline) to
choose from so there has to be at least one that fits my need. I literally went through all the listed pipeline systems,
looked into 20+ and tried  more than 10 of them, and I finally understood why nobody is using a workflow system in daily 
computational research. That is to say, **because of high overhead of learning and using a workflow system,
using a workflow system for analyses that will be run only once, or not meant to be shared with or repeated
by others can decrease creativity and productivity**, which was summarized quite nicely in 
[Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740):

> ### Pipelineitis is a nasty disease
>
> A pipeline is a series of steps, or software tools, run in sequence according to a predefined plan.
> Pipelines are great for running exactly the same set of steps in a repetitive fashion, and for 
> sharing protocols with others, but they force you into a rigid way of thinking and can decrease creativity.
>
> Warning: don’t pipeline too early. Get a method working before you turn it into a pipeline. And
> even then, does it need to be a pipeline? Have you saved time? Is your pipeline really of use to
> others? If those steps are only ever going to be run by you, then a simple script will suffice and 
> any attempts at pipelining will simply waste time. Similarly, if those steps will only ever be run
> once, just run them once, document the fact you did so and move on.

However, because of the biggest obstable of adopting an existing
workflow system is its steep learning curve and the fact that it forces you to re-engineer data analysis
using another environment even language, we believe that **pielineitis can be beneficial if we can
create a truly easy-to-use workflow system** and developed 
[SoS (Script of Scripts)](https://vatlab.github.io/sos-docs/). In stead of listing all the features of
SoS, this blog demonstrates the design ideas of SoS and how it can help you in daily computational
research.

## A single environment for both interactive and large-scale data processing

: they are designered for end users of workflows, not for creators of workflows, that is to say,
**existing workflow systems are not designed for daily computational research**

## Smooth transition from 

## A single environment for local and remote data processing

## A single notebook for reproducibility

