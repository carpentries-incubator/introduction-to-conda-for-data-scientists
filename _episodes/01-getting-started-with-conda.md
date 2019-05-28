---
title: "Getting Started with Conda"
teaching: 15
exercises: 5
questions:
- "Why should I use a package and environment management system as part of my research workflow?"
- "What is Conda?"
- "Why Conda and not pip+virtualenv or Pipenv?"
objectives:
- "Explain why you should use Conda as part of your research workflow?"
- "Install Conda for your OS."
- "Verify your Conda installation."
- "Update the base Conda installation to the most recent version."
keypoints:
- "Conda is a platform agnostic, open source package and environment management system."
- "Using a package and enviroment management tool, such as Conda, facilitates reproducibility of research workflows."
---

## Why should I use a package and environment managment system?

Installing software is hard. Installing scientific software (including all required dependencies of said software!) is often even more challenging. A good package management system greatly simplifies the process of installing software by automatically identifying and installing compatible versions of desired software and all required dependencies. Most package managers will also handle the process of updating software as more recent versions become available.

If you use some flavor of Linux, then you are probably familiar with the package manager for your Linux distribution (i.e., `apt` on Ubuntu, `yum` on CentOS); if you are a Mac OS user then you might be familiar with the [Home Brew Project](https://brew.sh/) which brings a Linux-like package management system to Mac OS X; if you are a Windows OS user, then you may not be terribly familiar with package managers as there isn't really a standard package manager for Windows (although there is the [Chocolatey Project](https://chocolatey.org/)).  These tools are great but actually solve a more general problem than we face as (data) scientists.

The Python and R communities are awash with third-party packages that provide additional functionality not found in the standard libraries of these programming languages. However the ever-growing number of third party packages places a great straing on users: installing these these third-party packages and required dependencies is hard. 

The R community has standardized The `pip` package is probably the most widely used package management tool in Python.

Package management...

An environment managment system solves a number of problems commonly encountered by (data) 
scientists.

*   An application you need for a research project requires different versions of Python (R) or 
    different versions of various Python (R) packages from the versions that you are currently using.
*   An application you developed as part of a previous research project that worked fine on your 
    system six months ago now no longer works.
*   Code that have written for a joint research project works on your machine but not on your 
    collaborators' machines.
*   A machine learning application that you are developing on your local machine doesn't provide 
    the same results when run on your remote cluster.

As we will see, an environment management system that enables you to set up a new, project specific 
environment containing specific Python (R) versions as well as the versions of packages and 
required dependencies that are all mutually compatible is a good solution to these problems.

*   Environment management systems help resolve dependency issues by allowing you to use different 
    versions of a package for different projects.
*   Make your projects self-contained and reproducible by capturing all package dependencies in a 
    single requirements file.
*   Often allow you to install packages on a host on which you do not have admin privileges.

A virtual environment is a Python tool for dependency management and project isolation. They allow Python site packages (third party libraries) to be installed locally in an isolated directory for a particular project, as opposed to being installed globally (i.e. as part of a system-wide Python).

[guide to Python virtual environments](https://towardsdatascience.com/virtual-environments-104c62d48c54)

Conda environments are like cousins of Python’s virtual environments. Both serve to help manage dependencies and isolate projects, and they function in a similar way, with one key distinction: conda environments are language agnostic. That is, they support languages other than Python.

## What is Conda?

From the [official Conda documentation](https://conda.io/projects/conda/en/latest/index.html). 
Conda is an open source package and environment management system that runs on Windows, Mac OS and 
Linux.

*   Conda can quickly install, run, and update packages and their dependencies.
*   Conda can create, save, load, and switch between project specific software environments on 
    your local computer. 
*   Although Conda was created for Python programs, Conda can package and distribute software for 
    any language such as R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN.

Conda as a *package manager* helps you find and install packages. If you need a package that 
requires a different version of Python, you do not need to switch to a different environment 
manager, because Conda is also an *environment manager*. With just a few commands, you can set up 
a totally separate environment to run that different version of Python, while continuing to run 
your usual version of Python in your normal environment. 

## Why Conda and not pip + virtualenv or Pipenv?

Conda solves both *package management* and *environment management* problems discussed above and 
Conda is language agnostic (although Conda does targets users in the Python and R (data) science 
communities). Other open source tools solve either package management or environment management 
problems, or target only Python or R users. 

* [`pip`](https://pip.pypa.io/en/stable/) is a package manager for Python only.
* [`venv`](https://docs.python.org/3/library/venv.html) is an environment manager for Python only.
* [`Pipenv`](https://docs.pipenv.org/en/latest/) is a package and environment manager for Python packages only and targets general Python community.
* [`Packrat`](https://rstudio.github.io/packrat/) is a pakcage and environment mangager for R packages only.

### Conda and pip can be used together

Conda and pip can also be used together: one can use `conda` to install a specific verion of `pip` and then use that version of `pip` to install third-party Python packages not directly available via `conda`. You will see some examples of how to do this in future episodes.

### Performance

When installed using `conda`, most of the core (data) science Python packages, inlcuding NumPy, SciPy, Scikit-learn, Tensorflow, et al, are compiled and linked against Intel's Math Kernel Libraries (MKL) which means that these core libraries will be significanly more performant than the exact same version of those libraries that are installed via `pip` and compiled and linked against against less performant core math libraries.  For detailed benchmarks of the performance of TensorFlow installed vis `conda` vs TensorFlow installed via `pip` see this excellent [blog post](https://towardsdatascience.com/stop-installing-tensorflow-using-pip-for-performance-sake-5854f9d9eb0c) on *Medium*.

## Installing Conda

First check whether Conda has already been  installed on your local machine by running the 
following command in a terminal if you are running macOS or Linux.

~~~
$ which conda
~~~
{: .language-bash}

If Conda already exists on your machine, then run the following commands to make sure that you 
have the most recent version and patches.

~~~
$ conda update -y conda
$ conda init
~~~
{: .language-bash}

**After running `conda init` you will need to close and restart your shell for changes to take 
effect.**

If Conda has not been installed on your machine, then install the Python 3 version of 
[Miniconda](https://docs.conda.io/en/latest/miniconda.html) from Anaconda for your OS. Miniconda 
is mini version of the [Anaconda Python distribution](https://www.anaconda.com/distribution/) 
that includes only Conda and its dependencies. Once you have installed Conda on your machine run 
the following commands in make sure that you have the most recent version and patches.

~~~
$ conda update -y conda
$ conda init
~~~
{: .language-bash}

**Again, after running `conda init` you will need to close and restart your shell for changes to take 
effect.**

> ## Initializing Conda
>
> Conda 4.4 introduced new scripts that make activation behavior uniform across operating systems. 
> Where previously you once had `source activate envname` on unix, and just `activate envname` on 
> windows, Conda 4.4 allowed `conda activate envname`.  Setting up your shell to use this new 
> feature was tricky. Conda 4.6 added extensive initialization support so that more shells can use 
> the new `conda activate` command. For more information, read the output from `conda init –-help`. 
{: .callout}

> ## Verifying your Conda installation
>
> Confirm that you have installed Conda correctly on your machine. What is the version number?
> 
> > ## Solution
> > 
> > In order to verify that you have installed Conda correctly run the `conda info` command. Output 
> > of the command should look similar to the following. Note that the conda version number is 
> > provided as part of the output of this command. 
> > 
> >  ~~~
> >  $ conda info
> > 
> >     active environment : base
> >     active env location : /Users/<USERNAME>/miniconda3
> >             shell level : 1
> >        user config file : /Users/<USERNAME>/.condarc
> >  populated config files : 
> >           conda version : 4.6.14
> >     conda-build version : not installed
> >          python version : 3.7.1.final.0
> >        base environment : /Users/<USERNAME>/miniconda3  (writable)
> >            channel URLs : https://repo.anaconda.com/pkgs/main/osx-64
> >                           https://repo.anaconda.com/pkgs/main/noarch
> >                           https://repo.anaconda.com/pkgs/free/osx-64
> >                           https://repo.anaconda.com/pkgs/free/noarch
> >                           https://repo.anaconda.com/pkgs/r/osx-64
> >                           https://repo.anaconda.com/pkgs/r/noarch
> >           package cache : /Users/<USERNAME>/miniconda3/pkgs
> >                           /Users/<USERNAME>/.conda/pkgs
> >        envs directories : /Users/<USERNAME>/miniconda3/envs
> >                           /Users/<USERNAME>/.conda/envs
> >                platform : osx-64
> >              user-agent : conda/4.6.14 requests/2.19.1 CPython/3.7.1 Darwin/18.6.0 OSX/10.14.5
> >                 UID:GID : 1682265470:1840429327
> >              netrc file : None
> >            offline mode : False
> >  ~~~
> > {: .language-bash}
> >
> > An alternative solution is to run the `conda` command and pass the `--version` flag which will 
> > return the version number if Conda has been properly installed.
> > 
> > ~~~
> > $ conda --version
> > conda 4.6.14
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

> ## Update the base Conda installation to the most recent version
>
> Update the base Conda installation to the most recent version. What is the version number?
> 
> > ## Solution
> > 
> > In order to verify that you have installed Conda correctly run the `conda update -y conda` 
> > command. The output of the command will differ depending on whether or not your current 
> > version of Conda needs to be updated. If you already have the most upto date version, then the 
> > output of the command should looks as follows.
> > 
> > ~~~
> > $ conda update -y conda
> > Collecting package metadata: done
> > Solving environment: done
> > 
> > # All requested packages already installed.
> > ~~~
> > {: .language-bash}
> >
> > If your current version needed to be updated, then the output of the command should look 
> > similar to the following.
> > 
> > ~~~
> > $ conda update -n base conda
> > ???
> > ~~~
> > 
> > Once the update process is complete you can check the version number as follows.
> > ~~~
> > $ conda --version
> > conda 4.6.14
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

{% include links.md %}

