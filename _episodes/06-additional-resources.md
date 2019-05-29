---
title: "Additional Resources"
teaching: 15
exercises: 0
questions:
- "What are some good resources to learn more?"
objectives:
- "First learning objective. (FIXME)"
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
---

## How do I create an exact copy of an existing environment?

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