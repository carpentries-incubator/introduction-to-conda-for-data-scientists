---
title: "Sharing Environments"
teaching: 30
exercises: 15
questions:
- "Why should I share my Conda environment with others?"
- "How do I share my Conda environment with others?"
- "How do I create a custom kernel for my Conda environments inside JupyterLab?"
objectives:
- "Create an environment from a YAML file that can be read by Windows, Mac OS, or Linux."
- "Create an environment based on exact package versions."
- "Create a custom kernel for a Conda environment for use inside JupyterLab and Jupyter notebooks."
keypoints:
- "Sharing Conda environments with other researchers facilitates the reproducibility of your research."
- "Create an`environment.yml` file that describes your project's software environment."
---

## Working with environment files

When working on a collaborative research project it is often the case that your operating system
might differ from the operating systems used by your collaborators. Similarly, the operating
system used on a remote cluster to which you have access will likely differ from the operating
system that you use on your local machine. In these cases it is useful to create an operating
system agnostic environment file which you can share with collaborators or use to re-create an
environment on a remote cluster.

### Creating an environment file

In order to make sure that your environment is truly shareable, you need to make sure that
that the contents of your environment are described in such a way that the resulting
environment file can be used to re-create your environment on Linux, Mac OS, and Windows. Conda
uses [YAML (YAML Ain't Markup Language)](https://yaml.org/) for writing its environment files. YAML is a human-readable
data-serialization language that is commonly used for configuration files and that uses Python-style indentation to
indicate nesting.

Creating your project's Conda environment from a single environment file is a Conda "best practice".
Not only do you have a file to share with collaborators but you also have a file that can be placed
under version control which further enhances the reproducibility of your research project and workflow.

> ## Default `environment.yml` file
>
> Note that by convention Conda environment files are called `environment.yml`. As such if you use
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to
> find a file called `environment.yml` in the current working directory and will throw an error if a
> file with that name can not be found.
{: .callout}

Let's take a look at a few example `environment.yml` files to give you an idea of how to write your own environment
files.

~~~
name: machine-learning-env

dependencies:
  - ipython
  - matplotlib
  - pandas
  - pip
  - python
  - scikit-learn
~~~
{: .language-yaml}

This `environment.yml` file would create an environment called `machine-learning-env` with the
most current and mutually compatible versions of the listed packages (including all required
dependencies). The newly created environment would be installed inside the `~/miniconda3/envs/`
directory, unless we specified a different path using `--prefix`.

If you prefer you can use explicit versions numbers for all packages:

~~~
name: machine-learning-env

dependencies:
  - ipython=8.8
  - matplotlib=3.6
  - pandas=1.5
  - pip=22.3
  - python=3.10
  - scikit-learn=1.2
~~~
{: .language-yaml}

Note that we are only specifying the major and minor version numbers and not the patch or build
numbers. Defining the version number by fixing only the major and minor version numbers while
allowing the patch version number to vary allows us to use our environment file to update our
environment to get any bug fixes whilst still maintaining significant consistency of our
Conda environment across updates.

> ## *Always* version control your `environment.yml` files!
>
> While you should *never* version control the contents of your `~/miniconda/env/` environment sub-directory,
> you should *always* version control your `environment.yml` files. Version controlling your
> `environment.yml` files together with your project's source code means that you always know
> which versions of which packages were used to generate your results at any particular point in
> time.
>
> Version control is a system of keeping track of changes that are made to files, in this case the `environment.yml`
> file. It's really useful to do so if for example you make a change, for example updating a specific version of a
> package and you find it breaks something in your environment or when running your code as you then have a record of
> what it previously was and can revert the changes.
>
> There are many systems for version control but the one you are most likely to encounter and we would recommend
> learning is [Git](https://git-scm.com/). Unfortunately the topic is too broad to cover in this material but we include
> the commands to version control your files at the command line using Git.
{: .callout}

Let's suppose that you want to use the `environment.yml` file defined above to create a Conda
environment in a sub-directory of some project directory. Here is how you would accomplish this
task.

~~~
$ cd ~/Desktop/introduction-to-conda-for-data-scientists
$ mkdir project-dir
$ cd project-dir
~~~

Once your project folder is created, create `environment.yml` using your favourite editor for instance `nano`.
Finally create a new Conda environment:

~~~
$ conda env create --name project-env --file environment.yml
$ conda activate project-env
~~~
{: .language-bash}

Note that the above sequence of commands assumes that the `environment.yml` file is stored within
your `project-dir` directory.

## Automatically generate an `environment.yml`

To export the packages installed into the previously created `machine-learning-env` you can run the
following command:

~~~
$ conda env export --name machine-learning-env
~~~
{: .language-bash}

When you run this command, you will see the resulting YAML formatted representation of your Conda
environment streamed to the terminal. Recall that we only listed five packages when we
originally created `machine-learning-env` yet from the output of the `conda env export` command
we see that these five packages result in an environment with roughly 80 dependencies!

> ## What's in the exported environment.yml file
>
> The exported version of the file looks a bit different to the one we wrote. In addition to version
> numbers, on some lines we've got another code in there e.g. `vs2015_runtime=14.34.31931=h4c5c07a_10`. The `h4c5c07a_10`
> is the [build variant hash](https://docs.conda.io/projects/conda-build/en/latest/resources/variants.html#differentiating-packages-built-with-different-variants). This appears when the package is different
> for different operating systems. The implication is that an environment file that contains a build variant
> hash for one or more of the packages cannot be used on a different operating system to the one it
> was created on.
>
{: .callout}

To export this list into an environment.yml file, you can use `--file` option to directly save the
resulting YAML environment into a file. If the target for `--file` exists it will be over-written so make sure your
filename is unique. So that we do not over-write `environment.yaml` we save the output to `machine-learning-env.yaml`
instead and add it to

~~~
$ conda env export --name machine-learning-env --file machine-learning-env.yml
$ git init
$ git add machine-learning-env.yml
$ git commit -m "Adding machine-learning-env.yml config file."
~~~
{: .language-bash}


This exported environment file may not *consistently* produce environments that are reproducible
across operating systems. The reason for this is, that it may include operating system specific low-level
packages which cannot be used by other operating systems.

**If you need an environment file that can produce environments that are reproducible across Mac OS, Windows,
and Linux, then you are better off just including those packages into the environment file that you have
specifically installed.**

~~~
$ conda env export --name machine-learning-env --from-history --file machine-learning-history-env.yml
$ git add machine-learning-history-env.yml
$ git commit -m "Adding machine-learning-history-env.yml based on environment history"
~~~
{: .language-bash}

> ## Excluding build variant hash
>
> In short: to make sure others can reproduce your environment independent of the operating system they use,
> make sure to add the `--from-history` argument to the `conda env export` command. This will **only** include the
> packages you explicitly installed and the version you requested to be installed. For example if you installed
> `numpy-1.24` this will be listed, but if you installed `pandas` without a version and therefore installed the latest
> then `pandas` will be listed in your environment file without a version number so anyone using your environment file
> will get the latest version which may not match the version you used. This is one reason to explicitly state the
> version of a package you wish to install.
>
> Without `--from-history` the output may on some occasions include the build variant hash (which can alternatively be
> removed by editing the environment file). These are often specific to the operating system and including them in your
> environment file means it will not necessarily work if someone is using a different operating system.
>
> **Be aware that `--from-history` will omit any packages you have installed using `pip`. This may be [addressed in future releases](https://github.com/conda/conda/pull/11532). In the meantime, editing your exported environment files by hand is sometimes the best option.**
{: .callout}

> ## Create a new environment from a YAML file.
>
> Create a new project directory and then create a new `environment.yml` file inside your project
> directory with the following contents.
>
> ~~~
> name: scikit-learn-env
>
> dependencies:
>   - ipython=8.8
>   - matplotlib=3.6
>   - pandas=1.5
>   - pip=22.3
>   - python=3.10
>   - scikit-learn=1.2
> ~~~
> {: .language-yaml}
>
> Now use this file to create a new Conda environment. Where is this new environment created?
>
> > ## Solution
> >
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> >
> > ~~~
> > $ mkdir scikit-learn-project
> > $ cd scikit-learn-project
> > $ conda env create --file scikit-learn-env.yml
> > $ git init
> > $ git add scikit-learn-env.yml
> > $ git commit -m "Adding scikit-learn-env.yml config file"
> > ~~~
> > {: .language-bash}
> >
> > The above sequence of commands will create a new Conda environment inside the
> > `~/miniconda3/envs` directory.
> >
> > You can now run the `conda env list` command and see that this environment has been
> > created.
> {: .solution}
{: .challenge}

> ## Specifying channels in the environment.yml
>
> We learnt in the previous episode, that some packages may need to be installed from other than the
> defaults channel. We can also specify the channels that conda should look for the packages within the
> `environment.yml` file:
>
> ~~~
> name: pytorch-env
>
> channels:
>   - pytorch
>   - defaults
>
> dependencies:
>   - pytorch=1.13
> ~~~
> {: .language-yaml}
>
> When the above file is used to create an environment, conda would first look in the `pytorch` channel for
> all packages mentioned under `dependencies`. If they exist in the `pytorch` channel, conda would install
> them from there, and not look for them in `defaults` at all.
{: .callout}


### Updating an environment

You are unlikely to know ahead of time which packages (and version numbers!) you will need to use
for your research project. For example it may be the case that

* one of your core dependencies just released a new version (dependency version number update).
* you need an additional package for data analysis (add a new dependency).
* you have found a better visualization package and no longer need to old visualization package
    (add new dependency and remove old dependency).

If any of these occurs during the course of your research project, all you need to do is update
the contents of your `environment.yml` file accordingly and then run the following command.

~~~
$ conda env update --name project-env --file environment.yml --prune
$ git add environment.yml
$ git commit -m "Updating environment.yml config file"
~~~
{: .language-bash}

The `--prune` option tells Conda to remove any dependencies that are no longer required from the environment.

> ## Rebuilding a Conda environment from scratch
>
> When working with `environment.yml` files it is often just as easy to rebuild the Conda
> environment from scratch whenever you need to add or remove dependencies. To rebuild a Conda
> environment from scratch you can pass the `--force` option to the `conda env create` command
> which will remove any existing environment directory before rebuilding it using the provided
> environment file.
> ~~~
> $ conda env create --name project-env --file environment.yml --force
> ~~~
> {: .language-bash}
{: .callout}

> ## Add Dask to the environment to scale up your analytics
>
> Add `dask` to the `scikit-learn-env.yml` environment file and update the environment. [Dask](https://dask.org/)
> provides advanced parallelism for data science workflows enabling performance at scale for the
> core Python data science tools such as Numpy Pandas, and Scikit-Learn.
>
> > ## Solution
> >
> > The `scikit-learn-env.yml` file should now look as follows.
> >
> > ~~~
> > name: scikit-learn-env
> >
> > dependencies:
> >   - dask=2022.7
> >   - dask-ml=2022.5
> >   - ipython=8.8
> >   - matplotlib=3.6
> >   - pandas=1.5
> >   - pip=22.3
> >   - python=3.10
> >   - scikit-learn=1.2
> > ~~~
> >
> > You could use the following command, that will rebuild the environment from scratch with the
> > new Dask dependencies:
> >
> > ~~~
> > $ conda env create --name project-env --file environment.yml --force
> > ~~~
> > {: .language-bash}
> >
> > Or, if you just want to update the environment in-place with the new Dask dependencies, you can use:
> >
> > ~~~
> > $ conda env update --name project-env --file environment.yml  --prune
> > ~~~
> > {: .language-bash}
> >
> > You would then add and commit the changes to the `scikit-learn-env.yml` to Git to keep the changes under version
> > control.
> > ~~~
> > $ git add scikit-learn-env.yml
> > $ git commit -m "Updating scikit-learn-env.yml with dask"
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

> ## Installing via `pip` in `environment.yml` files
>
> Since you write `environment.yml` files for all of your projects, you might be wondering how
> to specify that packages should be installed using `pip` in the `environment.yml` file.  Here
> is an example `environment.yml` file that uses `pip` to install the `kaggle` and `yellowbrick`
> packages.
>
> ~~~
> name: example
>
> dependencies:
>  - jupyterlab=1.0
>  - matplotlib=3.1
>  - pandas=0.24
>  - scikit-learn=0.21
>  - pip=22.3
>  - pip:
>    - kaggle==1.5
>    - yellowbrick==1.5
> ~~~
>
> Note the double '==' instead of '=' for the `pip` installation and that you should include `pip` itself
> as a Conda dependency and then a subsection denoting those packages to be installed via `pip`. In case
> you are wondering, the [Yellowbrick](https://www.scikit-yb.org/en/latest/) package is a suite of
> visual diagnostic tools called “Visualizers” that extend the
> [Scikit-Learn](https://scikit-learn.org/stable/) API to allow human steering of the model selection
> process. Recent version of Yellowbrick can also be installed using `conda` from the `conda-forge` channel.
>
> ~~~
> $ conda install --channel conda-forge yellowbrick=1.2 --name project-env
> ~~~
>
> An alternative way of installing dependencies via `pip` in your environment files is to store all the
> packages that you wish to install via `pip` in a `requirements.txt` file and then add the following to
> your `environment.yml file`.
>
> ```
> ...
>   - pip
>   - pip:
>     - -r file:requirements.txt
> ```
>
> Conda will then install your `pip` dependencies using `python -m pip install -r requirements.txt`
> (after creating the Conda environment and installing all Conda installable dependencies).
>
> A `requirements.txt` file has a similar structure although it does not use YAML markup, instead it simply lists the
> packages by name. If a specific version is required then as above it is specified with `==`. Remember you should not
> include `pip` in the `requirements.txt` because this should be installed by and managed by Conda in your environment.
>
> If you use a `requirements.txt` file then you should add this to your Git repository so it too is maintained under
> version control.
>
> ```
> kaggle
> yellowbrick==1.5
> ```
{: .callout}

{% include links.md %}
