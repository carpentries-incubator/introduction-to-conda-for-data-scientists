---
title: "Working with Environments"
teaching: 30
exercises: 10
questions:
- "What is a Conda environment?"
objectives:
- "Create a new environment from a file."
- "Activate a particular environment."
- "Deactivate the current environment."
- "Delete an entire environment."
keypoints:
- "A Conda environment is a directory that contains a specific collection of Conda packages that you have installed."
---

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

> ## Activate an existing environment by name
>
> Activate the "my-first-env" environment created in the previous challenge.
> 
> > ## Solution
> > 
> > In order to activate an existing environment by name you use the `conda activate` command as follows.
> > 
> > ~~~
> > $ conda activate my-first-env
> > ~~~
> > {: .language-bash}
> >
> >
> {: .solution}
{: .challenge}

> ## Deactivate the active environment
>
> Deactivate the active Conda environment.
> 
> > ## Solution
> > 
> > In order to deactivate the active environment you use the `conda deactivate` command.
> > 
> > ~~~
> > $ conda deactivate
> > ~~~
> > {: .language-bash}
> >
> >
> {: .solution}
{: .challenge}

> ## Activate an existing environment by path
>
> Activate the "my-second-env" environment created in a previous challenge using the path to the 
> environment directory.
> 
> > ## Solution
> > 
> > You can activate an existing environment by providing the path the the environment directory 
> > instead of the environment name when using the `conda activate` command as follows.
> > 
> > ~~~
> > $ conda activate ./env
> > ~~~
> > {: .language-bash}
> >
> > Note that the provided path can either be absolute or relative. If the path is a relative path 
> > then it must start with `./` on Unix systems and `.\` on Windows.
> {: .solution}
{: .challenge}


> ## Listing the contents of a particular environment.
>
> List the packages installed in the `my-first-env` environment. List the revisions made to the 
> `my-second-env` environment.
> 
> > ## Solution
> > 
> > You can list the packages and their versions installed in `my-first-env` using the 
> > `conda list` command as follows.
> > ~~~
> > $ conda list --name my-first-env
> > ~~~
> > {: .language-bash}
> >
> > To list the packages and their versions installed in the active environment simply leave off 
> > the `--name` option.
> > ~~~
> > $ conda list
> > ~~~
> > {: .language-bash}
> > 
> > To list all of the revisions made to `my-second-env` pass the `--revisions` flag to the 
> > `conda list` command.
> > ~~~
> > $ conda list --revisions --name my-second-env
> > ~~~
> > {: .language-bash}
> > 
> > To list all of the revisions made to the active environment simply leave off the `--name` option.
> > ~~~
> > $ conda list --revisions
> > ~~~
> > {: .language-bash}
> > 
> {: .solution}
{: .challenge}

> ## Delete an entire environment
>
> Delete the entire "my-first-env" environment created in a previous challenge.
> 
> > ## Solution
> > 
> > In order to delete an entire environment you use the `conda remove` command as follows.
> > 
> > ~~~
> > $ conda remove --name my-first-env --all
> > ~~~
> > {: .language-bash}
> >
> > This command will remove all packages from the named environment before removing the 
> > environment itself.
> >
> {: .solution}
{: .challenge}
{% include links.md %}

