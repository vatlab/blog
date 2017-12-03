---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS Notebook: a multi-language kernel for Jupyter"
ghcommentid: 2
category: "SoS Notebook"
date: "2017-11-29"
tags: ["Jupyter", "Multi-language", "Notebook"]
---

I started to use IPython, and then [Jupyter](http://jupyter.org/) more than ten years ago but despite of all the nice features,
there were always something missing, something that prevented me from using it as my main working environment. I still liked
Jupyter and used it from time to time, but when I tried to write a Jupyter kernel for my 
[SoS Workflow Engine](https://vatlab.github.io/sos-docs/), I realized that I really need to expand Jupyter to make SoS useful
for interactive data analysis, so I developed SoS Notebook, a multi-language kernel for Jupyter.

## What Jupyter is lacking

Instead of jumping directly into what SoS Notebook can do for you, allow me to explain what Jupyter lacks as an interactive
data anlysis environment, at least for a bioinformatician like me.

### Line by line execution of scripts

This is IMHO the single most important feature that is missing from Jupyter. In Jupyter, the basic execution units are cells so
there is no way for you to execute, tweak, and re-execute pieces of the code for debugging purposes
[Jupyter Issue 1094](https://github.com/jupyter/notebook/issues/1094). The problem here is actually not how difficult it is
for Jupyter to execute part of the cell content (it is actually quite easy), but where to execute the code and display the
output. The cell output in the main notebook is certainly not a good place because you do not want to change the output of
the entire cell when you would like to inspect the value of a single variable. A Jupyter notebook extension called 
[scratchpad](https://github.com/minrk/nbextension-scratchpad) was developed in 2015 to
alleviate this problem but you will have to manually enter (or copy/paste) the code to be executed, whichis quite inconvenient
compared to line-by-line execution mode of other IDEs.

### The single-kernel model of Jupyter notebooks

Jupyter supports virtually all scripting languages ever invented but each notebook can only use one of the kernels. This
is usually acceptable because almost all IDEs and working environments works in this manner. However, with increasing
complexity of modern data analysis, especially in the field of bioinformatics, analyzing data with more than one scripting
languages has become more and more a necessity. Keeping the entire data analysis in a single notebook has obvious advantages
in the book-keeping and sharing of data analysis, and allows easier reproducibility of prior analysis. Unfortunately,
with the emergence of several multi-language notebooks such as [R Notebooks](http://rmarkdown.rstudio.com/r_notebooks.html),
[Apache Zeppelin](https://zeppelin.apache.org/), and [Beaker Notebook](http://beakernotebook.com/) 
(Now [BeakerX](https://github.com/twosigma/beakerx)), Jupyter (even JupyterLab) does not seem to be moving in this
direction.

### Literate programming and other missing usability features

There are also many others features that, IMHO, should have been there in Jupyter. For example, whereas RSdutio allows
in-line interpolation (Literate programming), Jupyter has failed to provide such a feature after years of discussions
[ipython #2592](https://github.com/ipython/ipython/pull/2592). The developers has 
[explained](https://github.com/jupyter/help/issues/41) the complexity of this feature but even a non-perfect soluation
is better than none.

Other features that I have found useful but missing include but not limited to the ability to clear cell outputs 
(e.g. when an output is really long and non-informativa), the ability to execute long scripts without doubling 
braces in cell magics (e.g. [%%script](http://ipython.readthedocs.io/en/stable/interactive/magics.html), preview of
file content, and table of content. Sure, some of the features can be achived with nbextensions (e.g. 
[toc2](https://github.com/ipython-contrib/jupyter_contrib_nbextensions/tree/master/src/jupyter_contrib_nbextensions/nbextensions/toc2)
but I suppose only experienced Jupyter users know these.

## SoS Notebook

[SoS Notebook](https://vatlab.github.io/sos-docs/) is a kernel for the SoS Workflow Engine, but the SoS Kernel itself
is first a multi-language notebook with many extensions to the Jupyter frontend. It is useful by itself if you ignore
the workflow engine part (which we will introduce in a separate post). For the impatience, here is a youtube video for
an overview of SoS Notebook

{{< youtube xrwhNMRTBp4 >}}


### Multi-language notebook

The first thing you can notice is that language-selection dropdown box at the top right corner of each cell. This dropdown
box allows you to change the kernel of a notebook cell, to any of the installed Jupyter kernels (called subkernels in
SoS). This allows you to, for example, execute a shell script using the [Bash kernel](https://github.com/takluyver/bash_kernel)
in a cell, import data in Python (SoS, because SoS is based on Python), and analyze data in R using an
[ir kernel](https://github.com/IRkernel/IRkernel).

![Demo of SoS Notebook](https://vatlab.github.io/sos-docs/doc/media/SoS_Notebook.gif)

In addition to the per-cell language-selection dropfown, you can also use the global language-selection dropdown to change
the global default kernel for new cells, and magics such as [`%use`](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html#%use-1023)
and [`%with`](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html#%with-1024) to switch kernels in the cell.

It is worth noting that SoS uses string interpolation (Python f-string) to compose scripts in other cells. However,
instead of interpolating expressions in braces (e.g. `{variable}`) by default, SoS by default does not interpolate
cell content and allows you to use cells in subkernels without worrying about the inclusion of braces in the scripts.
If there is a need to pass variables from SoS to the subkernels, you can use the

```
%expand
```

magic to expand expressions in braces with variables in the SoS kernel. Furthermore, if your script in the subkernel
contains many braces, you can specify an alternative sigil so that you do not have to double the braces for string
interpolation. For example, in a cell with Bash subkernel, you can do
 
```
%expand %( )
for i in {1..25}
do
    echo ${i} > %(filename)
done
```
to pass `filename` to the subkernel because it is unsafe to expand the script with either `{ }` or `${ }`.


### Inter-language data exchange

The power of this multi-kernel configuration lies in its persistent execution model in which data flow through cells of the
same kernel and cross cells to other live kernels. To enable SoS Notebook to exchange data among these subkernels, a data 
exchange protocol is defined for a subkernel to receive variables from and send variables to the SoS kernel, and optionally
also send variables to other subkernels, allowing any two kernels to exchange data either directly or via SoS. Because of
large differences in datatypes between scripting languages, SoS tries to convert variables to the most similar datatypes in
the destination language.

For example, although `3` and `c(3, 5)` are both numeric arrays in `R`, they are converted to Python as integer `3` and 
numpy `array([3, 5])` respectively. Similarly, a Python `DataFrame` is converted to `data.frame` in `R`, `dataset` in SAS,
`table` in `Matlab`, `dataframe` in Octave, and nested dictionaries in JavaScript. In contrast with some implementations in
which an object in one language is made available to another via a wrapper (e.g. rpy2), SoS creates independent homonymous
variables of similar types that are native to the destination language. Actual data exchange happens both in memory and 
through disk files (e.g. using feather format to exchange dataframes between Python and R), depending on the type and
size of the variables. With the magics `%put` and `%get`, it is very easy to, for example, transfer a Python DataFrame to R
as an R data.frame or SAS as a dataset for statistical analysis, and send the results to JavaScript for dynamic visualization.
In addition, variables with names starting with sos will be automatically transferred between kernels. As a shortcut 
to executing statements in another kernel as if calling a local function, the magic “%with kernel --in v1 --out v2” 
allows for evaluation of a cell in another language with variables sent from the current kernel (v1) and returning results
(v2) to the current kernel after the execution of 

### Side panel and line-by-line execution of cells

Whereas the main notebook records all scripts and their results persistently,

the side panel is used to perform a variety of nonpermanent actions, such as executing scratch commands,
showing results of line-by-line execution, previewing variables and files, and showing the table of contents 
of the notebook. A number of keyboard shortcuts are provided to facilitate these actions.

###	Preview of variables, expressions and files

Instant feedback from executed codes is crucial for smooth interactive data analysis.
SoS Notebook provides a mechanism to preview variables and results of expression from any kernel,
and files in many different formats, including bioinformatic-specific formats such as fastq, vcf, and bam.
Depending on the nature of the data, the previewer displays dimensions and records for tables (dataframe-like 
variables or files in csv or excel format), shows images in different formats, lists contents of compressed
files, and summarizes key information such as the length of reads for fastq files and the reference genome used for VCF files.

The preview feature of SoS Notebook is tightly integrated into the interactive data analysis workflow. 
For example, when the current line or selected text is sent to the side panel for evaluation (using the 
keyboard shortcut C-S-Enter), SoS automatically determines whether the code is a single variable or an 
assignment statement and previews the resulting variable. This works for different syntaxes in different 
languages (e.g., a <- 3 in R) and is arguably easier to use than looking for a variable in a dedicated
window that lists all variables in some IDEs such as RStudio.
The %preview magic, with its many options, can be used to preview expression in SoS and any subkernels,
preview files on local and remote hosts, save displayed results in the main notebook or side panel, and control
the style of preview results. In particular, it can preview Python DataFrames in scrollable, sortable, and searchable tabular format, or in interactive scatterplot with a tooltip for each data point. These features make it easy to present and search large tables, show sample distribution and identify outliers of the data.


### Other SoS Notebook features

SoS Notebook provides a large number of magics.

It is worth noting that SoS passes unrecognized magics to the subkernels so 

If you find SoS Notebook useful, please support the project by starring the [SoS](https://github.com/vatlab/SoS) and [SoS Notebook](https://github.com/vatlab/sos-notebook)
github project, or spreading the word with [twitter](https://twitter.com/ScriptOfScripts).
