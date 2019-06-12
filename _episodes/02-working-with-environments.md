---
title: "Working with Environments"
teaching: 45
exercises: 15
questions:
- "What is a Conda environment?"
- "How do I create (delete) an environment?"
- "How do I activate (deactivate) an environment?"
- "Where should I install my conda environment?"
- "How do I find out the environments that exist on my machine?"
- "How do I find out what packages have been installed in an environment?"
- "How do I delete an environment that I no longer need?"
objectives:
- "Understand how Conda environments can improve your research workflow."
- "Create a new environment."
- "Activate (deactivate) a particular environment."
- "Specify the installation location of an environment."
- "List all of the existing environments on your machine."
- "List all of the installed packages within a particular environment."
- "Delete an entire environment."
keypoints:
- "A Conda environment is a directory that contains a specific collection of Conda packages that you have installed."
- "You create (remove) a new environment using the `conda create` (`conda remove`) commands."
- "You activate (deactivate) an environment using the `conda activate` (`conda deactivate`) commands."
- "You should install each environment as a sub-directory inside its corresponding project directory"
- "Use the `conda env list` command to list existing environments and their respective locations."
- "Use the `conda list` command to list all of the packages installed in an environment."
---
## What is a Conda environment

A [Conda environment][conda-env-docs] is a directory that contains a specific collection of Conda 
packages that you have installed. For example, you may be working on a research project that 
requires NumPy 1.16 and its dependencies, while another environment associated with an finished 
project has NumPy 1.12 (perhaps because version 1.12 was the most current version of NumPy at the 
time the project finished). If you change one environment, your other environments are not 
affected. You can easily activate or deactivate environments, which is how you switch between them.

## Creating environments

To create a new environment for Python development using `conda` you should use the `create` 
command as follows.

~~~
$ conda create --name my-first-conda-env python
~~~
{: .language-bash}

The command above will create a new Conda environment called "my-first-conda-env" and install 
Python. Since no particular version of Python was specified, the version of Python installed by 
`conda` will be the version used by your current shell's Python interpreter. If you wish, you may 
specify a specific version of Python for `conda` to install as follows.

~~~
$ conda create --name my-first-conda-env python=3.6
~~~
{: .language-bash}

> ## Always specify a version number for each package you wish to install
>
> In order to make your results more reproducible and to make it easier for research colleagues to 
> recreate your Conda environments on their machines it is a "best practice" to always explicitly 
> specify the version number for each package that you install into an environment.  
{: .callout}

You can create a Conda environment and install multiple packages by simply listing the packages that you wish to install.

~~~
$ conda create --name my-second-conda-env jupyterlab matplotlib numpy
~~~
{: .language-bash}
 
When `conda` installs a package into an environment it also installs any required dependencies. 
For example, even though Python is not listed as a packaged to install into the 
`my-second-conda-env` environment above, `conda` will still install Python into the environment 
because it is a required dependency of at least one of the listed packages.

