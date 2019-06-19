---
title: "Sharing Environments"
teaching: 25
exercises: 10
questions:
- "Why should I share my Conda environment with others?"
- "How do I share my Conda environment with others?"
objectives:
- "Create an environment from a YAML file that can be read by Windows, Mac OS, or Linux."
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

### Creating an environment file

In order to make sure that your environment is truly shareable, you need to make sure that 
that the contents of your environment are described in such a way that the resulting 
environment file can be used to re-create your environment on Linux, Mac OS, and Windows. 

> ## Default `environment.yml` file
> 
> Note that by convention Conda environment files are called `environment.yml`. As such if you use 
> the `conda env create` sub-command without passing the `--file` option, then `conda` will expect to 
> find a file called `environment.yml` in the current working directory and will throw an error if a 
> file with that name can not be found.
{: .callout}

> ## YAML Ain't Markup Language (YAML)
> 
> YAML ("YAML Ain't Markup Language") is a human-readable data-serialization language that is 
> commonly used for configuration files that uses Python-style indentation to indicate nesting.
>
{: .callout}

Creating you project's Conda environment from a single environment file is a Conda "best practice". 
Not only do you have a file to share with collaborators but you also have a file that can be placed 
under version control which further enhancing the reproducibility of your research project and 
workflow.

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
  - matplotlib=3.1.*
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
$ source activate ./env
~~~
{: .language-bash}

Note that the above sequence of commands assumes that the `environment.yml` file is stored within 
your `project-dir` directory. 

> ## Avoid the `conda env export` command
>  
> Many other Conda tutorials (including the official documentation) encourage the use of the 
> `conda env export` command to export an existing environment. For exampe to export the packages 
> installed into the previously created `machine-learning-env` you would run the following command.
> 
> ~~~
> $ conda env export --name machine-learning-env --no-builds
> ~~~
> {: .language-bash}
> 
> When you run this command, you will see the resulting YAML formatted representation of your Conda 
> environment streamed to the terminal. Recall that we only listed five packages when we 
> originally created `machine-learning-env` yet from the output of the `conda env export` command 
> we see that these five packages result in an environment with over 80 dependencies!
>
> ~~~
> name: machine-learning-env
> channels:
>   - defaults
> dependencies:
>   - appnope=0.1.0
>   - attrs=19.1.0
>   - backcall=0.1.0
>   - blas=1.0
>   - bleach=3.1.0
>   - ca-certificates=2019.5.15
>   - certifi=2019.3.9
>   - cycler=0.10.0
>   - decorator=4.4.0
>   - defusedxml=0.6.0
>   - entrypoints=0.3
>   - freetype=2.9.1
>   - intel-openmp=2019.4
>   - ipykernel=5.1.1
>   - ipython=7.5.0
>   - ipython_genutils=0.2.0
>   - jedi=0.13.3
>   - jinja2=2.10.1
>   - joblib=0.13.2
>   - jsonschema=3.0.1
>   - jupyter_client=5.2.4
>   - jupyter_core=4.4.0
>   - jupyterlab=0.35.5
>   - jupyterlab_server=0.2.0
>   - kiwisolver=1.1.0
>   - libcxx=4.0.1
>   - libcxxabi=4.0.1
>   - libedit=3.1.20181209
>   - libffi=3.2.1
>   - libgfortran=3.0.1
>   - libpng=1.6.37
>   - libsodium=1.0.16
>   - llvm-openmp=4.0.1
>   - markupsafe=1.1.1
>   - matplotlib=3.1.0
>   - mistune=0.8.4
>   - mkl=2019.4
>   - mkl-service=2.0.2
>   - mkl_fft=1.0.12
>   - mkl_random=1.0.2
>   - nbconvert=5.5.0
>   - nbformat=4.4.0
>   - ncurses=6.1
>   - notebook=5.7.8
>   - numpy=1.16.4
>   - numpy-base=1.16.4
>   - openssl=1.1.1c
>   - pandas=0.24.2
>   - pandoc=2.2.3.2
>   - pandocfilters=1.4.2
>   - parso=0.4.0
>   - pexpect=4.7.0
>   - pickleshare=0.7.5
>   - pip=19.1.1
>   - prometheus_client=0.6.0
>   - prompt_toolkit=2.0.9
>   - ptyprocess=0.6.0
>   - pygments=2.4.2
>   - pyparsing=2.4.0
>   - pyrsistent=0.14.11
>   - python=3.6.8
>   - python-dateutil=2.8.0
>   - pytz=2019.1
>   - pyzmq=18.0.0
>   - readline=7.0
>   - scikit-learn=0.21.2
>   - scipy=1.2.1
>   - send2trash=1.5.0
>   - setuptools=41.0.1
>   - six=1.12.0
>   - sqlite=3.28.0
>   - terminado=0.8.2
>   - testpath=0.4.2
>   - tk=8.6.8
>   - tornado=6.0.2
>   - traitlets=4.3.2
>   - wcwidth=0.1.7
>   - webencodings=0.5.1
>   - wheel=0.33.4
>   - xz=5.2.4
>   - zeromq=4.3.1
>   - zlib=1.2.11
> prefix: /Users/pughdr/miniconda3/envs/machine-learning-env
> ~~~
> 
> In practice this command does not *consistently* produce environments that are reproducible 
> across Mac OS, Windows, and Linux. The issue is that even after removing the build numbers (by 
> passing the `--no-builds` option), the environment file will often still contain Mac OS or 
> Windows specific packages that will not exist for Linux.
> {: .language-bash}

> ## Create a new environment from a YAML file.
> 
> Create a new project directory and then create a new `environment.yml` file inside your project 
> directory with the following contents.
>
> ~~~
> name: xgboost-env
> 
> dependencies:
>   - matplotlib=3.1.*
>   - pandas=0.24.*
>   - python=3.6.*
>   - scikit-learn=0.21.*
>   - py-xgboost=0.80.*
>   - pip=19.1.*
> ~~~
>
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
> >   - matplotlib=3.1.*
> >   - pandas=0.24.*
> >   - python=3.6.*
> >   - scikit-learn=0.21.*
> >   - py-xgboost=0.80.*
> >   - pip=19.1.*
> > ~~~
> >
> > And then use explicitly pass the `--prefix` to the `conda env create` command as follows.
> > 
> > ~~~
> > $ conda env create --file environment.yml --prefix ./env
> > ~~~
> > {: .language-bash}
> > 
> > You can now run the `conda env list` command and see that these two environments have been 
> > created in different locations.
> {: .solution}
{: .challenge}

> ## Create your own `environment.yml` file from scratch
> 
> Create a new project directory, write you own `environment.yml` file, and then use the file to 
> create an environment in a sub-directory of your project directory. Make sure to be provide 
> version numbers for your packages! 
>
> > ## Solution
> > 
> > To create a new environment from a YAML file use the `conda env create` sub-command as follows.
> > ~~~
> > $ mkdir project-dir
> > $ cd project-dir
> > $ nano environment.yml # write the contents of the environment file in the buffer
> > $ conda env create --file environment.yml -- prefix ./env
> > ~~~
> > {: .language-bash}
> >
> {: .solution}
{: .challenge}

{% include links.md %}
