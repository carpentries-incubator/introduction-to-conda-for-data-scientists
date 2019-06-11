---
title: "Sharing Environments"
teaching: 15
exercises: 5
questions:
- "Why should I share my Conda environment with others?"
- "How do I share my Conda environment with others?"
objectives:
- "Export an environment to a YAML file that can be read by Windows, Mac OS, or Linux."
- "Create an environment from a YAML file."
- "Export an environment with exact package versions for your OS."
- "Create an environment based on exact package versions."
keypoints:
- "Sharing Conda environments with other researchers facilitates the reprodicibility of your research."
- "Create an`environment.yml` file that describes your project's software environment."
---

## Working with environment files

When working on a collaborative research project it is often the case that your operating system 
might differ from the operating systems used by your collaborators. Similarly, the operating 
system used on a remote cluster to which you have access will likely differ from the operating 
system that you use on your local machine. In these cases it is useful to create an operating 
system agnostic environment file which you can share with collaborators or use to re-create an 
environment on a remote cluster. 

Great! So environment files can be used to share Conda environments with my collaborators or used 
and I can use environment files to re-create Conda environments on other computers including 
remote clusters. Sounds like environment files are really useful. How exactly do I create an 
environment file for an existing Conda environment?

> ## YAML Ain't Markup Language (YAML)
> 
> TODO
{: .callout}

> ## Default `environment.yml` file
> 
> Note that by convention Conda environment files are called `environment.yml`. As such if you use 
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to 
> find a file called `environment.yml` in the current working directory and will throw an error if a 
> file with that name can not be found.
{: .callout}

### Exporting an existing environment to a YAML file

In order to make sure that your environment is truly shareable, you need to make sure that 
that the contents of your environment are described in such a way that the resulting 
`enviroment.yml` file can be used to re-create your environment on Linux, Mac OS, and Windows. The 
`conda` command to accomplish this is the following.

~~~
$ conda env export --name explicit-conda-env
~~~
{: .language-bash}

If you run this command, you will see the resulting YAML formatted representation of your Conda 
environment streamed to the terminal. You *could* copy and paste the output into a text file and 
save it as `environment.yml` but a *better* approach is to re-direct the output of the 
`conda env export` sub-command directly into an `environment.yml` file!

~~~
$ conda env export --name explicit-conda-env > environment.yml
~~~
{: .language-bash}

Note that the `>` shell operator re-directs the output of the `conda env export` sub-command to 
the file called `environment.yml`. If such a file does not already exist, then it will be created. 
If a file with the name `environment.yml` already exists it will be overwritten.

> ## Exporting an existing environment using `--prefix`
>
> If the Conda environment that you wish to export is installed as a sub-directory inside your 
> project directory, then you can export the environment by passing the `--prefix` option instead 
> of the `--name` option to the `conda env export` sub-command as follows.
>
> ~~~
> $ conda env export --prefix ./env > environment.yml
> ~~~
> {: .language-bash}
{: .callout}

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
> {: .solution}
{: .challenge}

### Create a new environment from a YAML file

OK. So it seems pretty straightforward to create a platform agnostic `environment.yml` file from 
an existing Conda environment. But I would really like to be able to write an environment file for 
a Conda environment and then use that file to *create* the environment. Can I do that?

Yes! Creating you project's Conda environment from a single environment file is actually a Conda 
"best practice". Not only do you have a file to share with collaborators but you also have a file 
that can be placed under version control which further enhancing the reproducibility of your research 
project and workflow.

Let's take a look at a few example `environment.yml` files  to give you an idea of how to write your own environment files.

~~~
name: machine-learning-env

dependencies:
  - jupyterlab
  - matplotlib
  - pandas
  - scikit-learn
~~~

This `environment.yml` file would create an environment called `machine-learning-env` with the most current and mutually compatible versions of the listed packages (including all required dependecies). The newly created environment would be installed inside the `~/miniconda3/envs/` directory. Alternatively, if we intended this environment file to be used to create an environment inside a sub-directory call `./env` of the project directory we could define the enviroment file as follows.

~~~
name: null

dependencies:
  - jupyterlab
  - matplotlib
  - pandas
  - scikit-learn

prefix: ./env
~~~

Note that in the above `environment.yml` file we set the name to be `null` when we provide a `prefix`. Finally, since explicit versions numbers for all packages should be preferred a better environment file would be the following.

~~~
name: null

dependencies:
  - jupyterlab=0.35.*
  - matplotlib=0.3.*
  - pandas=0.24.*
  - scikit-learn=0.21.*

prefix: ./env
~~~

Note the use of the wildcard `*` when defining the patch version number. Defining the version number by fixing the major and minor version numbers while allowing the patch version number to vary allows us to use our environment file to update our environment to get any bug fixes whilst still maintaining consistency of software environment.

> ## *Always* version control your `environment.yml` files!
>
> While you should *never* version control the contents of your `env/` environment sub-directory, 
> you should *always* version control your `environment.yml` files. Version controlling your 
> `environment.yml` files together with your project's source code means that you always know 
> which versions of which packages were used to generate your results at any particular point in 
> time.
{: .callout}

Let's suppose that you want to use the `environment.yml` file defined above to create a Conda 
environment in a sub-directory of some project directory. Here is how you would accomplish this 
task.

~~~
$ cd project-dir
$ conda env create --prefix ./env --file environment.yml
$ conda activate ./env
~~~
{: .language-bash}

Note that the above sequence of commands assumes that the `environment.yml` file is stored within 
your `project-dir` directory. 

> ## Create a new environment from a YAML file.
> 
> Create a new project directory and then create a new `environment.yml` file insdie your project 
> directory with the following contents.
>
> ~~~
> name: basic-machine-learning-env
> 
> dependencies:
>   - matplotlib=3.0.*
>   - pandas=0.24.*
>   - python=3.6.*
>   - scikit-learn=0.21.*
>   - py-xgboost=0.80.*
>
> ~~~
> Now use this file to create a new Conda environment. Where is this new environment created? 
> Modify the `environment.yml` file so that the same `conda` commands will create a Conda 
> environment as a subdirectory called `env/` inside your project directory.
> 
> > ## Solution
> > 
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> > 
> > ~~~
> > $ mkdir project-dir
> > $ cd project-dir
> > $ nano environment.yml
> > $ conda env create --file environment.yml
> > ~~~
> > {: .language-bash}
> >
> > The above sequence of commands will create a new Conda environment inside the 
> > `~/miniconda3/envs` directory. In order to create the Conda environment inside a sub-directory 
> > of the project directory using the same sequence of `conda` commands, we need to modify the 
> > `environment.yml` file as follows.
> > 
> > ~~~
> > name: null
> >
> > dependencies:
> >   - matplotlib=3.0.*
> >   - pandas=0.24.*
> >   - python=3.6.*
> >   - scikit-learn=0.21.*
> >   - py-xgboost=0.80.*
> >
> > prefix: ./env
> > ~~~
> >
> {: .solution}
{: .challenge}

> ## Create your own `environment.yml` file from scratch
> 
> Create a new file in your current working directory called `environment.yml` with the following 
> contents.
>
> ~~~
> name: basic-machine-learning-env
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
> > $ conda env create --file environment.yml
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

{% include links.md %}
