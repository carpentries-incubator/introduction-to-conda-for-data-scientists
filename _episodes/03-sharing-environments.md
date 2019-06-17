---
title: "Sharing Environments"
teaching: 25
exercises: 10
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

### Exporting an existing environment to a YAML file

In order to make sure that your environment is truly shareable, you need to make sure that 
that the contents of your environment are described in such a way that the resulting 
`environment.yml` file can be used to re-create your environment on Linux, Mac OS, and Windows. 

> ## Default `environment.yml` file
> 
> Note that by convention Conda environment files are called `environment.yml`. As such if you use 
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to 
> find a file called `environment.yml` in the current working directory and will throw an error if a 
> file with that name can not be found.
{: .callout}

The `conda` command to export an existing environment is the following.

~~~
$ conda env export --name explicit-conda-env
~~~
{: .language-bash}

When you run this command, you will see the resulting YAML formatted representation of your Conda 
environment streamed to the terminal. Recall that we only listed five packages when we 
originally created `explicit-conda-env` yet from the output of the `conda env export` command 
we see that these five packages result in an environment with over 80 dependencies!

~~~
name: explicit-conda-env
channels:
  - defaults
dependencies:
  - appnope=0.1.0=py36hf537a9a_0
  - attrs=19.1.0=py36_1
  - backcall=0.1.0=py36_0
  - blas=1.0=mkl
  - bleach=3.1.0=py36_0
  - ca-certificates=2019.5.15=0
  - certifi=2019.3.9=py36_0
  - cycler=0.10.0=py36hfc81398_0
  - decorator=4.4.0=py36_1
  - defusedxml=0.6.0=py_0
  - entrypoints=0.3=py36_0
  - freetype=2.9.1=hb4e5f40_0
  - intel-openmp=2019.4=233
  - ipykernel=5.1.1=py36h39e3cac_0
  - ipython=7.5.0=py36h39e3cac_0
  - ipython_genutils=0.2.0=py36h241746c_0
  - jedi=0.13.3=py36_0
  - jinja2=2.10.1=py36_0
  - joblib=0.13.2=py36_0
  - jsonschema=3.0.1=py36_0
  - jupyter_client=5.2.4=py36_0
  - jupyter_core=4.4.0=py36_0
  - jupyterlab=0.35.5=py36hf63ae98_0
  - jupyterlab_server=0.2.0=py36_0
  - kiwisolver=1.1.0=py36h0a44026_0
  - libcxx=4.0.1=hcfea43d_1
  - libcxxabi=4.0.1=hcfea43d_1
  - libedit=3.1.20181209=hb402a30_0
  - libffi=3.2.1=h475c297_4
  - libgfortran=3.0.1=h93005f0_2
  - libpng=1.6.37=ha441bb4_0
  - libsodium=1.0.16=h3efe00b_0
  - llvm-openmp=4.0.1=hcfea43d_1
  - markupsafe=1.1.1=py36h1de35cc_0
  - matplotlib=3.0.3=py36h54f8f79_0
  - mistune=0.8.4=py36h1de35cc_0
  - mkl=2019.4=233
  - mkl_fft=1.0.12=py36h5e564d8_0
  - mkl_random=1.0.2=py36h27c97d8_0
  - nbconvert=5.5.0=py_0
  - nbformat=4.4.0=py36h827af21_0
  - ncurses=6.1=h0a44026_1
  - notebook=5.7.8=py36_0
  - numpy=1.16.4=py36hacdab7b_0
  - numpy-base=1.16.4=py36h6575580_0
  - openssl=1.1.1c=h1de35cc_1
  - pandas=0.24.2=py36h0a44026_0
  - pandoc=2.2.3.2=0
  - pandocfilters=1.4.2=py36_1
  - parso=0.4.0=py_0
  - pexpect=4.7.0=py36_0
  - pickleshare=0.7.5=py36_0
  - pip=19.1.1=py36_0
  - prometheus_client=0.6.0=py36_0
  - prompt_toolkit=2.0.9=py36_0
  - ptyprocess=0.6.0=py36_0
  - pygments=2.4.2=py_0
  - pyparsing=2.4.0=py_0
  - pyrsistent=0.14.11=py36h1de35cc_0
  - python=3.6.8=haf84260_0
  - python-dateutil=2.8.0=py36_0
  - pytz=2019.1=py_0
  - pyzmq=18.0.0=py36h0a44026_0
  - readline=7.0=h1de35cc_5
  - scikit-learn=0.21.1=py36h27c97d8_0
  - scipy=1.2.1=py36h1410ff5_0
  - send2trash=1.5.0=py36_0
  - setuptools=41.0.1=py36_0
  - six=1.12.0=py36_0
  - sqlite=3.28.0=ha441bb4_0
  - terminado=0.8.2=py36_0
  - testpath=0.4.2=py36_0
  - tk=8.6.8=ha441bb4_0
  - tornado=6.0.2=py36h1de35cc_0
  - traitlets=4.3.2=py36h65bd3ce_0
  - wcwidth=0.1.7=py36h8c6ec74_0
  - webencodings=0.5.1=py36_1
  - wheel=0.33.4=py36_0
  - xz=5.2.4=h1de35cc_4
  - zeromq=4.3.1=h0a44026_3
  - zlib=1.2.11=h1de35cc_3
prefix: /Users/pughdr/miniconda3/envs/explicit-conda-env
~~~
{: .language-bash}

> ## YAML Ain't Markup Language (YAML)
> 
> YAML ("YAML Ain't Markup Language") is a human-readable data-serialization language that is 
> commonly used for configuration files that uses Python-style indentation to indicate nesting. 
> 
{: .callout}

Now you *could* copy and paste the output into a text file and save it as `environment.yml` but a 
*better* approach is to re-direct the output of the `conda env export` sub-command directly into 
an `environment.yml` file!

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

Let's take a look at a few example `environment.yml` files  to give you an idea of how to write 
your own environment files.

~~~
name: machine-learning-env

dependencies:
  - jupyterlab
  - matplotlib
  - pandas
  - scikit-learn
~~~

This `environment.yml` file would create an environment called `machine-learning-env` with the 
most current and mutually compatible versions of the listed packages (including all required 
dependencies). The newly created environment would be installed inside the `~/miniconda3/envs/` 
directory. Alternatively, if we intended this environment file to be used to create an environment 
inside a sub-directory call `./env` of the project directory, then we should set then `name` key 
to `null` as follows.

~~~
name: null

dependencies:
  - jupyterlab
  - matplotlib
  - pandas
  - scikit-learn
~~~

Finally, since explicit versions numbers for all packages should be preferred a better environment 
file would be the following.

~~~
name: null

dependencies:
  - jupyterlab=0.35.*
  - matplotlib=0.3.*
  - pandas=0.24.*
  - scikit-learn=0.21.*
~~~

Note the use of the wildcard `*` when defining the patch version number. Defining the version 
number by fixing the major and minor version numbers while allowing the patch version number to 
vary allows us to use our environment file to update our environment to get any bug fixes whilst 
still maintaining consistency of software environment.

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
> Create a new project directory and then create a new `environment.yml` file inside your project 
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
> > $ conda env create --file environment.yml --prefix ./env
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
