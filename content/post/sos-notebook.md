---
type: "post"
draft: false
author: "Bo Peng"
title: "SoS Notebook: a multi-language notebook for Jupyter"
ghcommentid: 2
category: "SoS Notebook"
date: "2017-11-29"
tags: ["Jupyter", "Multi-language", "Notebook"]
---


I started to use IPython, and then [Jupyter](http://jupyter.org/) more than ten years ago but despite of all the nice features,
there were always something missing, something that prevented me from using it as my main working environment. Notably, Jupyter lacks

* **Line by line execution of scripts**: The basic execution units of Jupyter are cells so
there is no way for you to execute, tweak, and re-execute pieces of the code for debugging purposes
[Jupyter Issue 1094](https://github.com/jupyter/notebook/issues/1094). The problem here is not how difficult it is
for Jupyter to execute part of the cell content, but where to execute the code and display the
output. You could use a [scratchpad](https://github.com/minrk/nbextension-scratchpad) extension to execute scratch cells but 
this is still less convenient than line-by-line execution mode of other IDEs.

* **Support for multiple kernels in one notebook**: Jupyter supports virtually all scripting languages ever invented but each 
notebook can only use one of the kernels. With increasing complexity of modern data analysis, especially in the field of
bioinformatics, analyzing data with more than one scripting languages has become more and more a necessity. Keeping the
entire data analysis in a single notebook has obvious advantages in the book-keeping and sharing of data analysis, and allows
easier reproducibility of prior analyses. Unfortunately, despite of the emergence of several multi-language notebooks such as
[R Notebooks](http://rmarkdown.rstudio.com/r_notebooks.html),
[Apache Zeppelin](https://zeppelin.apache.org/), and [Beaker Notebook](http://beakernotebook.com/) 
(Now [BeakerX](https://github.com/twosigma/beakerx)), Jupyter (even JupyterLab) does not seem to be moving in this
direction.

* **Literate programming and some other usability features**: Whereas RSdutio allows in-line interpolation in Markdown paragraphs
(Literate programming), Jupyter has failed to provide such a feature after years of discussions 
[ipython #2592](https://github.com/ipython/ipython/pull/2592). The developers has 
[explained](https://github.com/jupyter/help/issues/41) the complexity of this feature but IMHO even a non-perfect soluation
is better than no solution. Other features that I have found useful but missing include but not limited to the ability to clear cell outputs 
(e.g. when an output is really long and non-informative), the ability to execute long scripts without doubling 
braces in cell magics (e.g. magic [%%script](http://ipython.readthedocs.io/en/stable/interactive/magics.html)), and preview of
variables even content of files.

## SoS Notebook

When I started to implement a Jupyter kernel for the [SoS Workflow Engine](https://vatlab.github.io/sos-docs/), I decided to
tackle these limitations to provide an interactive multi-language data analysis environment. Because SoS is based on Python 3.6,
you can use a SoS Notebook just like a Python 3 kernel with multi-language support. I will explain the SoS workflow engine in
another post.

The rest of this post will explain features of SoS Notebook in details. However, if you are already feeling impatient, you can
click the <i class="fa fa-rocket" aria-hidden="true"></i>
button at the top right corner of the [SoS Homepage](https://vatlab.github.io/sos-docs/) and start a SoS notebook
while watching the follow video:

{{< youtube xrwhNMRTBp4 >}}


### Multi-language notebook

The first thing you will notice from a SoS notebook is the language-selection dropdown boxes at the top right corner of each cell. This dropdown
box allows you to change the kernel of a notebook cell to any of the installed Jupyter kernels (called subkernels in
SoS Notebook). This allows you to, for example, execute a shell script using the [Bash kernel](https://github.com/takluyver/bash_kernel)
in a cell, import data in Python (SoS), and analyze data in R using an
[ir kernel](https://github.com/IRkernel/IRkernel). 
In addition to the per-cell language-selection dropfown, you can also use the global language-selection dropdown to change
the global default kernel for new cells, and magics such as [`%use`](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html#%use-1023)
and [`%with`](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html#%with-1024) to switch kernels in the cell.

![Demo of SoS Notebook](https://vatlab.github.io/sos-docs/doc/media/SoS_Notebook.gif)

It is worth noting that SoS uses string interpolation ([Python f-string](https://www.python.org/dev/peps/pep-0498/)) to
compose scripts before sending them to subkernels. However, unlike the iPython `%%script` magic that always expand
expressions in braces (e.g. `{variable}`), **SoS by default does not interpolate cell content** so that you can use
subkernels freely without worrying about the inclusion of braces in the scripts.
If there is a need to pass variables from SoS to the subkernels, you can use the

```
%expand
```

magic to expand expressions in braces with expressions in the SoS kernel. Furthermore, if your script in the subkernel
contains many braces, you can specify an alternative sigil so that you do not have to double the braces for string
interpolation. For example, in a cell with Bash subkernel, you can pass `filename` to the subkernel with sigil `%( )`
as follows if it is unsafe to expand the script with either `{ }` or `${ }`:
 
```
%expand %( )
for i in {1..25}
do
    echo ${i} >> %(filename)
done
```

![expand magic](https://vatlab.github.io/sos-docs/doc/media/expand_magic.gif)

### Inter-language data exchange

If you followed the animations in the first figure, you might have noticed another way to pass variables
from one kernel to another, namely the use of the `%get` magic. This magic allows you to 
transfer variables in one to another kernel, so whereas it is correct to think of a SoS notebook with multiple
subkernels as multiple trains (cells belonging to each subkernel) running on their own tracks, 


![multi kernels](https://vatlab.github.io/sos-docs/doc/media/data_exchange/Slide1.png)

the **data actually flow through cells of the same kernel and cross cells to other live kernels** as shown in the following
figure


![data flow](https://vatlab.github.io/sos-docs/doc/media/data_exchange/Slide2.png)

This allows you to, for example, load and clean your data in Python, analyze them in R, SAS, or MATLAB, and plot the results
in other kernels such as JavaScript. The data exchange can be done explicitly with magic `%get`

![magic get](https://vatlab.github.io/sos-docs/doc/media/data_exchange_magic_get.png)

implicitly for variables with names starting with `sos`

![auto data exchange](https://vatlab.github.io/sos-docs/doc/media/data_exchange_auto.png)

or as side effect of kernel switch

![magic with](https://vatlab.github.io/sos-docs/doc/media/data_exchange_magic_with.png)

The last method is pretty interesting because it allows you to execute a cell in another kernel with input and
output variables, similar to calling a function.

Exchanging data of different types between live kernels in different languages is a very chanllenging task and requires
coordination between sending and receiving kernels, either directly or by way of SoS. SoS currently
supports data exchange of most native data types among SoS and nine supported languages and you are encouraged to
check out the SoS manual on [Supported Languages](https://vatlab.github.io/sos-docs/doc/documentation/Supported_Languages.html)
for details. But just to clarify a few questions you might already have,

1. SoS does not copy or transfer variables from one kernel to another. It **creates independent homonymous
variables in the destination kernel** so that changing variables in another kernel does not affect the variables
in the sending kernel.

2. SoS **creates variables of similar types that are native to the destination language**. For example, although `3` 
and `c(3, 5)` are both numeric arrays in `R`, they are converted to Python as integer `3` and 
numpy `array([3, 5])` respectively. Similarly, a Python `DataFrame` is converted to `data.frame` in `R`, `dataset` in SAS,
`table` in `Matlab`, `dataframe` in Octave, and nested dictionaries in JavaScript. 

3. Data exchange between subkernels are usually done by way of SoS (e.g. `kernel A` -> `SoS` -> `Kernel B`) but the protocol
allows direct data exchange between subkernels.


### Side panel, line-by-line execution of cells

SoS Notebook provides a side panel that can be used to execute a variety of nonpermanent actions, namely actions that
are temporary in nature and with results kept outside of the main notebook. Such actions include executing scratch commands,
showing results of line-by-line execution, previewing variables and files, and showing the table of contents 
of the notebook. The most important feature is the **`Ctrl-Shift-Enter` keyboard shortcut for executing
selected code (or current line if no code is selected) in the side panel**. As you can see from the following figure, the
side-panel automatically switch langauges so you can step through your cells in any language.

![step through](https://vatlab.github.io/sos-docs/doc/media/step_through.gif)

### Preview of variables and files

Instant feedback is crucial for smooth interactive data analysis so SoS Notebook went a long way in providing
a really powerful magic called `%preview`. Basically, the `%preview` magic can

1. Preview variables and expressions (quotation required if expressions contain spaces) in any kernel.
2. Preview files in many different formats, including bioinformatic-specific formats such as fastq, vcf, and bam.
Depending on the nature of the data, SoS shows  images in different formats, lists contents of compressed
files, and summarizes key information such as the length of reads for fastq files and the reference genome used for VCF files.
3. Preview Python DataFrames in scrollable, sortable, and searchable tabular format, or in interactive scatterplot with a tooltip for each data point.
These features make it easy to present and search large tables, show sample distribution and identify outliers of the data.
4. Automatically determines whether the code is a single variable or an assignment statement and previews the resulting
variable. This works for different syntaxes in different languages (e.g., a <- 3 in R) and is arguably easier to use than
looking for a variable in a dedicated window that lists all variables in some IDEs such as RStudio.

![step through](https://vatlab.github.io/sos-docs/doc/media/preview_magic.gif)

The `%preview` magic also provides options to preview results in the side panel or main document,
preview files on local and remote hosts, save displayed results in the main notebook or side panel, and control
the style of preview results. Please refer to 
[the documentation of `%preview` magic](https://vatlab.github.io/sos-docs/doc/documentation/Notebook_Interface.html#Preview-of-results-13) for details.


### Summary

SoS Notebook was started as a Jupyter kernel for the SoS workflow engine but evolved to a multi-language notebook to provide users to 
complete environment for interactive data analysis using multiple languages. This post lists several of its key features but these are
more interesting magics such as

1. A `%render` magic is provided to render output from any kernel as markdown, HTML, svg etc, which essentially allows inline
string interpolation from any subkernel.
2. A `%clear` magic can be used to suppress output of cells to be executed, or clear output of selected or all cells after they are executed.
3. A `%toc` magic to display dynamically updated table of content in the side panel.
4. A `%sessioninfo` magic to collect and display session information from all running kernels.

More details of these magics and SoS Notebook in general can be found at the [SoS Notebook documentation](https://vatlab.github.io/sos-docs/doc/documentation/Notebook_Interface.html#Preview-of-results-13),
[SoS Magics](https://vatlab.github.io/sos-docs/doc/documentation/SoS_Magics.html), [Supported Languages](https://vatlab.github.io/sos-docs/doc/documentation/Supported_Languages.html),
and videos from the [SoS Video Library](https://vatlab.github.io/sos-docs/index.html#documentation).

SoS Notebook is fairly stable but we would not want to make a 1.0 release without hearing from you. Please test SoS Notebook and send
us your feedback and/or bug report to our [github issue tracker](https://github.com/vatlab/sos-notebook/issues). 
If you find SoS Notebook useful, please support the project by starring the [SoS](https://github.com/vatlab/SoS) and [SoS Notebook](https://github.com/vatlab/sos-notebook)
github projects, or spreading the word with [twitter](https://twitter.com/ScriptOfScripts).
