---
title: "Working with Environments"
teaching: 60
exercises: 15
questions:
- "What is a Conda environment?"
- "How do I create (delete) an environment?"
- "How do I activate (deactivate) an environment?"
- "How do I install packages into existing environments using Conda?"
- "How do I find out what packages have been installed in an environment?"
- "How do I find out what environments that exist on my machine?"
- "How do I delete an environment that I no longer need?"
objectives:
- "Understand how Conda environments can improve your research workflow."
- "Create a new environment."
- "Activate (deactivate) a particular environment."
- "Install packages into existing environments using Conda."
- "List all of the existing environments on your machine."
- "List all of the installed packages within a particular environment."
- "Delete an entire environment."
keypoints:
- "A Conda environment is a directory that contains a specific collection of Conda packages that you have installed."
- "You create (remove) a new environment using the `conda create` (`conda remove`) commands."
- "You activate (deactivate) an environment using the `conda activate` (`conda deactivate`) commands."
- "You install packages into environments `conda install`."
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
>
> Alternatively, you can always "right-click" and "create new folder" on your Desktop. All the
> commands that are run during the workshop should be run in a terminal within the
> `introduction-to-conda-for-data-scientists` directory.
>
{: .callout}

## What is a Conda environment

A [Conda environment](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/environments.html)
is a directory that contains a specific collection of Conda packages that you have installed. For
example, you may be working on a research project that requires [NumPy
1.24.1](https://github.com/numpy/numpy/releases/tag/v1.24.1) and its dependencies, while another environment associated
with an finished project has [NumPy 1.12.1](https://github.com/numpy/numpy/releases/tag/v1.12.1) (perhaps because
version 1.12 was the most current version of NumPy at the time the project finished). If you change one environment,
your other environments are not affected. You can easily activate or deactivate environments, which is how you switch
between them.

> ## Avoid installing packages into your `base` Conda environment
>
> Conda has a default environment called `base` that includes a Python installation and some core
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
good convention to follow. Sometimes also the specific version of a package why you had to create a new environment is a
good name.

The command above will create a new Conda environment called `python3-env` and install a recent
version of Python (3.10). If you wish, you can specify a specific version of Python for `conda` to
install when creating the environment by changing the version number in `python=3.10` e.g. to `python=3.7`.

~~~
$ conda create --name python310-env python=3.10
~~~
{: .language-bash}

> ## You can specify a version number for each package you wish to install
>
> In order to make your results more reproducible and to make it easier for research colleagues to
> recreate your Conda environments on their machines it is sometimes a good practice to explicitly
> specify the version number for each package that you install into an environment.
>
> Many packages use [semantic versioning](https://semver.org/)
> where there are three version numbers separated by decimal points e.g. 2.11.3. In this scheme
> the numbers have this meaning: _major_version_._minor_version_._patch_version_. Changes to
> _patch_version_ are for backwards compatible bug fixes, so we often only specify the first two
> numbers.
>
> If you are not
> sure exactly which version of a package you want to use, then you can use search to see what
> versions are available using the `conda search` command.
>
> ~~~
> $ conda search $PACKAGE_NAME
> ~~~
>
> For example, if you wanted to see which versions of [Scikit-learn](https://scikit-learn.org/stable/),
> a popular Python library for machine learning, were available, you would run the following.
>
> ~~~
> $ conda search scikit-learn
> ~~~
>
> As always you can run `conda search --help` to learn about available options.
{: .callout}

> ## Discussion
>
> What are some of the _potential_ benefits of specifying versions of each package, what
> are some of the _potential_ drawbacks.
>
> > ## Solution
> > Specifying versions exactly helps make it more likely that the exact results of an analysis
> > will be reproducible e.g. at a later time or on a different computer. However, not all versions
> > of a package will be compatible with all versions of another, so specifying exact versions can
> > make it harder to add or change packages in the future, limiting reusability e.g. with different
> > data.
> >
> {: .solution}
{: .challenge}

You can create a Conda environment and install multiple packages by listing the packages that you wish to install,
optionally including the version you wish to use.

~~~
$ conda create --name basic-scipy-env ipython matplotlib=3.1 numpy=1.23.5 scipy=1.9.3
~~~
{: .language-bash}

When `conda` installs a package into an environment it also installs any required dependencies. For example, even though
Python is not listed as a package to install into the `basic-scipy-env` environment above, `conda` will still install
Python into the environment because it is a required dependency of at least one of the listed packages. Note if you
wanted a specific version of Python installed you would have to explicitly state which version as you did earlier.

This command will fail as the requested `scipy` and `numpy` versions are incompatible.

~~~
$ conda create --name basic-scipy-env ipython matplotlib=3.1 numpy=1.9.3 scipy=1.9.3
~~~
{: .language-bash}

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
> > $ conda create --name machine-learning-env ipython matplotlib pandas python scikit-learn numba
> > ~~~
> > {: .language-bash}
> >
> > Since no version numbers are provided for any of the Python packages, Conda will download the
> > most current, mutually compatible versions of the requested packages. However, since it is sometimes good
> > practice to provide explicit version numbers, you may prefer the following solution.
> >
> > ~~~
> > $ conda create --name machine-learning-env ipython=8.8.0 matplotlib=3.6.2 pandas=1.5.2 python=3.10
> > scikit-learn=1.2.0 numba=0.56.4
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

1. Adds entries to `$PATH` for the environment.
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
include the name of the previously active environment, but will return to `base`.

~~~
(base) $
~~~

> ## Returning to the `base` environment
>
> To return to the `base` Conda environment, it's better to call `conda activate` with no
> environment specified, rather than to use `deactivate`. If you run `conda deactivate` from your
> `base` environment, you may lose the ability to run `conda` commands at all. **Don't worry if
> you encounter this undesirable state! Just start a new shell or `source ~/.bashrc` if you are on
> a Linux or OSX system.**
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
> > In order to deactivate the active environment you could use the `conda deactivate` command.
> >
> > ~~~
> > (active-environment-name) $ conda deactivate
> > ~~~
> > {: .language-bash}
> >
> > Or you could switch back to the `base` conda environment this way.
> > ~~~
> > (active-environment-name) $ conda activate
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## Installing a package into an existing environment

You can install a package into an existing environment using the `conda install` command. This
command accepts a list of package specifications (i.e., `numpy=1.18`) and installs a set of
packages consistent with those specifications *and* compatible with the underlying environment. If
full compatibility cannot be assured, an error is reported and the environment is *not* changed.

By default the `conda install` command will install packages into the current, active environment. The following would
activate the `basic-scipy-env` we created above and install [`Numba`](https://numba.pydata.org/), an open source JIT
(Just In Time) compiler that translates a subset of Python and `numpy` code into fast machine code, into the active
environment.

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
$ conda install scikit-learn=1.2.0
~~~
{: .language-bash}

> ## Freezing installed packages
>
> To prevent existing packages from being updated when using the `conda install` command, you can
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
> > $ conda install --name machine-learning-env dask=2022.7.0
> > ~~~
> > {: .language-bash}
> >
> > You could also install Dask into `machine-learning-env` by first activating that environment
> > and then using the `conda install` command.
> >
> > ~~~
> > $ conda activate machine-learning-env
> > $ conda install dask=2022.7.0
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## Where do Conda environments live?

Environments created with `conda`, by default, live in the `envs/` folder of your `miniconda3` (or `anaconda3`)
directory the absolute path to which will look something the following: `/home/$USERNAME/miniconda3/envs` (Linux),
`/Users/$USERNAME/miniconda3/envs` (OSX) or `C:\Users\$USERNAME\Anaconda3` (Windows). It's also been spotted in
`C:\ProgramData\miniconda3\envs`.

Running `ls` (Linux or OSX) / `dir` (Windows) on your anaconda `envs/` directory will list out the directories
containing the existing Conda environments.

> ## Finding Conda
>
> If you're not sure where your Conda environments are stored, activate an environment, launch Python, then run
> the following:
> ```
> import sys                   
> sys.executable
> ~~~
> {: .language-python}
>
> This will tell you where the python executable for the environment is stored e.g. `/home/neil/miniconda3/bin/python`.
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
$ conda remove --name first-conda-env --all
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
