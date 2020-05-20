---
title: "Managing CUDA dependencies with Conda"
teaching: 30
exercises: 15
questions:
- "Which NVIDIA libraries are available via Conda?"
objectives:
- "???"
keypoints:
- "???"
---

Transitioning your data science projects from CPU to GPU can seem like a daunting task. 
In particular, there is quite a bit of unfamiliar additional software, such as NVIDIA CUDA Toolkit, 
NVIDIA Collective Communications Library (NCCL), and NVIDIA Deep Neural Network Library (cuDNN) to 
download and install.

If you go to the NVIDIA developer website you will find loads of documentation and instructions 
for how to install these libraries system wide. But then what do you do if you need different 
versions of these new libraries for different projects? You could install a bunch of different 
versions of NVIDIA CUDA Toolkit, NCCL, and cuDNN system wide and then use environment variables to 
control the “active” versions for each project but this is cumbersome and error prone. Fortunately 
there are better ways!

In this episode we are going to see how to manage project specific versions of the NVIDIA CUDA 
Toolkit, NCCL, and cuDNN using Conda. I will assume a basic familiarity with Conda so if you 
haven’t hear of Conda or are just getting started I would encourage you to read my other articles 
Getting started with Conda, and Managing your data science projects with Conda.

# Are NVIDIA libraries available via Conda?

Yep! You can use the conda search command to see what versions of the NVIDIA CUDA Toolkit are 
available from the default channels.

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
interested in confirming this you can run the commandconda search --channel nvidia cudatoolkit and 
then compare the build numbers with those listed above from the default channels.

An important limitation of the versions of the NVIDIA CUDA Toolkit that are available from the 
either the default or NVIDIA Conda channels is that they do not include the NVIDIA CUDA Compiler 
(NVCC).

## What about cuDNN?

You will also find various versions of cuDNN available from the default channels.

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

Not to worry! Conda Forge to the rescue. Conda Forge is a community-led collection of recipes, 
build infrastructure and distributions for the Conda package manager. I always check the 
`conda-forge` channel when I can’t find something I need available on the default channels.

~~~
$ conda search -c conda-forge nccl
Loading channels: done
# Name                       Version           Build  Channel
nccl                           1.3.5      cuda10.0_0  pkgs/main
nccl                           1.3.5       cuda9.0_0  pkgs/main
nccl                           1.3.5       cuda9.2_0  pkgs/main
nccl                         2.4.6.1      h51cf6c1_0  conda-forge
nccl                         2.4.6.1      h7cc98d6_0  conda-forge
nccl                         2.4.6.1      hc6a2c23_0  conda-forge
nccl                         2.4.6.1      hd6f8bf8_0  conda-forge
nccl                         2.4.7.1      h51cf6c1_0  conda-forge
nccl                         2.4.7.1      h7cc98d6_0  conda-forge
nccl                         2.4.7.1      hd6f8bf8_0  conda-forge
nccl                         2.4.8.1      h51cf6c1_0  conda-forge
nccl                         2.4.8.1      h51cf6c1_1  conda-forge
nccl                         2.4.8.1      h7cc98d6_0  conda-forge
nccl                         2.4.8.1      h7cc98d6_1  conda-forge
nccl                         2.4.8.1      hd6f8bf8_0  conda-forge
nccl                         2.4.8.1      hd6f8bf8_1  conda-forge
nccl                         2.5.6.1      h51cf6c1_0  conda-forge
nccl                         2.5.6.1      h7cc98d6_0  conda-forge
nccl                         2.5.6.1      hc6a2c23_0  conda-forge
nccl                         2.5.6.1      hd6f8bf8_0  conda-forge
nccl                         2.5.7.1      h51cf6c1_0  conda-forge
nccl                         2.5.7.1      h7cc98d6_0  conda-forge
nccl                         2.5.7.1      hc6a2c23_0  conda-forge
nccl                         2.5.7.1      hd6f8bf8_0  conda-forge
nccl                         2.6.4.1      h51cf6c1_0  conda-forge
nccl                         2.6.4.1      h7cc98d6_0  conda-forge
nccl                         2.6.4.1      hc6a2c23_0  conda-forge
nccl                         2.6.4.1      hd6f8bf8_0  conda-forge
~~~
{: .language-bash}

# Some example Conda environment files

Now that you know how to figure out which versions of the various NVIDIA CUDA libraries are 
available on which channels you are ready to write your environment.yml file. In this section I 
will provide some example Conda environment files for PyTorch, TensorFlow, and NVIDIA RAPIDS to 
help get you started on your next GPU data science project.

## PyTorch

