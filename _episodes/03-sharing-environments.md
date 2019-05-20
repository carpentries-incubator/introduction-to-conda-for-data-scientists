---
title: "Sharing Environments"
teaching: 15
exercises: 5
questions:
- "Why should I share my Conda environment with others?"
- "How do I share my Conda environment with others?"
objectives:
- "Make an exact copy of an environment."
- "Export an environment to a YAML file that can be read by Windows, Mac OS, or Linux."
- "Create an environment from a YAML file."
- "Export an environment with exact package versions for your OS."
- "Create an environment based on exact package versions."
keypoints:
- "Sharing Conda environments with other researchers facilitates the reprodicibility of your research."
- "Use Conda to create an`environment.yml` file that describes your project's software environment."
---

> ## Making a copy of an existing environment
>
> Create an new environment called `pyspark-env` with Python 3.6 and the most current versions of 
> [JupyterLab](https://jupyterlab.readthedocs.io/en/stable/), 
> [Matplotlib](https://matplotlib.org/), [Pandas](https://pandas.pydata.org/), 
> [pyarrow](https://arrow.apache.org/docs/python/), and 
> [pySpark](https://spark.apache.org/). Use Conda to create an exact copy of this environment 
> called `cloned-pyspark-env`.
> 
> > ## Solution
> > 
> > First create the environment using the `conda create` command as follows.
> > ~~~
> > $ conda create --name pyspark-env python=3.6 jupyterlab matplotlib pandas pyarrow pyspark
> > ~~~
> > {: .language-bash}
> >
> > Now you can create an exact copy of the environment using the `conda create` command as follows.
> > ~~~
> > $ conda create --clone pyspark-env --name cloned-pyspark-env
> > ~~~
> > {: .language-bash}
> {: .solution}
{: .challenge}

## Working with `environment.yml` files

When working on a collaborative research project it is often the case that your operating system 
might differ from the operating systems used by your collaborators. Similarly, the operating 
system used on a remote cluster to which you have access will likely differ from the operating 
system that you use on your local machine. In these cases it is useful to create an operating 
system agnostic environment file which you can share with collaborators or use to re-create an 
environment on a remote cluster that you developed on your local machine. 

### Exporting an existing environment to a YAML file

> ## Export an environment to a YAML file.
> 
> Export the `pyspark-env` environment created in a previous challenge to a YAML file called 
> `environment.yml`.
> 
> > ## Solution
> > 
> > Export an existing environment using the `conda env export` sub-command as follows.
> > ~~~
> > $ conda env export --name pyspark-env > environment.yml
> > ~~~
> > {: .language-bash}
> >
> > Note that the `>` shell operator re-directs the output of the `conda env export` sub-command 
> > to the file called `environment.yml`. If such a file does not already exist, then it will be 
> > created. If a file with the name `environment.yml` already exists it will be overwritten.
> {: .solution}
{: .challenge}

### Create a new environment from a YAML file

> ## Default `environment.yml` file
> 
> Note that by convention Conda environment files are called `environment.yml`. As such if you use 
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to 
> find a file called `environment.yml` in the current working directory and will throw an error if a 
> file with that name can not be found.
{: .callout}

> ## Create a new environment from a YAML file.
> 
> Create a new file in your current working directory called `my-environment-file.yml` with the 
> following contents.
>
> ~~~
> name: basic-machine-learning-env
> 
> channels:
>   - defaults
> 
> dependencies:
>   - matplotlib=3.0.*
>   - pandas=0.24.*
>   - python=3.6.*
>   - scikit-learn=0.21.*
>   - py-xgboost=0.80.*
>
> ~~~
> Now use this file to create a new Conda environment.
> 
> > ## Solution
> > 
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> > ~~~
> > $ conda env create --file my-environment-file.yml
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

{% include links.md %}
