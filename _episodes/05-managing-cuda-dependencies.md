---
title: "Managing GPU dependencies"
teaching: 45
exercises: 15
questions:
- "Which NVIDIA libraries are available via Conda?"
- "What do you do when you need the NVIDIA CUDA Compiler (NVCC) for your project?"
objectives:
- "Show how to use Conda to manage key GPU dependencies for you next (data) science project."
- "Show how to identify which versions of CUDA packages are available via Conda."
- "Understand how to write a Conda environment file for a project with GPU dependencies."
- "Understand when you need the NVIDIA CUDA Compiler (NVCC) and how  to handle this situation." 
keypoints:
- "Conda can be used to manage your key GPU dependencies."
- "Use `conda search` to identify which version of CUDA libraries are available."
- "For most projects you will not need NVCC and can use the `cudatoolkit` package from default channels."
- "If your project does need NVCC, try `cudatoolkit-dev` package or `nvcc_linux-64` meta-package (requires separate NVIDIA CUDA Toolkit 
  install)."
---

# Getting familiar with NVIDIA CUDA libraries

Transitioning your (data) science projects from CPU to GPU can seem like a daunting task. 
In particular, there is quite a bit of unfamiliar additional software, such as 
[NVIDIA CUDA Toolkit](https://developer.nvidia.com/cuda-toolkit), 
[NVIDIA Collective Communications Library (NCCL)](https://developer.nvidia.com/nccl), 
and [NVIDIA Deep Neural Network Library (cuDNN)](https://developer.nvidia.com/cudnn) to download 
and install.

If you go to the [NVIDIA developer](https://developer.nvidia.com/) website you will find loads of 
documentation and instructions for how to install these libraries system wide. But then what do 
you do if you need different versions of these new libraries for different projects? You could 
install a bunch of different versions of NVIDIA CUDA Toolkit, NCCL, and cuDNN system wide and then 
use environment variables to control the “active” versions for each project but this is cumbersome 
and error prone. Fortunately there are better ways!

In this episode we are going to see how to manage project specific versions of the NVIDIA CUDA 
Toolkit, NCCL, and cuDNN using Conda.

## Are NVIDIA libraries available via Conda?

Yep! The most important NVIDIA CUDA library that you will need is the NVIDIA CUDA Toolkit. The 
NVIDIA CUDA Toolkit provides a development environment for creating high performance 
GPU-accelerated applications. The toolkit includes GPU-accelerated libraries, debugging and 
optimization tools and a runtime library. You can use the `conda search` command to see what 
versions of the NVIDIA CUDA Toolkit are available from the default channels.

~~~
$ conda search cudatoolkit

Loading channels: done

# Name                       Version           Build  Channel
cudatoolkit                      9.0      h13b8566_0  pkgs/main
cudatoolkit                      9.2               0  pkgs/main
cudatoolkit                 10.0.130               0  pkgs/main
cudatoolkit                 10.1.168               0  pkgs/main
cudatoolkit                 10.1.243      h6bb024c_0  pkgs/main
cudatoolkit                  10.2.89      hfd86e86_0  pkgs/main
cudatoolkit                  10.2.89      hfd86e86_1  pkgs/main
~~~
{: .language-bash}

NVIDIA actually maintains their own Conda channel and the versions of CUDA Toolkit available from 
the default channels are the same as those you will find on the NVIDIA channel. If you are 
interested in confirming this you can run the command 

~~~
$ conda search --channel nvidia cudatoolkit
~~~
{: .language-bash}

and then compare the build numbers with those listed above from the default channels.

> ## The CUDA Toolkit packages available from defaults do not include NVCC
>
> An important limitation of the versions of the NVIDIA CUDA Toolkit that are available from the 
> either the default or NVIDIA Conda channels is that they do not include the NVIDIA CUDA Compiler 
> (NVCC).
{: .callout}

### What about cuDNN?

The [NVIDIA CUDA Deep Neural Network library (cuDNN)](https://developer.nvidia.com/cudnn) is a 
GPU-accelerated library of primitives for deep neural networks. cuDNN provides highly tuned 
implementations for standard routines such as forward and backward convolution, pooling, 
normalization, and activation layers. 

If you are interested in deep learning, then you will need to get your hands on cuDNN. Various 
versions of cuDNN are available from the default channels.

~~~
$ conda search cudnn

Loading channels: done

# Name                       Version           Build  Channel
cudnn                          7.0.5       cuda8.0_0  pkgs/main
cudnn                          7.1.2       cuda9.0_0  pkgs/main
cudnn                          7.1.3       cuda8.0_0  pkgs/main
cudnn                          7.2.1       cuda9.2_0  pkgs/main
cudnn                          7.3.1      cuda10.0_0  pkgs/main
cudnn                          7.3.1       cuda9.0_0  pkgs/main
cudnn                          7.3.1       cuda9.2_0  pkgs/main
cudnn                          7.6.0      cuda10.0_0  pkgs/main
cudnn                          7.6.0      cuda10.1_0  pkgs/main
cudnn                          7.6.0       cuda9.0_0  pkgs/main
cudnn                          7.6.0       cuda9.2_0  pkgs/main
cudnn                          7.6.4      cuda10.0_0  pkgs/main
cudnn                          7.6.4      cuda10.1_0  pkgs/main
cudnn                          7.6.4       cuda9.0_0  pkgs/main
cudnn                          7.6.4       cuda9.2_0  pkgs/main
cudnn                          7.6.5      cuda10.0_0  pkgs/main
cudnn                          7.6.5      cuda10.1_0  pkgs/main
cudnn                          7.6.5      cuda10.2_0  pkgs/main
cudnn                          7.6.5       cuda9.0_0  pkgs/main
cudnn                          7.6.5       cuda9.2_0  pkgs/main
~~~
{: .language-bash}

## What about NCCL?

If you are already accelerating your (data) science workflows with a GPU, then in the near future 
you will probably be interested in using more than one GPU. The 
[NVIDIA Collective Communications Library (NCCL)](https://developer.nvidia.com/nccl) implements 
multi-GPU and multi-node collective communication primitives that are performance optimized for 
NVIDIA GPUs.

There are some older versions of NCCL available from the default channels but these versions will 
not be useful (unless, perhaps, you are forced to use very old versions of TensorFlow or similar).

~~~
$ conda search nccl

Loading channels: done

# Name                       Version           Build  Channel
nccl                           1.3.5      cuda10.0_0  pkgs/main
nccl                           1.3.5       cuda9.0_0  pkgs/main
nccl                           1.3.5       cuda9.2_0  pkgs/main
~~~
{: .language-bash}

Not to worry: Conda Forge to the rescue! [Conda Forge](https://conda-forge.org/) is a 
community-led collection of recipes, build infrastructure and distributions for the Conda package 
manager. I always check the `conda-forge` channel when I can’t find something I need available on 
the default channels.

> ## Which version of NCCL are available via Conda Forge?
> 
> Find out which versions of the NVIDIA Collective Communications Library (NCCL) are available via 
> Conda Forge?
> 
> > ## Solution
> > 
> > Use the `conda search` command with the `--channel conda-forge` option.
> >
> > ~~~
> > $ conda search --channel conda-forge nccl
> > 
> > Loading channels: done
> > 
> > # Name                       Version           Build  Channel
> > nccl                           1.3.5      cuda10.0_0  pkgs/main
> > nccl                           1.3.5       cuda9.0_0  pkgs/main
> > nccl                           1.3.5       cuda9.2_0  pkgs/main
> > nccl                         2.4.6.1      h51cf6c1_0  conda-forge
> > nccl                         2.4.6.1      h7cc98d6_0  conda-forge
> > nccl                         2.4.6.1      hc6a2c23_0  conda-forge
> > nccl                         2.4.6.1      hd6f8bf8_0  conda-forge
> > nccl                         2.4.7.1      h51cf6c1_0  conda-forge
> > nccl                         2.4.7.1      h7cc98d6_0  conda-forge
> > nccl                         2.4.7.1      hd6f8bf8_0  conda-forge
> > nccl                         2.4.8.1      h51cf6c1_0  conda-forge
> > nccl                         2.4.8.1      h51cf6c1_1  conda-forge
> > nccl                         2.4.8.1      h7cc98d6_0  conda-forge
> > nccl                         2.4.8.1      h7cc98d6_1  conda-forge
> > nccl                         2.4.8.1      hd6f8bf8_0  conda-forge
> > nccl                         2.4.8.1      hd6f8bf8_1  conda-forge
> > nccl                         2.5.6.1      h51cf6c1_0  conda-forge
> > nccl                         2.5.6.1      h7cc98d6_0  conda-forge
> > nccl                         2.5.6.1      hc6a2c23_0  conda-forge
> > nccl                         2.5.6.1      hd6f8bf8_0  conda-forge
> > nccl                         2.5.7.1      h51cf6c1_0  conda-forge
> > nccl                         2.5.7.1      h7cc98d6_0  conda-forge
> > nccl                         2.5.7.1      hc6a2c23_0  conda-forge
> > nccl                         2.5.7.1      hd6f8bf8_0  conda-forge
> > nccl                         2.6.4.1      h51cf6c1_0  conda-forge
> > nccl                         2.6.4.1      h7cc98d6_0  conda-forge
> > nccl                         2.6.4.1      hc6a2c23_0  conda-forge
> > nccl                         2.6.4.1      hd6f8bf8_0  conda-forge
> > ~~~
> > {: .language-bash}
> {: .solution} 
{: .challenge}

# Some example Conda environment files

Now that you know how to figure out which versions of the various NVIDIA CUDA libraries are 
available on which channels you are ready to write your environment.yml file. In this section I 
will provide some example Conda environment files for PyTorch, TensorFlow, and NVIDIA RAPIDS to 
help get you started on your next GPU data science project.

## PyTorch

[PyTorch](https://pytorch.org/) is an open source machine learning library based on the Torch 
library, used for applications such as computer vision and natural language processing. It is 
primarily developed by Facebook’s AI Research lab. Conda is actually the 
[recommended way](https://pytorch.org/get-started/locally/) to install PyTorch. The official 
PyTorch binary ships with NCCL and cuDNN so it is not necessary to include these libraries in 
your `environment.yml` file (unless some other package also needs these libraries!).

~~~
name: null

channels:
  - pytorch
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit=10.1
  - pip=20.0
  - python=3.7
  - pytorch=1.5
  - torchvision=0.6
~~~
{: .language-yaml}

> ## Check you channel priorities!
> 
> Also take note of the channel priorities: the official `pytorch` channel must be given priority 
> over `conda-forge` in order to insure that the official PyTorch binaries (the ones that include 
> NCCL and cuDNN) will be installed (otherwise you will get some unofficial version of PyTorch 
> available on `conda-forge`).
{: .callout}

## TensorFlow

[TensorFlow](https://www.tensorflow.org/) is a free, open-source software library for dataflow and 
differentiable programming across a range of tasks. It is a symbolic math library, and is also used 
for machine learning applications such as neural networks. There are lots of versions and builds of 
TensorFlow available via Conda (the output of `conda search tensorflow` is too long to share here!). 

How do you decide which version is the “correct” version? How to make sure that you get a build 
that includes GPU support? At this point you have seen all the Conda "tricks" required  to solve 
this one yourself!

> ## Create an `environment.yml` file for TensorFlow
> 
> In this exercise you will create a Conda environment for TensorFlow. Important CUDA depedencies 
> of TensorFlow are the CUDA Toolkit, cuDNN, and 
> [CUPTI](https://docs.nvidia.com/cuda/cupti/index.html). Don't forget that if you want to train 
> on more than one GPU, then your environment will need also need NCCL and an MPI implementation.
> 
> > ## Solution
> > 
> > Use the `tensorflow-gpu` meta-package to select the appropriate version and build of TensorFlow for 
> > your OS; use [`mpi4py`](https://mpi4py.readthedocs.io/en/stable/) to get a CUDA-aware OpenMPI build.
> > 
> > ~~~
> > name: null
> > 
> > channels:
> >   - conda-forge
> >   - defaults
> > 
> > dependencies:
> >   - cudatoolkit=10.1
> >   - cudnn=7.6
> >   - cupti=10.1
> >   - mpi4py=3.0 # installs cuda-aware openmpi
> >   - nccl=2.4
> >   - pip=20.0
> >   - python=3.7
> >   - tensorflow-gpu=2.1 # installs tensorflow=2.1=gpu_py37h7a4bb67_0
> > ~~~
> > {: .language-yaml}
> {: .solution} 
{: .challenge}

## NVIDIA RAPIDS (+BlazingSQL+Datashader)

As a final example let's see how to get started with 
[NVIDIA RAPIDS](https://rapids.ai/) (and friends!). NVIDIA RAPIDS is a suite of open source 
software libraries and APIs gives you the ability to execute end-to-end data science and 
analytics pipelines entirely on GPUs (think [Pandas](https://pandas.pydata.org/) + 
[Scikit-learn](https://scikit-learn.org/stable/index.html) but for GPUs instead of CPUs). In 
addition to RAPIDS we have also included [BlazingSQL](https://www.blazingsql.com/) in this 
example environment file. BlazingSQL is an open-source SQL interface to extract-transform-load 
(ETL) massive datasets directly into GPU memory for analysis using NVIDIA RAPIDS. The environment 
also includes [Datashader](https://datashader.org/), a graphics pipeline system for creating 
meaningful representations of large datasets quickly and flexibly, that can be accelerated with 
GPUs. If you are going to do all of your data analysis on the GPU, then you might as well do your 
data visualization on the GPU too!

~~~
name: null 

channels:  
  - blazingsql
  - rapidsai
  - nvidia
  - conda-forge
  - defaults

dependencies:  
  - blazingsql=0.13
  - cudatoolkit=10.1
  - datashader=0.10
  - pip=20.0
  - python=3.7
  - rapids=0.13
~~~
{: .language-yaml}

> ## What exactly is installed when you install NVIDIA RAPIDS?
> 
> Create an NVIDIA RAPIDS environment using the Conda environment file above. Use Conda commands 
> to inspect the complete list of packages that have been installed. Do you recognize any of the 
> packages?  
> 
> > ## Solution
> > 
> > Use the following commands to create the environment.
> > 
> > ~~~
> > $ mkdir nvidia-rapids-project
> > $ cd nvidia-rapids-project/
> > $ nano environment.yml # copy-paste the environment file contents
> > $ conda env create --prefix ./env --file environment.yml
> > ~~~
> > {: .language-bash}
> > 
> > Use the following commands to activate the environment and list all the packages installed.
> > 
> > ~~~
> > $ conda activate ./env
> > $ conda list
> > ~~~
> > {: .language-bash}
> {: .solution} 
{: .challenge}

# But what if I need the NVIDIA CUDA Compiler?

Way up at the beginning of this episode I mention that the versions of the NVIDIA CUDA Toolkit 
available via the default channels did not include the NVIDIA CUDA Compiler (NVCC). However, if 
your data science project has dependencies that require compiling custom CUDA extensions then you 
will almost surely need the NVIDIA CUDA Compiler (NVCC).

How do you know that your project dependencies need NVCC? Most likely you will try the standard 
approaches above and Conda will fail to successfully create the environment and perhaps throw a 
bunch of compiler errors. Once you know that you need NVCC, there are a couple of ways to get the 
NVCC compiler installed.

## First, try the `cudatoolkit-dev` package...

The cudatoolkit-dev package available from the conda-forge channel includes GPU-accelerated 
libraries, debugging and optimization tools, a C/C++ compiler and a runtime library. This package 
consists of a post-install script that downloads and installs the full CUDA toolkit (NVCC compiler 
and libraries, but not the exception of CUDA drivers).

While the cudatoolkit-dev packages available from conda-forge do include NVCC, I have had 
difficult getting these packages to consistently install properly.

* Some of the available builds require manual intervention to accept license agreements making 
  these builds unsuitable for installing on remote systems (which is critical functionality).
* Some other builds seems to work on Ubuntu but not on other flavors of Linux such as CentOS.
* Other Conda packages that depend on CUDA will often install the cudatoolkit package even though 
  everything included in this package will have already been installed via cudatoolkit-dev.

That said, it is always worth trying the `cudatoolkit-dev` approach first. Maybe it will work for 
your use case. Here is an example that uses NVCC installed via the cudatoolkit-dev package to 
compile custom extensions from [PyTorch Cluster](https://github.com/rusty1s/pytorch_cluster), a 
small extension library of highly optimized graph cluster algorithms for the use with 
[PyTorch](https://pytorch.org/).

~~~
name: null

channels:
  - pytorch
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit-dev=10.1
  - cxx-compiler=1.0
  - matplotlib=3.2
  - networkx=2.4
  - numba=0.48
  - pandas=1.0
  - pip=20.0
  - pip:
    - -r file:requirements.txt
  - python=3.7
  - pytorch=1.4
  - scikit-image=0.16
  - scikit-learn=0.22
  - tensorboard=2.1
  - torchvision=0.5
~~~
{: .language-yaml}

The `requirements.txt` file referenced above contains PyTorch Cluster and related packages such as 
[PyTorch Geometric](https://github.com/rusty1s/pytorch_geometric). Here is what that file looks 
like.

~~~
torch-scatter==2.0.*
torch-sparse==0.6.*
torch-spline-conv==1.2.*
torch-cluster==1.5.*
torch-geometric==1.4.*

# make sure the following are re-compiled if environment is re-built
--no-binary=torch-scatter
--no-binary=torch-sparse
--no-binary=torch-spline-conv
--no-binary=torch-cluster
--no-binary=torch-geometric
~~~

The use of the `--no-binary` option here insures that the packages with custom extensions will be 
re-built whenever the environment is re-built which helps increase the reproducibility of the 
environment build process when porting from workstations to remote clusters that might have 
different OS.

## ...if that doesn't work, then use the `nvcc_linux-64` meta-package

The most robust approach to obtain NVCC and still use Conda to manage all the other dependencies 
is to install the NVIDIA CUDA Toolkit on your system and then install a meta-package nvcc_linux-64 
from conda-forge which configures your Conda environment to use the NVCC installed on your system 
together with the other CUDA Toolkit components installed inside the Conda environment. While we 
have found this approach to be more robust then relying on `cudatoolkit-dev`, this approach is more 
involved as it requires installing a 
[particular version](https://developer.nvidia.com/cuda-toolkit-archive) of the NVIDIA CUDA Toolkit 
on your system first.

In order to demonstrate how to use the `nvcc_linux-64` meta-package approach we will show how to 
build a Conda environment for deep learning projects that use Horovod to enable distributed 
training across multiple GPUs (either on the same node or spread across multiple nodes). 

[Horovod](https://github.com/horovod/horovod) is an open-source distributed training framework for 
[TensorFlow](https://www.tensorflow.org/), [Keras](https://keras.io/), 
[PyTorch](https://pytorch.org/), and [Apache MXNet](https://mxnet.incubator.apache.org/). 
Originally developed by Uber for in-house use, Horovod was open sourced a couple of years ago and 
is now an official [Linux Foundation AI (LFAI)](https://lfai.foundation/) project.

### Typical `environment.yml` file

Let's checkout the `environment.yml` file. You can find this `environment.yml` file on GitHub as a 
part of a [template repository](https://github.com/kaust-vislab/horovod-gpu-data-science-project) to 
help you get started with Horovod. 

~~~
name: null

channels:
  - pytorch
  - conda-forge
  - defaults

dependencies:
  - bokeh=1.4
  - cmake=3.16 # insures that Gloo library extensions will be built
  - cudnn=7.6
  - cupti=10.1
  - cxx-compiler=1.0 # insures C and C++ compilers are available
  - jupyterlab=1.2
  - mpi4py=3.0 # installs cuda-aware openmpi
  - nccl=2.5
  - nodejs=13
  - nvcc_linux-64=10.1 # configures environment to be "cuda-aware"
  - pip=20.0
  - pip:
    - mxnet-cu101mkl==1.6.* # MXNET is installed prior to horovod
    - -r file:requirements.txt
  - python=3.7
  - pytorch=1.4
  - tensorboard=2.1
  - tensorflow-gpu=2.1
  - torchvision=0.5
~~~
{: .language-yaml}

Take note of the channel priorities: `pytorch` is given highest priority in order to insure that 
the official PyTorch binary is installed (and not the binaries available on `conda-forge`). There 
are also a few things worth noting about the dependencies. Even though you have installed the 
NVIDIA CUDA Toolkit manually you can still use Conda to manage the other required CUDA components 
such as `cudnn` and `nccl` (and the optional `cupti`).

* We use two meta-packages, `cxx-compiler` and `nvcc_linux-64`, to make sure that suitable C, and C++ 
  compilers are installed and that the resulting Conda environment is aware of the manually installed 
  CUDA Toolkit.
* Horovod requires some controller library to coordinate work between the various Horovod processes. 
  Typically this will be some MPI implementation such as OpenMPI. However, rather than specifying 
  the `openmpi` package directly, opt for `mpi4py` Conda package which will install a CUDA-aware 
  build of OpenMPI (assuming it is supported by your hardware).
* Horovod also supports the Gloo collective communications library that can be used in place of MPI. 
  If you include `cmake` in order to insure that the Horovod extensions for Gloo are built.

### Typical `requirements.txt` file

The `requirements.txt` file is where all of the dependencies, including Horovod itself, are 
listed for installation via `pip`. In addition to Horovod, it is recommended to use `pip` to install 
JupyterLab extensions to enable GPU and CPU resource monitoring via 
[`jupyterlab-nvdashboard`](https://github.com/rapidsai/jupyterlab-nvdashboard) and 
[Tensorboard](https://www.tensorflow.org/tensorboard/) support via 
[`jupyter-tensorboard`](https://github.com/chaoleili/jupyterlab_tensorboard). Note the use of the 
`--no-binary` option at the end of the file. Including this option insures that Horovod will be 
re-built whenever the Conda environment is re-built.

~~~
horovod==0.19.*
jupyterlab-nvdashboard==0.2.*
jupyter-tensorboard==0.2.*

# make sure horovod is re-compiled if environment is re-built
--no-binary=horovod
~~~

### Creating the Conda environment

You can create the Conda environment in a sub-directory `env` of your project directory by 
running the following commands. By default Horovod will try and build extensions for all 
detected frameworks. See the [Horovod documentation](https://horovod.readthedocs.io/en/stable/) on 
for the details on additional environment variables that can be set prior to building Horovod.

~~~
export ENV_PREFIX=$PWD/env
export HOROVOD_CUDA_HOME=$CUDA_HOME
export HOROVOD_NCCL_HOME=$ENV_PREFIX
export HOROVOD_GPU_OPERATIONS=NCCL
conda env create --prefix $ENV_PREFIX --file environment.yml --force
~~~
{: .language-bash}

> ## Wrap complex Conda environment builds in a script!
> 
> In order to enhance reproducibiity of your complex Conda build, I typically wrap commands into a 
> shell script called `create-conda-env.sh`. Running the shell script will set the Horovod build 
> variables, create the Conda environment, activate the Conda environment, and build JupyterLab 
> with any additional extensions as specified in a `postBuild` script.
> 
> ~~~
> #!/bin/bash --login
> 
> set -e
> 
> export ENV_PREFIX=$PWD/env
> export HOROVOD_CUDA_HOME=$CUDA_HOME
> export HOROVOD_NCCL_HOME=$ENV_PREFIX
> export HOROVOD_GPU_OPERATIONS=NCCL
> conda env create --prefix $ENV_PREFIX --file environment.yml --force
> conda activate $ENV_PREFIX
> . postBuild
> ~~~
> {: .language-bash}
> 
> You can put scripts inside a `bin` directory in my project root directory. The script should 
> be run from the project root directory as follows.
> ~~~
> $ ./bin/create-conda-env.sh
> ~~~
> {: .language-bash}
{: .callout}

We covered a lot of ground in this episode! We showed you how to use `conda search` to see which 
versions of the NVIDIA CUDA Toolkit and related libraries such as NCCL and cuDNN were available 
via Conda. Then we walked you through example Conda environment files for several popular data 
science frameworks that can use GPUs. We wrapped up with a discussion of two different approaches 
for getting NVCC when your project requires compiler custom CUDA extensions. Hopefully these ideas 
will help you make the jump from CPUs to GPUs on your next data science project!