PyTorch is an open source machine learning library based on the Torch library, used for 
applications such as computer vision and natural language processing. It is primarily developed by 
Facebook’s AI Research lab. Conda is actually the recommended way to install PyTorch.

~~~
name: null

channels:
  - pytorch
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit=10.1
  - mpi4py=3.0 # installs cuda-aware openmpi
  - pip=20.0
  - python=3.7
  - pytorch=1.5
  - torchvision=0.6
~~~

The official PyTorch binary ships with NCCL and cuDNN so it is not necessary to include these 
libraries in your environment.yml file (unless some other package also needs these libraries!). 
Also take note of the channel priorities: the official `pytorch` channel must be given priority 
over `conda-forge` in order to insure that the official PyTorch binaries (the ones that include 
NCCL and cuDNN) will be installed (otherwise you will get some unofficial version of PyTorch 
available on `conda-forge`).

## TensorFlow example

TensorFlow is a free, open-source software library for dataflow and differentiable programming 
across a range of tasks. It is a symbolic math library, and is also used for machine learning 
applications such as neural networks.

There are lots of versions and builds of TensorFlow available via Conda (the output of 
`conda search tensorflow` is too long to share here!). How do you decide which version is the 
“correct” version? How to make sure that you get a build that includes GPU support? Use the 
`tensorflow-gpu` meta-package to select the appropriate version and build of TensorFlow for 
your OS.

~~~
name: null

channels:
  - conda-forge
  - defaults

dependencies:
  - cudatoolkit=10.1
  - cudnn=7.6
  - cupti=10.1
  - mpi4py=3.0 # installs cuda-aware openmpi
  - nccl=2.4
  - pip=20.0
  - python=3.7
  - tensorflow-gpu=2.1 # installs tensorflow=2.1=gpu_py37h7a4bb67_0
~~~

## BlazingSQL + NVIDIA RAPIDS example

As a final example I want to show you how to get started with NVIDIA RAPIDS (and friends!). NVIDIA 
RAPIDS is a suite of open source software libraries and APIs gives you the ability to execute 
end-to-end data science and analytics pipelines entirely on GPUs (think Pandas + Scikit-learn but 
for GPUs instead of CPUs).

I have also included BlazingSQL in this example environment file. BlazingSQL is an open-source SQL 
interface to extract-transform-load (ETL) massive datasets directly into GPU memory for analysis 
using NVIDIA RAPIDS. Installing BlazingSQL so that it works properly with NVIDIA RAPIDS was a bit 
trickier than most Conda package installs so I decided to include a working example here.

Finally, the environment above also includes Datashader, a graphics pipeline system for creating 
meaningful representations of large datasets quickly and flexibly, that can be accelerated with 
GPUs. If you are going to do all of your data analysis on the GPU you might as well do your data 
visualization on the GPU too.

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

# But what if I need the NVIDIA CUDA Compiler?

Way up at the beginning of this episode I mention that the versions of the NVIDIA CUDA Toolkit 
available via the default channels did not include the NVIDIA CUDA Compiler (NVCC). However, if 
your data science project has dependencies that require compiling custom CUDA extensions then you 
will almost surely need the NVIDIA CUDA Compiler (NVCC).

How do you know that your project dependencies need NVCC? Most likely you will try the standard 
approaches above and Conda will fail to successfully create the environment and perhaps throw a 
bunch of compiler errors. Once you know that you need NVCC, there are a couple of ways to get the 
NVCC compiler installed.

## Try the cudatoolkit-dev package

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
compile custom extensions from PyTorch Cluster, a small extension library of highly optimized graph 
cluster algorithms for the use with PyTorch.

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

The requirements.txt file referenced above contains PyTorch Cluster and related packages such as 
PyTorch Geometric. Here is what that file looks like.

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

## In general go with the nvcc_linux-64 meta-package

The most robust approach to obtain NVCC and still use Conda to manage all the other dependencies 
is to install the NVIDIA CUDA Toolkit on your system and then install a meta-package nvcc_linux-64 
from conda-forge which configures your Conda environment to use the NVCC installed on your system 
together with the other CUDA Toolkit components installed inside the Conda environment. While I 
have found this approach to be more robust then relying on cudatoolkit-dev, this approach is more 
involved as it requires installing a particular version of the NVIDIA CUDA Toolkit on your system 
first.

To see a fully worked out example of this approach take a look at my recent article Building a 
Conda environment for Horovod.

# Summary

We covered a lot of ground in this post. We showed you how to use `conda search` to see which 
versions of the NVIDIA CUDA Toolkit and related libraries such as NCCL and cuDNN were available 
via Conda. Then we walked you through example Conda environment files for several popular data 
science frameworks that can use GPUs. Hopefully these ideas will help you make the jump from CPUs 
to GPUs on your next data science project!