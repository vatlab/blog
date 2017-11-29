---
type: "post"
draft: false
author: "Bo Peng"
title: "One Jupyter notebook, multiple languages"
description: "Introduction to SoS Notebook"
keywords: ["SoS Notebook", "Jupyter"]
topics: ["topic 1"]
ghcommentid: 2
tags: ["Jupyter", "SoS", "SoS Notebook", "Multi-language", "Notebook"]
---

Jupyter supports a large number of kernels for virtually all scripting languages ever invented, but despite of all the magical ways (magics) to call shell commands and scripts in other languages, one notebook only supports one kernel. You will have to create multiple notebooks if your analysis involves multiple scripting languages, making it difficult to organize, share and reproduce.

A SoS (Script of Scripts) kernel has been developed to fill this gap, basically allows you to have multiple live kernels in the same notebook. Besides obvious advantages in fitting your entire multi-language analysis in one notebook, it allows you to transfer variables freely between kernels using SoS magics so that you can gather your data in Python, analyze it in R or MATLAB, plot the results in JavaScript. Data types in different scripting languages are mapped heuristically so a R data.frame will be mapped as Pandas DataFrame in Python, dictionary in JavaScript, and table in MATLAB.

The SoS Notebook based on the SoS kernel also tries to address several of Jupyter's limitations by providing

A side panel to execute scratch commands, preview variables (in multiple kernels) and files, display table of content, and more importantly, allows
Line-by-line execution of scripts in cells so that you can step through your code for debugging purposes
A large number of magics to render cell output as markdown or HTML, to clear output of cells, and to preview variables and files
Support for the SoS workflow engine so that you can organize jupyter cells as workflows and execute them in a non-linear fashion
SoS Notebook is hosted at https://github.com/vatlab/SoS with complete documentation hosted at https://vatlab.github.io/SoS. You can check out its video library for an overview (https://vatlab.github.io/sos-docs/#documentation) or start playing with it using a live SoS server (top right button at the SoS homepage).