> ## Creating a new environment
>
> Create a new environment called "conda-env" with Python and the most current versions of 
> [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/), 
> [Matplotlib](https://matplotlib.org/), [Pandas](https://pandas.pydata.org/), and 
> [Scikit-Learn](https://scikit-learn.org/stable/index.html).
> 
> > ## Solution
> > 
> > In order to create a new environment you use the `conda create` command as follows.
> > 
> > ~~~
> > $ conda create --name conda-env \
> > > python \
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

> ## Creating a new environment with explicit version numbers
>
> Create a new environment called "explicit-conda-env" with Python 3.6, version 0.35.5 of 
> JupyterLab, version 3.0.3 of Matplotlib, version 0.24.2 of Pandas, and version 0.21.1 of 
> Scikit-Learn.
> 
> > ## Solution
> > 
> > ~~~
> > $ conda create --name explicit-conda-env \
> > > python=3.6 \
> > > jupyterlab=0.35.5 \
> > > matplotlib=3.0.3 \
> > > pandas=0.24.2 \
> > > scikit-learn=0.21.1
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

> ## Install *all* desired packages into your environment at the same time
>
> Conda insures that there are no conflicts between versions of packages and their required 
> dependencies at install time. Installing *all* the packages that you want to include in a 
> particular `conda` environment at the same time eliminates hard to debug depedency conflicts. 
{: .callout}

## Activating an existing environment

Activating environments is essential to making the software in environments work well (or 
sometimes at all!). Activation of an environment does two things.

1. Adds entries to `PATH` for the environment.
2. Runs any activation scripts that the environment may contain.

Step 2 is particularly important as activation scripts are how packages can set arbitrary 
environment variables that may be necessary for their operation.

To activate the `my-second-conda-env` environment by name use the `activate` command as follows.

~~~
$ conda activate my-second-conda-env
~~~
{: .language-bash}

You can see that an environment has been activated because the shell prompt will now include the 
name of the active environment.

~~~
(my-second-conda-env) $
~~~

> ## Activate an existing environment by name
>
> Activate the "explicit-conda-env" environment created in the previous challenge by name.
> 
> > ## Solution
> > 
> > In order to activate an existing environment by name you use the `conda activate` command as follows.
> > 
> > ~~~
> > $ conda activate explicit-conda-env
> > ~~~
> > {: .language-bash}
> >
> >
> {: .solution}
{: .challenge}

## Deactivate the current environment

To deactivate the currently active environment use the `deactivate` command as follows.

~~~
(my-second-conda-env) $ conda deactivate
~~~
{: .language-bash}

You can see that an environment has been deactivated because the shell prompt will no longer 
include the name of the active environment.

~~~
$
~~~

> ## Returning to the `base` environment
>
> To simply return to the `base` Conda environment, it's better to call `conda activate` with no 
> environment specified, rather than to use `deactivate`. If you run `conda deactivate` from your 
> `base` environment, you may lose the ability to run `conda` commands at all. **Don't worry if 
> you encounter this undesireable state! Just start a new shell.**
{: .callout}

> ## Deactivate the active environment
>
> Deactivate the currently active Conda environment.
> 
> > ## Solution
> > 
> > In order to deactivate the active environment you use the `conda deactivate` command.
> > 
> > ~~~
> > (active-environment-name) $ conda deactivate
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

## Where do Conda environments live?

Environments created with `conda`, by default, live in the `envs/` folder of your `miniconda3` 
directory the absolute path to which will look something the following.

~~~
$ /Users/$USERNAME/miniconda3/envs
~~~
{: .language-bash} 

Running `ls` on your `~/miniconda3/envs/` directory will list out the directories containing the 
existing Conda environments.

## How do I specify a location for a Conda environment?

You can control where a Conda environment lives by providing a path to a target directory when 
creating the environment. For example to following command will create  a new environment in a 
sub-directory of the current working directory called `my-local-env`.

~~~
$ conda create --prefix ./my-local-env jupyterlab matplotlib numpy
~~~
{: .language-bash}

Note that the name of the environment that is created using this command is `my-local-env`. You 
can it activate the environment as follows.

~~~
$ conda activate my-local-env
~~~
{: .language-bash}

It is a good idea to *always* specify a path to a sub-directory of your project directory when 
creating a `conda` environment. Why?

1.  Makes it easy to tell if your project utilizes an isolated environment by including the 
    environment as a sub-directory.
2.  Makes your project more self-contained as everything *including the required software* is 
    contained in a single project directory.

An additional benefit of creating your project's `conda` environment inside a sub-directory is 
that you can then use the same name for all your `conda` environments; if you keep all of your 
environments in your `~/miniconda3/env/` folder, you’ll have to give each of them a different name.

> ## Conda environment sub-directory naming convention
> 
> In order to be consistent with the convention used by tools such as `venv` and `Pipenv`, I 
> recommend using `env` as the name of the sub-directory of your project directory that contains 
> your Conda environment. A benefit of maintaining the convention is that your environment 
> sub-directory will be automatically ignored by the default Python `.gitignore` file used on 
> [GitHub](https://github.com/github/gitignore/blob/master/Python.gitignore). 
>
> Whatever naming convention you adopt be consistent! Using the same name for all of your Conda 
> environments allows you to use the same `activate` command as well.
> 
> ~~~ 
> $ cd my-project/
> $ conda activate ./env
> ~~~
> {: .language-bash}
{: .callout}

> ## Creating a new environment as a sub-directory within a project directory 
>
> First create a project directory called `project-dir` using the following command.
> 
> ~~~
> $ mkdir project-dir
> $ cd project-dir
> ~~~
> {: .language-bash}
>
> Next, create a new environment inside the newly created `project-dir` in a sub-directory called 
> `env` an install Python 3.6, version 0.35.5 of JupyterLab, version 3.0.3 of Matplotlib, version 
> 0.24.2 of Pandas, and version 0.21.1 of Scikit-Learn.
> 
> > ## Solution
> > 
> > ~~~
> > project-dir $ conda create --prefix ./env \
> > > python=3.6 \
> > > jupyterlab=0.35.5 \
> > > matplotlib=3.0.3 \
> > > pandas=0.24.2 \
> > > scikit-learn=0.21.1
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

Placing Conda environments outside of the default `~/miniconda3/envs/` folder comes with a couple 
of minor drawbacks. First, `conda` can no longer find your environment with the `--name` flag; 
you’ll generally need to pass the `--prefix` flag along with the environment’s full path to 
find the environment.

Second, an annoying side-effect of specifying an install path when creating your Conda 
environments is that your command prompt is now prefixed with the active environment’s absolute 
path rather than the environment's name. After activating an environment using its prefix your 
prompt will look similar to the following.

~~~
(/absolute/path/to/env) $
~~~

As you can imagine, this can quickly get out of hand.

~~~
(/Users/USER_NAME/research/data-science/PROJECT_NAME/env) $
~~~

If (like me!) you find this long prefix to your shell prompt annoying, then there is a quick fix: 
modify the `env_prompt` setting in your `.condarc` file, which you can do with the following 
command.

~~~
$ conda config --set env_prompt '({name})'
~~~
{: .language-bash}
 
This will either edit your `.condarc` file if you already have one or create a `condarc` file if 
you do not. Now your command prompt will display the active environment’s generic name.
 
~~~
$ cd project-directory
$ conda activate ./env
(env) project-directory $
~~~
{: .language-bash}

For more on modifying your `.condarc` file, see [the docs][conda-docs].

> ## Activate an existing environment by path
>
> Activate the environment created in a previous challenge using the path to the 
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
`my-first-environment` that you created above, run the following command.

~~~
$ conda list --name my-first-environment
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
> List the packages installed in the `my-first-env` environment.
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
the reason the command to delete an environment is the following.

~~~
$ conda remove --name my-first-env --all
~~~
{: .language-bash}

If you wish to delete and environment that you created with a `--prefix` option, then you will 
need to provide the prefix again when removing the environment.

~~~
$ conda remove --prefix /path/to/conada-env/ --all
~~~
{: .language-bash}

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

