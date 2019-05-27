---
title: "Using Packages and Channels"
teaching: 15
exercises: 15
questions:
- "What are Conda packages?"
- "What are Conda channels?"
- "Why should I be explicit about which channels my research project uses?"
objectives:
- "Search for a package on currently configured channels."
- "Install a package from a specific channel."
- "Install a package by exact version number."
- "Install a package where version number satisfies certain constraints."
- "Add channels to your Conda configuration."
keypoints:
- "???"
- "A Conda channel is a URL to a directory containing a Conda package(s)."
- "Explicitly including the channels (and their priority!) in a project's environment file is necessary for another research to completely re-create that project's software environment." 
---

## What are Conda packages?

From the 
[Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/concepts/packages.html#what-is-a-conda-package)...

A conda package is a compressed tarball file (.tar.bz2) that contains:

* system-level libraries
* Python or other modules
* executable programs and other components
* metadata under the `info/` directory
* a collection of files that are installed directly into an `install` prefix

## What are Conda channels?
Again from the [Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/concepts/channels.html#what-is-a-conda-channel)

> ## Installing packages whose versions satisfy `OR` or `AND` conditions
>
> Using `conda` it is possible to install a package whose version number satifies certain boolean 
> conditions.  For example, suppose you wish to install one of the listed versions of some package.
> ~~~
> $ conda install "PKGNAME[version='3.1.2|3.1.4']" 
> ~~~
> {: .language-bash}
>
{: .callout}

> ## Creating a new environment
>
> Create a new environment called "my-first-env" with Python 3.6, and the most current versions of 
> [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/), 
> [Matplotlib](https://matplotlib.org/), [Pandas](https://pandas.pydata.org/), and 
> [Scikit-Learn](https://scikit-learn.org/stable/index.html).
> 
> > ## Solution
> > 
> > In order to create a new environment you use the `conda create` command as follows.
> > 
> > ~~~
> > $ conda create --name my-first-env \
> > > python=3.6 \
> > > jupyterlab \
> > > matplotlib \
> > > pandas \
> > > scikit-learn
> > ~~~
> > {: .language-bash}
> >
> > Since no version numbers are provided for any of the Python packages, Conda will download the 
> > most current, mutually compatible versions of the requested packages.
> >
> {: .solution}
{: .challenge}

{% include links.md %}

