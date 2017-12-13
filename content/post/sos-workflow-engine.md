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
**everyone knows the benefits of workflow systems, everyone has heard of and has talks about the power and popularity of
some workflow systems, but yet no one I know actually uses a workflow system for their daily computational research.**

When I started to analyze bioinformatic datasets, I naturally encountered many problems with
running and managing large numbers of jobs locally and on cluster systems and I started to look for a good workflow
system. There are [more than 100 workflow systems](https://github.com/pditommaso/awesome-pipeline) to choose from so
there has to be at least one that fits my need. I literally went through all the listed pipeline systems, looked into
20+ and tried  more than 10 of them, and I finally understood why nobody is using a workflow system in daily 
computational research. Basically, as
[Loman and Watson, 2013, Nat Biotechnol](https://www.nature.com/articles/nbt.2740) pointed out: because pipelines force
you into a rigid way of thinking and into the use of separate tools, **pipeline too early, or pipeline analyses that will be run only once, or pipeline
analyses that are not meant to be shared with or repeated by others can decrease creativity and productivity**.

## Do we need a workflow system for daily computational research?

: they are designered for end users of workflows, not for creators of workflows, that is to say,
**existing workflow systems are not designed for daily computational research**


