---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS: a simple yet powerful workflow system for daily computational research"
ghcommentid: 8
date: "2017-12-12"
tags: ["SoS", "Workflow"]
---

A phenomenon has puzzled me for quite a while when I started my career as a bioinformatician, that is to say,

> **Everyone knows the benefits of workflow systems, everyone has heard of and has talks about the power and popularity of
  some workflow systems, but yet no one I know actually uses a workflow system for their daily computational research**

When I started to analyze bioinformatic datasets, mostly RNA Seq data, I naturally encountered many problems with
running and managing large number of jobs locally and on cluster systems and I started to look for a good workflow
systems. There are many workflow systems, actually there are more than 100 workflow systems listed on 
[awesome pipelines](https://github.com/pditommaso/awesome-pipeline) so there has to been something that fits my
need. I literally went through all the listed pipeline systems, looked into 20+ and tried to use more than 10 of them, and
I finally understoodd why nobody is using a workflow system in daily computational research: they are designered
for end users of workflows, not for creators of workflows, that is to say, **they are not designed for us, 
not for daily computational research**.




