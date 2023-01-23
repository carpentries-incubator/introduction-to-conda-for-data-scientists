---
title: "Working with Environments"
teaching: 60
exercises: 15
questions:
- "What is a Conda environment?"
- "How do I create (delete) an environment?"
- "How do I activate (deactivate) an environment?"
- "How do I install packages into existing environments using Conda (+pip)?"
- "How do I find out what packages have been installed in an environment?"
- "How do I find out what environments that exist on my machine?"
- "How do I delete an environment that I no longer need?"
objectives:
- "Understand how Conda environments can improve your research workflow."
- "Create a new environment."
- "Activate (deactivate) a particular environment."
- "Install packages into existing environments using Conda (+pip)."
- "List all of the existing environments on your machine."
- "List all of the installed packages within a particular environment."
- "Delete an entire environment."
keypoints:
- "A Conda environment is a directory that contains a specific collection of Conda packages that you have installed."
- "You create (remove) a new environment using the `conda create` (`conda remove`) commands."
- "You activate (deactivate) an environment using the `conda activate` (`conda deactivate`) commands."
- "You install packages into environments using `conda install`; you install packages into an active environment using `pip install`."
- "You should install each environment as a sub-directory inside its corresponding project directory"
- "Use the `conda env list` command to list existing environments and their respective locations."
- "Use the `conda list` command to list all of the packages installed in an environment."
---

> ## Workspace for Conda environments
> If you haven't done it yet, create a new `introduction-to-conda-for-data-scientists` directory on your Desktop in order to maintain a consistent workspace for all your conda environment. 
>
> On Mac OSX and Linux running following commands in the
> Terminal will create the required directory on the Desktop.
> 
> ~~~
> $ cd ~/Desktop
> $ mkdir introduction-to-conda-for-data-scientists
> $ cd introduction-to-conda-for-data-scientists
> ~~~
> {: .language-bash}
> 
> 
> For Windows users you may need to reverse the direction of the slash and run 
> the commands from the command prompt.
> 
> ~~~
> > cd ~\Desktop
> > mkdir introduction-to-conda-for-data-scientists
> > cd introduction-to-conda-for-data-scientists
> ~~~
> {: .language-bash}
>
> Alternatively, you can always "right-click" and "create new folder" on your Desktop. All the 
> commands that are run during the workshop should be run in a terminal within the 
> `introduction-to-conda-for-data-scientists` directory.
>
{: .callout}

## What is a Conda environment

A [Conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html) 
is a directory that contains a specific collection of Conda packages that you have installed. For 
example, you may be working on a research project that requires NumPy 1.18 and its dependencies, 
while another environment associated with an finished project has NumPy 1.12 (perhaps because 
version 1.12 was the most current version of NumPy at the time the project finished). If you 
change one environment, your other environments are not affected. You can easily activate or 
deactivate environments, which is how you switch between them.

> ## Avoid installing packages into your `base` Conda environment
>
> Conda has a default environment called `base` that include a Python installation and some core 
> system libraries and dependencies of Conda. It is a "best practice" to avoid installing 
> additional packages into your `base` software environment. Additional packages needed for a new 
> project should always be installed into a newly created Conda environment.
{: .callout}

## Creating environments

To create a new environment for Python development using `conda` you can use the `conda create` 
command.

~~~
$ conda create --name python3-env python
~~~
{: .language-bash}

