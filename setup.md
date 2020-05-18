---
layout: page
title: "Setup"
permalink: /setup/
root: ..
---

## Installation Instructions

### Check to see if Conda is already installed

If you have ever installed the [Anaconda Python distribution](https://www.anaconda.com/distribution/) 
on your local machine, then you already have Conda installed! Mac and Linux users can check 
whether Conda is installed by running the following command in a terminal.

~~~
$ which conda
/Users/$USERNAME/miniconda3/bin/conda
~~~
{: .language-bash}

If Conda has already been installed on your machine, then you this command should return the 
absolute path to the conda executable. Windows users should search for "Anaconda" to see if the 
"Anaconda Command Prompt" shows up as an option, if it does then you already have Conda installed.

### Install Python 3 version of Miniconda

If Conda has not been installed on your machine, then install the Python 3 version of 
[Miniconda](https://docs.conda.io/en/latest/miniconda.html) from Anaconda for your OS. As the name 
suggests, Miniconda is a "mini" version of the 
[Anaconda Python distribution](https://www.anaconda.com/distribution/) that includes only Conda a 
base Python 3 distribution, and any necessary OS-specific dependencies. 

For convenience here are links to the 64-bit GUI Miniconda installers for Windows and Mac OSX.

* [Windows](https://repo.anaconda.com/miniconda/Miniconda3-latest-Windows-x86_64.exe)
* [Mac OSX](https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.pkg) 

#### Linux installation

I will walk through the steps for installing on Linux systems below as installing on Linux systems 
is slightly more involved.

Download the 64-bit Python 3 install script for Miniconda.

~~~
wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
~~~
{: .language-bash}

Run the Miniconda install script. The `-b` runs the install script in batch mode which doesn't 
require manual intervention (and assumes that the user agrees to the terms of the license).

~~~
bash Miniconda3-latest-Linux-x86_64.sh -b
~~~
{: .language-bash}

Once the install script completes, you can remove it.

~~~
rm Miniconda3-latest-Linux-x86_64
~~~
{: .language-bash}

### Make sure you have the most recent version

Once Conda exists on your machine, then run the following command to make sure that you 
have the most recent version and patches.

~~~
$ conda update -y conda
~~~
{: .language-bash}

You can re-run this command at any time to update to the most recent version of Conda.

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
> > version of Conda needs to be updated. If you already have the most up to date version, then the 
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
> > Once the update process is complete you can check the version number as follows.
> > ~~~
> > $ conda --version
> > conda 4.6.14
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

## Workspace Setup

In order to maintain a consistent workspace for all learners we will create an 
`introduction-to-conda-for-data-scientists` directory on the Desktop to serve as our working 
directory for the duration of the lesson. On Mac OS and Linux running following commands in the
Terminal will create the required directory on the Desktop.

~~~
$ cd ~/Desktop
$ mkdir introduction-to-conda-for-data-scientists
$ cd introduction-to-conda-for-data-scientists
~~~
{: .language-bash}


For Windows users you will need to reverse the direction of the slash (because Windows) and run 
the commands from the command prompt.

~~~
> cd ~\Desktop
> mkdir introduction-to-conda-for-data-scientists
> cd introduction-to-conda-for-data-scientists
~~~

