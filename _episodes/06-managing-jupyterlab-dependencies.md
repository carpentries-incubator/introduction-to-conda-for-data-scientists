---
title: "Managing JupyterLab-based data science projects using Conda (+pip)"
teaching: 45
exercises: 15
questions:
- "How are the common approaches to managing JupyterLab-based data science project using Conda (+pip)?"
- "What is the "system-wide" approach to managing a JupyterLab installation?"
- "What is the "project-based" approach to managing a JupyterLab installation?"
- "When should I prefer the "system-wide" approach to the "project-based" approach?"
objectives:
- "Discuss some common approaches to managing JupyterLab-base data science projects using Conda (+pip)."
- "Understand how to a manage a "system-wide" JupyterLab installation using Conda (+pip)."
- "Understand how to a manage a "project-based" JupyterLab installation using Conda (+pip)."
- "Understand the relevant tradeoffs between these two approaches."
keypoints:
- "With JupyterLab >= 3.0 the process of installing extensions is much improved: no `nodejs` dependency; no need to re-build JupyterLab to install and enable extensions."
---

## Managing a "system-wide" JupyterLab installation

Conda (+pip) manage a JupyterLab installation shared across all projects.

* Common set of JupyterLab extensions simplifies user interface (UI) and user experience (UX).
* Allows for quicker start of new projects as no need to install (and build!) JupyterLab.
* Easy low-level configuration of JupyterLab via files inside the `~/.jupyter` directory in your user home directory.

### Example `environment.yml` for a "system-wide" JupyterLab install

```yaml
name: jupyterlab-base-env

channels:
  - conda-forge
  - defaults
  
dependencies:
  - jupyterlab
  - jupyterlab-git # provides git support
  - nodejs # required for building (some) extensions using JupyterLab < 3.0
  - pip
  - pip:
    - -r file:requirements.txt # extensions available via pip go here
  - python
```

### "System-wide" JupyterLab best practices

