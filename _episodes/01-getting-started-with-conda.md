---
title: "Getting Started with Conda"
teaching: 15
exercises: 5
questions:
- "Why should I use a package and environment management system as part of my research workflow?"
- "Why Conda and not pip+virtualenv or Pipenv?"
- "What is Conda?"
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

Package management...

An environment managment system solve a number of problems commonly encountered by (data) scientists.

* An application you need for a research project requires different versions of Python or different versions of various Python packages from the versions that you are currently using.
* An application you developed as part of a previous research project worked fine on your system when you submitted the project for review but now no longer works on your current system.
* You need to make sure that the code for a joint research project works on your collaborator's computer.
* You need to make sure that a machine learning application that you are developing on your local machine works properly on your remote cluster.

As we will see an environment management system that enables you to set up a new environment 
containing the Python version and the versions of packages that are completely compatible for each 
research project is a good solution to these problems.

## Why Conda and not pip+virtualenv or Pipenv?

### Conda and pip can be used together

### Performance

## What is Conda?

From the [official Conda documentation](https://conda.io/projects/conda/en/latest/index.html). 
Conda is an open source package and environment management system that runs on Windows, macOS and 
Linux. 
* Conda can quickly install, run, and update packages and their dependencies.
* Conda can create, save, load, and switch between project specific software environments on your local computer. 
* Although Conda was created for Python programs, Conda can package and distribute software for any language such as R, Ruby, Lua, Scala, Java, JavaScript, C/ C++, FORTRAN.

Conda as a *package manager* helps you find and install packages. If you need a package that requires a different version of Python, you do not need to switch to a different environment manager, because Conda is also an *environment manager*. With just a few commands, you can set up a totally separate environment to run that different version of Python, while continuing to run your usual version of Python in your normal environment. 

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