For a list of all commands, take a look at [Conda general commands](https://docs.conda.io/projects/conda/en/latest/commands.html).

It is a good idea to give your environment a meaningful name in order to help yourself remember 
the purpose of the environment. While naming things can be difficult, `$PROJECT_NAME-env` is a 
good convention to follow. Sometimes also the specific version of a package why you had to create a new environment is a good name.

The command above will create a new Conda environment called "python3" and install the most recent 
version of Python. If you wish, you can specify a particular version of packages for `conda` to 
install when creating the environment.

~~~
$ conda create --name python36-env python=3.6
~~~
{: .language-bash}

> ## You can specify a version number for each package you wish to install
>
> In order to make your results more reproducible and to make it easier for research colleagues to 
> recreate your Conda environments on their machines it is sometimes a good practice to always explicitly 
> specify the version number for each package that you install into an environment. If you are not 
> sure exactly which version of a package you want to use, then you can use search to see what 
> versions are available using the `conda search` command. A drawback of doing this is that it can lead to conflicts between versions of packages in an environment.
>
> ~~~
> $ conda search $PACKAGE_NAME
> ~~~
> 
> So, for example, if you wanted to see which versions of 
> [Scikit-learn](https://scikit-learn.org/stable/), a popular Python library for machine learning,
> were available, you would run the following. 
>
> ~~~
> $ conda search scikit-learn
> ~~~
> 
> As always you can run `conda search --help` to learn about available options.
{: .callout}

You can create a Conda environment and install multiple packages by listing the packages 
that you wish to install.

~~~
$ conda create --name basic-scipy-env ipython=7.13 matplotlib=3.1 numpy=1.18 scipy=1.4
~~~
{: .language-bash}
 
When `conda` installs a package into an environment it also installs any required dependencies. 
For example, even though Python is not listed as a packaged to install into the 
`basic-scipy-env` environment above, `conda` will still install Python into the environment 
because it is a required dependency of at least one of the listed packages.

> ## Creating a new environment
>
> Create a new environment called "machine-learning-env" with Python and the most current versions 
> of [IPython](https://ipython.org/), [Matplotlib](https://matplotlib.org/), 
> [Pandas](https://pandas.pydata.org/), [Numba](https://numba.pydata.org/) and 
> [Scikit-Learn](https://scikit-learn.org/stable/index.html).
> 
> > ## Solution
> > 
> > In order to create a new environment you use the `conda create` command as follows.
> > 
> > ~~~
> > $ conda create --name machine-learning-env \
> >  ipython \
> >  matplotlib \
> >  pandas \
> >  python \
> >  scikit-learn \
> >  numba
> > ~~~
> > {: .language-bash}
> >
> > Since no version numbers are provided for any of the Python packages, Conda will download the 
> > most current, mutually compatible versions of the requested packages. However, since it is sometimes good 
> > practice to provide explicit version numbers, you may prefer the following solution.
> > 
> > ~~~
> > $ conda create --name machine-learning-env \
> >  ipython=7.19 \
> >  matplotlib=3.3 \
> >  pandas=1.2 \
> >  python=3.8 \
> >  scikit-learn=0.23 \
> >  numba=0.51
> > ~~~
> > {: .language-bash}
> >
> > However, please be aware that the version numbers for each packages may not be the latest available and would need to be adjusted.
> > 
> {: .solution}
{: .challenge}

## Activating an existing environment

Activating environments is essential to making the software in environments work well (or 
sometimes at all!). Activation of an environment does two things.

1. Adds entries to `PATH` for the environment.
2. Runs any activation scripts that the environment may contain.

Step 2 is particularly important as activation scripts are how packages can set arbitrary 
environment variables that may be necessary for their operation. You activate the 
`basic-scipy-env` environment by name using the `activate` command.

~~~
$ conda activate basic-scipy-env
~~~
{: .language-bash}

You can see that an environment has been activated because the shell prompt will now include the 
name of the active environment.

~~~
(basic-scipy-env) $
~~~

## Deactivate the current environment

To deactivate the currently active environment use the Conda `deactivate` command as follows.

~~~
(basic-scipy-env) $ conda deactivate
~~~
{: .language-bash}

You can see that an environment has been deactivated because the shell prompt will no longer 
include the name of the previously active environment.

~~~
$
~~~

> ## Returning to the `base` environment
>
> To return to the `base` Conda environment, it's better to call `conda activate` with no 
> environment specified, rather than to use `deactivate`. If you run `conda deactivate` from your 
> `base` environment, you may lose the ability to run `conda` commands at all. **Don't worry if 
> you encounter this undesirable state! Just start a new shell.**
{: .callout}

> ## Activate an existing environment by name
>
> Activate the `machine-learning-env` environment created in the previous challenge by name.
> 
> > ## Solution
> > 
> > In order to activate an existing environment by name you use the `conda activate` command as 
> > follows.
> > 
> > ~~~
> > $ conda activate machine-learning-env
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Deactivate the active environment
>
> Deactivate the `machine-learning-env` environment that you activated in the previous challenge.
> 
> > ## Solution
> > 
> > In order to deactivate the active environment you use the `conda deactivate` command.
> > 
> > ~~~
> > (active-environment-name) $ conda deactivate
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## Installing a package into an existing environment

You can install a package into an existing environment using the `conda install` command. This 
command accepts a list of package specifications (i.e., `numpy=1.18`) and installs a set of 
packages consistent with those specifications *and* compatible with the underlying environment. If 
full compatibility cannot be assured, an error is reported and the environment is *not* changed.

By default the `conda install` command will install packages into the current, active environment. 
The following would activate the `basic-scipy-env` we created above and install 
[`Numba`](https://numba.pydata.org/), an open source JIT compiler that translates a subset of Python 
and `numpy` code into fast machine code, into the active environment.

~~~
$ conda activate basic-scipy-env
$ conda install numba
~~~
{: .language-bash}

As was the case when listing packages to install when using the `conda create` command, if version 
numbers are not explicitly provided, Conda will attempt to install the newest versions of any 
requested packages. To accomplish this, Conda may need to update some packages that are already 
installed or install additional packages. It is sometimes a good idea to explicitly provide version 
numbers when installing packages with the `conda install` command. For example, the following would 
install a particular version of Scikit-Learn, into the current, active environment. 

~~~
$ conda install scikit-learn=0.22
~~~
{: .language-bash}

> ## Freezing installed packages
> 
> To prevent existing packages from being updating when using the `conda install` command, you can 
> use the `--freeze-installed` option. This may force Conda to install older versions of the 
> requested packages in order to maintain compatibility with previously installed packages. Using 
> the `--freeze-installed` option does not prevent additional dependency packages from being 
> installed.
{: .callout}

> ## Installing a package into a specific environment
>
> [`dask`](https://dask.org/) 
> provides advanced parallelism for data science workflows enabling performance at scale for the 
> core Python data science tools such as `numpy`, `pandas`, and `scikit-learn`. Have a read through the 
> [official documentation](https://docs.conda.io/projects/conda/en/latest/commands/install.html) 
> for the `conda install` command and see if you can figure out how to install `dask` into the 
> `machine-learning-env` that you created in the previous challenge. 
> 
> > ## Solution
> > 
> > You can install Dask into `machine-learning-env` using the `conda install` command as follow.
> > ~~~
> > $ conda install --name machine-learning-env dask=2.16
> > ~~~
> > {: .language-bash}
> > 
> > You could also install Dask into `machine-learning-env` by first activating that environment 
> > and then using the `conda install` command.
> > 
> > ~~~
> > $ conda activate machine-learning-env
> > $ conda install dask=2020.12
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge} 

## Where do Conda environments live?

Environments created with `conda`, by default, live in the `envs/` folder of your `miniconda3` (or `anaconda3`) directory the absolute path to which will look something the following: `/Users/$USERNAME/miniconda3/envs` or `C:\Users\$USERNAME\Anaconda3`.

Running `ls` (linux) / `dir` (Windows) on your anaconda `envs/` directory will list out the directories containing the existing Conda environments.

> ## Location of Conda environments on Binder
>
> If you are working through these lessons using a Binder instance, then the default location of 
> the Conda environments is slightly different.
>
> ~~~
> $ /srv/conda/envs
> ~~~
> {: .language-bash} 
> 
> Running `ls /srv/conda/envs/` from a terminal will list out the directories containing any 
> previously installed Conda environments.
{: .callout}

## Listing existing environments

Now that you have created a number of Conda environments on your local machine you have probably 
forgotten the names of all of the environments and exactly where they live. Fortunately, there is 
a `conda` command to list all of your existing environments together with their locations.

~~~
$ conda env list
~~~
{: .language-bash}

## Listing the contents of an environment

In addition to forgetting names and locations of Conda environments, at some point you will 
probably forget exactly what has been installed in a particular Conda environment. Again, there is 
a `conda` command for listing the contents on an environment. To list the contents of the 
`basic-scipy-env` that you created above, run the following command.

~~~
$ conda list --name basic-scipy-env
~~~
{: .language-bash}

If you created your Conda environment using the `--prefix` option to install packages into a 
particular directory, then you will need to use that prefix in order for `conda` to locate the 
environment on your machine.

~~~
$ conda list --prefix /path/to/conda-env
~~~
{: .language-bash}

> ## Listing the contents of a particular environment.
>
> List the packages installed in the `machine-learning-env` environment that you created in a 
> previous challenge.
> 
> > ## Solution
> > 
> > You can list the packages and their versions installed in `machine-learning-env` using the 
> > `conda list` command as follows.
> > ~~~
> > $ conda list --name machine-learning-env
> > ~~~
> > {: .language-bash}
> >
> > To list the packages and their versions installed in the active environment leave off 
> > the `--name` or `--prefix` option.
> > ~~~
> > $ conda list
> > ~~~
> > {: .language-bash}
> > 
> {: .solution}
{: .challenge}

## Deleting entire environments

Occasionally, you will want to delete an entire environment. Perhaps you were experimenting with 
`conda` commands and you created an environment you have no intention of using; perhaps you no 
longer need an existing environment and just want to get rid of cruft on your machine. Whatever 
the reason, the command to delete an environment is the following.

~~~
$ conda remove --name my-first-conda-env --all
~~~
{: .language-bash}

If you wish to delete and environment that you created with a `--prefix` option, then you will 
need to provide the prefix again when removing the environment.

~~~
$ conda remove --prefix /path/to/conda-env/ --all
~~~
{: .language-bash}

> ## Delete an entire environment
>
> Delete the entire "basic-scipy-env" environment.
> 
> > ## Solution
> > 
> > In order to delete an entire environment you use the `conda remove` command as follows.
> > 
> > ~~~
> > $ conda remove --name basic-scipy-env --all --yes
> > ~~~
> > {: .language-bash}
> >
> > This command will remove all packages from the named environment before removing the 
> > environment itself. The use of the `--yes` flag short-circuits the confirmation prompt (and 
> > should be used with caution).
> >
> {: .solution}
{: .challenge}

{% include links.md %}