[`jupyterlab-base-env`](https://github.com/davidrpugh/jupytercon-2020-talk/tree/jupyterlab-base-env) should *only* contain JupyterLab and required extensions (+deps).

* Automate environment build with Bash script.
* Projects should have separate Conda (+pip) environments.
* Create custom Jupyter kernels for project Conda (+pip) environments.

#### Automate environment build with Bash script

```bash
#!/bin/bash --login
set -e

conda env create \
    --name jupyterlab-base-env \
    --file environment.yml \
    --force
conda activate jupyterlab-base-env
source postBuild # put jupyter labextension install commands here
```

#### Example

https://github.com/davidrpugh/jupytercon-2020-talk/tree/jupyterlab-base-env

## Making Jupyter aware of your Conda environments

Both JupyterLab and Jupyter Notebooks automatically ensure that the standard IPython kernel is 
always available by default. However, if you want to use a kernel based on a particular Conda 
environment from inside Jupyter (and Juptyer is *not* installed inside your environment) then will 
need to create a 
[kernel spec](https://jupyter-client.readthedocs.io/en/latest/kernels.html#kernelspecs) file for 
your Conda environments manually.

Before you can create a custom kernel for you Conda environment you need to make sure that the 
[`ipykernel`](https://pypi.org/project/ipykernel/) package is installed in your Conda environment 
as you will need to use this package to create the kernel spec file. Here is the updated `xgboost-env`
`environment.yml` file that includes the `ipykernel` package.

~~~
name: ml-env

dependencies:
  - ipykernel=5.3
  - ipython=7.13
  - matplotlib=3.1
  - pandas=1.0
  - pip=20.0
  - python=3.6
  - scikit-learn=0.22
~~~
{: .language-yaml}

Next, rebuild the Conda environment using the following command.

~~~
$ conda env create --prefix ./env --file environment.yml --force
~~~

Once the Conda environment has been re-built you can activate the environment and then create the 
custom kernel for the activated environment.

~~~
$ conda activate ./env
$ python -m ipykernel install --user --name xgboost-env --display-name "XGBoost"
~~~

The last command installs a kernel spec file for the current environment. Kernel spec files are 
JSON files which can be viewed and changed with a normal text editor. The `--name` value is used 
by Jupyter internally; `--display-name` is what you see in the JupyterLab launcher menu as well as 
the Jupyter Notebook dropdown kernel menu. This command will overwrite any existing kernel with 
the same name. 

> ## Create a kernel for a Conda environment
>
> Create a custom kernel for the `machine-learning-env` environment created in a previous challenge.
> 
> > ## Solution
> > 
> > In order to activate an existing environment by name you use the `conda activate` command as 
> > follows.
> > 
> > ~~~
> > $ conda activate machine-learning-env
> > $ python -m ipykernel install --user --name machine-learning-env
> > ~~~
> > {: .language-bash}
> > 
> > Note that by leaving the `--display-name` unspecified, the display name will match the value 
> > provided to `--name` .
> >
> {: .solution}
{: .challenge}

#### Creating Jupyter kernels for Conda environments

Allows you to launch Jupyter Notebooks and IPython consoles for different Conda (+pip) environments within a common JupyterLab installation.

* Can automate process for all Conda (+pip) envs using [`jupyter-conda`](https://github.com/fcollonval/jupyter_conda) extension.
* Can manually create custom Jupyter kernel for particular Conda (+pip) envs.

#### How to manually create custom Jupyter kernel

```bash
conda activate $PROJECT_DIR/env # don't forget to activate env first!
python -m ipykernel install \ # requires ipykernel installed in the env
    --user \
    --name $PROJECT_NAME-kernel \ # for internal use only!
    --display-name "Name you will see in JupyterLab"
```


#### Example

![IMAGE](assets/img/jupyterlab-screenshot.png)


#### `%conda` and `%pip` magic commands

Built-in IPython magic commands for installing packages into the *active* kernel via Conda ([`%conda`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-conda)) or Pip ([`%pip`](https://ipython.readthedocs.io/en/stable/interactive/magics.html#magic-pip)).

* Both commands can be used from within Jupyter Notebooks or IPython consoles. 
* Both `%conda` and `%pip` are mostly useful for prototyping new projects.
* For "production", prefer adding new packages to `environment.yml`/`requirements.txt` (and rebuilding environment).


## "Project-based" JupyterLab

Conda (+pip) manage separate JupyterLab installations for each project.

* More flexible UI/UX as JupyterLab version and extensions can customized for each project.
* Easier experimentation with bleeding edge features of JupyterLab.
* Automatically makes a data science project repo ["Binder-ready"](https://mybinder.org/). 

### `environment.yml` for a "project-based" JupyterLab install

```yaml
name: null

channels:
  - conda-forge
  - defaults
  
dependencies:
  - jupyterlab
  - jupyterlab-git # extensions available via conda go here
  - nodejs # required for building (some) extensions using JupyterLab < 3.0
  - pandas # now project deps go here!
  - pip
  - pip:
    - -r file:requirements.txt # packages available via pip go here
  - python
```

#### Automate environment creation with Bash script

```bash
#!/bin/bash --login
set -e

export ENV_PREFIX=$PROJECT_DIR/env # directory included in .gitignore
conda env create \
    --prefix $ENV_PREFIX 
    --file environment.yml \
    --force
conda activate $ENV_PREFIX
source postBuild # put jupyter labextension install commands here
```

#### Examples of "project-based" JupyterLab installs

* [JupyterLab + Scikit Learn + friends](https://github.com/davidrpugh/jupytercon-2020-talk/tree/scikit-learn-env)
* [JupyterLab + PyTorch + friends](https://github.com/davidrpugh/jupytercon-2020-talk/tree/pytorch-env)
* [JupyterLab + NVIDIA RAPIDS + BlazingSQL + Dask](https://github.com/davidrpugh/jupytercon-2020-talk/tree/nvidia-rapids-env)

## "System-wide" or "project-based"?

* Prefer "project-based" for its greater flexibility with minimal additional overhead. 
* Prefer "project-based" if only some of your projects use GPUs. 
* Prefer "system-wide" when all projects use a common set of extensions.
