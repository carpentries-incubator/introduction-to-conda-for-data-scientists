---
title: "Using Packages and Channels"
teaching: 20
exercises: 10
questions:
- "What are Conda channels?"
- "What are Conda packages?"
- "Why should I be explicit about which channels my research project uses?"
- "What should I do if a Python package isn't available via a Conda channel?"
objectives:
- "Install a package from a specific channel."
keypoints:
- "A package is a compressed archive file containing system-level libraries, Python or other modules, executable programs and other components, and associated metadata."
- "A Conda channel is a URL to a directory containing a Conda package(s)."
- "Explicitly including the channels (and their priority!) in a project's environment file is necessary for another researcher to completely re-create that project's software environment."
- "Understand how to use Conda and Pip together effectively."
---

## What are Conda packages?

A conda package is a compressed archive file (`.tar.bz2` or `.conda`) that contains:

* system-level libraries
* Python or other modules
* executable programs and other components
* metadata under the `info/` directory
* a collection of files that are installed directly into an `install` prefix.

Conda keeps track of the dependencies between packages and platforms; the conda package format is
identical across platforms and operating systems.

### Package Structure

All conda packages have a specific sub-directory structure inside the file. There is a
`bin` directory that contains any binaries for the package; a `lib` directory containing the
relevant library files (e.g., the `.py` files); and an `info` directory containing package metadata.
For a more details of the conda package specification, including discussions of the various
metadata files, see the [docs](https://docs.conda.io/projects/conda-build/en/latest/resources/package-spec.html).

## What are Conda channels?

Again from the [Conda documentation](https://conda.io/en/latest/), conda packages are downloaded from
remote channels, which are URLs to directories containing conda packages. The `conda` command
searches a default set of channels, and packages are automatically downloaded and updated from the
[Anaconda Cloud channels](https://repo.anaconda.com/pkgs/).

*   `main`: The majority of all new Anaconda, Inc. package builds are hosted here. Included in
    conda's defaults channel as the top priority channel.
*   `r`: Microsoft R Open conda packages and [Anaconda, Inc.'s R conda packages](https://anaconda.org/r/repo).
    This channel is included in conda's defaults channel. When creating new environments, MRO is
    now chosen as the default R implementation.

Collectively, the Anaconda managed channels are referred to as the `defaults` channel because,
unless otherwise specified, packages installed using `conda` will be downloaded from these
channels.

> ## The `conda-forge` channel
>
> In addition to the `default` channels that are managed by Anaconda Inc., there is another channel called that also has
> a special status. The [Conda-Forge](https://github.com/conda-forge) project "_is a community led collection of
> recipes, build infrastructure and distributions for the conda package manager._"
>
> There are a few reasons that you may wish to use the `conda-forge` channel instead of the
> `defaults` channel maintained by Anaconda:
>
> 1. Packages on `conda-forge` may be more up-to-date than those on the `defaults` channel.
> 2. There are packages on the `conda-forge` channel that aren't available from `defaults`.
> 3. You may wish to use a dependency such as `openblas` (from `conda-forge`) instead of `mkl`
> (from `defaults`).
{: .callout}

## How do I install a package from a specific channel?

You can install a package from a specific channel into the currently activate environment by
passing the `--channel` option to the `conda install` command as follows.

~~~
$ conda activate machine-learning-env
$ conda install scipy=1.10.0 --channel conda-forge
~~~
{: .language-bash}


You can also install a package from a specific channel into a named environment (using `--name`). For example, the
following command installs the `scipy` package from the `conda-forge` channel into the environment
called `my-first-conda-env` which we created earlier.

~~~
$ conda install scipy=1.10.0 --channel conda-forge --name machine-learning-env
~~~
{: .language-bash}

The following command would install `tensorflow` package from `conda-forge` channel into the environment called
`machine-learning-env`.

~~~
$ conda install tensorflow=1.14 --channel conda-forge --name machine-learning-env
~~~
{: .language-bash}

> ## Channel priority
>
> You may specify multiple channels for installing packages by passing the `--channel` argument
> multiple times.
>
> ~~~
> $ conda install scipy=1.6 --channel conda-forge --channel bioconda
> ~~~
> {: .language-bash}
>
> Channel priority decreases from left to right - the first argument has higher priority than the
> second. For reference, [bioconda](https://bioconda.github.io/) is a channel for the conda package manager specializing
> in bioinformatics software. For those interested in learning more about the Bioconda project, checkout the project's
> [GitHub](https://bioconda.github.io/) page.
>
> Please note that in our example, adding `bioconda` channel is irrelevant because `scipy` is no longer available on
> `bioconda` channel.
{: .callout}

## My package isn't available on the `defaults` channel! What should I do?

It may very well be the case that packages (or often more recent versions of packages!) that you need to
install for your project are not available on the `defaults` channel.  In this case you should try the
following.

1.  `conda-forge`: the `conda-forge` channel contains a large number of community curated conda
    packages. Typically the most recent versions of packages that are generally available via the
    `defaults` channel are available on `conda-forge` first.
2. `bioconda`: the `bioconda` channel also contains a large number of Bioinformatics curated conda packages.
    `bioconda` channel is meant to be used with `conda-forge`, you should not worried about using the two channels
    when installing your prefered packages.
3.  `pip`: if a package is not otherwise available via `conda-forge` (or some
    domain-specific channel like `bioconda`) a package can be installed into a conda
    environment from PyPI using `pip`. It's fine to just use `pip` for installing most packages.

For example, [Kaggle](https://www.kaggle.com/) publishes a Python 3 API that can be used to interact with Kaggle datasets, kernels and competition submissions. You can search for the package on the `defaults` channels but you will not find it!

~~~
$ conda search kaggle
Loading channels: done
No match found for: kaggle. Search: *kaggle*

PackagesNotFoundError: The following packages are not available from current channels:

  - kaggle

Current channels:

  - https://repo.anaconda.com/pkgs/main/osx-64
  - https://repo.anaconda.com/pkgs/main/noarch
  - https://repo.anaconda.com/pkgs/free/osx-64
  - https://repo.anaconda.com/pkgs/free/noarch
  - https://repo.anaconda.com/pkgs/r/osx-64
  - https://repo.anaconda.com/pkgs/r/noarch

To search for alternate channels that may provide the conda package you're
looking for, navigate to

    https://anaconda.org

and use the search bar at the top of the page.
~~~
{: .language-bash}

The [official installation instructions](https://github.com/Kaggle/kaggle-api) suggest downloading
the `kaggle` package using `pip`. But since we are using `conda`, let's check whether the
package exists on at least `conda-forge` channel before proceeding to use `pip`.

~~~
$ conda search --channel conda-forge kaggle
Loading channels: done
# Name                       Version           Build  Channel
kaggle                         1.5.3          py27_1  conda-forge
kaggle                         1.5.3          py36_1  conda-forge
kaggle                         1.5.3          py37_1  conda-forge
kaggle                         1.5.4          py27_0  conda-forge
kaggle                         1.5.4          py36_0  conda-forge
kaggle                         1.5.4          py37_0  conda-forge
.
.
.
kaggle                        1.5.12  py38h578d9bd_1  conda-forge
kaggle                        1.5.12  py38h578d9bd_2  conda-forge
kaggle                        1.5.12  py39hf3d152e_0  conda-forge
kaggle                        1.5.12  py39hf3d152e_1  conda-forge
kaggle                        1.5.12  py39hf3d152e_2  conda-forge
kaggle                        1.5.12    pyhd8ed1ab_4  conda-forge
~~~
{: .language-bash}

Or you can also check online at [https://anaconda.org/conda-forge/kaggle](https://anaconda.org/conda-forge/kaggle).

Once we know that the `kaggle` package is available via `conda-forge` we can go ahead and install
it.

~~~
$ conda install --channel conda-forge kaggle=1.5.12  --name machine-learning-env
~~~
{: .language-bash}

## What actually happens when I install packages?

During the installation process, files are extracted into the specified environment (defaulting to
the current environment if none is specified). Installing the files of a conda package into an
environment can be thought of as changing the directory to an environment, and then downloading
and extracting the package and its dependencies.

For example, when you `conda install` a package that exists in a channel and has no dependencies,
conda does the following.

1. looks at your configured channels (in priority)
2. reaches out to the repodata associated with your channels/platform
3. parses repodata to search for the package
4. once the package is found, conda pulls it down and installs

The [conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/concepts/installing-with-conda.html) has a nice decision tree that describes the package installation process.

<p align="center">
    <img alt="Installing with Conda" src="../fig/installing-with-conda.png" width="250">
</p>

> ## Specifying channels when installing packages
>
> Like many projects, [`PyTorch`](https://pytorch.org/) has its own
> [channel](https://anaconda.org/pytorch) on Anaconda Cloud. This channel has several interesting
> packages, in particular `pytorch` (PyTorch core) and `torchvision` (datasets, transforms, and
> models specific to computer vision).
>
> Create a new directory called `my-computer-vision-project` and then create a Python 3.10
> environment called `my-computer-vision-project` with the two packages listed above. Also include
> the most recent version of `jupyterlab` in your environment (so you have a nice UI) and
> `matplotlib` (so you can make plots).
>
> > ## Solution
> >
> > In order to create a new environment you use the `conda create` command as follows.
> >
> > ~~~
> > $ mkdir my-computer-vision-project
> > $ cd my-computer-vision-project/
> > $ conda create --name my-computer-vision-project --channel pytorch python=3.10 jupyterlab pytorch=1.13.1 torchvision=0.14.1 matplotlib
> > ~~~
> > {: .language-bash}
> >
> > Hint: For the lazy typers: the `--channel` argument can also be shortened to `-c`, for more
> > abbreviations, see also the
> > [Conda command reference](https://docs.conda.io/projects/conda/en/latest/commands.html) .
> {: .solution}
{: .challenge}

> ## Alternative syntax for installing packages from specific channels
>
> There exists an alternative syntax for installing conda packages from specific channels that
> more explicitly links the channel being used to install a particular package.
>
> ~~~
> $ conda install conda-forge::tensorflow --name my-computer-vision-project
> ~~~
> {: .language-bash}
>
> Create a new folder `my-final-project` in `~/Desktop/introduction-to-conda-for-data-scientists` and repeat the
> previous exercise using this alternative syntax to install `python`, `jupyterlab`, and `matplotlib` from the
> `conda-forge` channel and `pytorch` and `torchvision` from the `pytorch` channel.
>
> > ## Solution
> >
> > One possibility would be to use the `conda create` command as follows.
> >
> > ~~~
> > $ cd ~/Desktop/introduction-to-conda-for-data-scientists
> > $ mkdir my-final-project
> > $ cd my-final-project/
> > $ conda create --name my-final-project conda-forge::python=3.10 conda-forge::jupyterlab conda-forge::matplotlib pytorch::pytorch=1.13.1  pytorch::torchvision=0.14.1
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## A Python package isn't available on any Conda channel! What should I do?

If a Python package that you need isn't available on any Conda channel, then you can use the default Python package
manager [`pip`](https://pip.pypa.io/en/stable/) to install it  from [Python Package Index
(PyPI)](https://pypi.org/). However, there are a few [potential
issues](https://www.anaconda.com/blog/using-pip-in-a-conda-environment) that you should be aware of when using `pip` to
install Python packages when using Conda.

First, `pip` is sometimes installed by default on operating systems where it is used to manage any Python packages
needed by your OS. **You do not want to use this `pip` to install Python packages when using Conda environments.**

~~~
(base) $ conda deactivate
$ which python
/usr/bin/python
$ which pip # sometimes installed as pip3
/usr/bin/pip
~~~
{: .language-bash}

Second, `pip` is also included in the Miniconda installer where it is used to install and manage OS specific Python
packages required to setup your `base` Conda environment. **You do not want to use this `pip` to install Python packages
when using Conda environments.**

~~~
$ conda activate
(base) $ which python
~/miniconda3/bin/python
$ which pip
~/miniconda3/bin/pip
~~~
{: .language-bash}

> ## Another reaon to avoid installing packages into your `base` Conda environment
>
> If your `base` Conda environment becomes cluttered with a mix of `pip` and Conda installed
> packages it may no longer function. Creating separate conda environments allows you to
> delete and recreate environments readily so you dont have to worry about risking your core
> Conda functionality when mixing packages installed with Conda and Pip.
{: .callout}

If you find yourself needing to install a Python package that is only available via PyPI, then you should first install
`pip` into your Conda environment and then use that `pip` to install  the desired package from PyPI. Using the `pip`
installed in your Conda environment to install Python packages not available via Conda channels will help you avoid
difficult to debug issues that frequently  arise when using Python packages installed via a `pip` that was not installed
inside you Conda environment.

> ## Conda (+Pip)
>
> Pitfalls of using Conda and `pip` together can be avoided by adopting the following practices.
>
> * Always be sure your desired environment is *active* before installing anything using `pip`.
> * Always explicitly install `pip` in *every* Python-based Conda environment (`conda install pip`).
{: .callout}

> ## Installing packages into Conda environments using `pip`
>
> [Combo](https://github.com/yzhao062/combo) is a comprehensive Python toolbox for combining
> machine learning models and scores. Model combination can be considered as a subtask of
> [ensemble learning](https://en.wikipedia.org/wiki/Ensemble_learning), and has been widely used
> in real-world tasks and data science competitions like [Kaggle](https://www.kaggle.com/).
>
> Activate the `machine-learning-env` you created in a previous challenge and use `pip` to install
> `combo`.
>
> > ## Solution
> >
> > The following commands will activate the `basic-scipy-env` and install `combo`.
> >
> > ~~~
> > $ conda install --name machine-learning-env pip
> > $ conda activate machine-learning-env
> > $ python -m pip install combo==0.1.*
> > ~~~
> > {: .language-bash}
> >
> > For more details on using `pip` see the [official documentation](https://pip.pypa.io/en/stable/).
> {: .solution}
{: .challenge}

{% include links.md %}
