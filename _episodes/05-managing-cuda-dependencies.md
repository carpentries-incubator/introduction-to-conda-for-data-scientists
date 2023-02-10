---
title: "Managing packages with GPU dependencies"
teaching: 45
exercises: 15
questions:
- "What packages have GPU dependencies?"
objectives:
- "Show how to use Conda to manage key GPU dependencies for you next (data) science project."
- "Install `pytorch` with GPU support."
- "Install `TensorFlow` with GPU support."
keypoints:
- "Conda can be used to manage your key GPU dependencies."
- "Conda can be used to install `pytorch` and `TensorFlow`."
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

In this episode we are going to see how to manage project specific versions of the NVIDIA packages using Conda.

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

If you are interested in deep learning, then you may need to get your hands on cuDNN. Various 
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

# Environments with `pytorch` and `TensorFlow`

Both `pytorch` and `TensorFlow` provide installation options using Conda.

## PyTorch

[PyTorch](https://pytorch.org/) is an open source machine learning library based on the Torch 
library, used for applications such as computer vision and natural language processing. It is 
primarily developed by Facebook’s AI Research lab.

PyTorch has a [handy tool to help you figure out what commands to run](https://pytorch.org/) to install on your platform. First, we'll make an empty Conda environment and activate it:

```
conda create --name pytorch-with-gpu
conda activate pytorch-with-gpu
```

Then we'll run the command recommended for our platform e.g. Windows 10:

```
conda install pytorch torchvision torchaudio pytorch-cuda=11.7 -c pytorch -c nvidia
```

From what we've learned, we can see that the **pytorch** channel will be used at the
first priority, then the **nvidia** channel.

We can check what packages have been installed using:

```
conda env export
```

We might want to check this has worked by running some Python code to import the `torch` module
and output to the screen the results of a check to see if CUDA is working:

```
python -c "import torch; print(torch.cuda.is_available())"
```

## TensorFlow

[TensorFlow](https://www.TensorFlow.org/) is a free, open-source software library for dataflow and 
differentiable programming across a range of tasks. It is a symbolic math library, and is also used 
for machine learning applications such as neural networks.

TensorFlow [recommend using `pip` to install](https://www.TensorFlow.org/install/pip), even if you're using Conda.

As with pytorch, we'll make and activate a new environment:

```
conda create --name tf-with-gpu
conda activate tf-with-gpu
```

Then follow the instructions to install TensorFlow e.g. on Windows 10:

```
conda install -c conda-forge cudatoolkit=11.2 cudnn=8.1.0
# Anything above 2.10 is not supported on the GPU on Windows Native
python3 -m pip install "tensorflow<2.11"
```

Note the lack of support for recent versions on Windows. The [Windows Subsystem for Linux](https://learn.microsoft.com/en-gb/windows/wsl/install)
is a good choice for installing later versions.

We might want to check this has workedby running some Python code to import the `tensorflow` module
and output to the screen the results of a check to see if CUDA is working by listing available GPU
devices:

```
python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"
```

> ## Environment variables
> Your install instructions might include a line like this:
> 
> ```
> export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:$CONDA_PREFIX/lib/
> ```
> 
> This line sets an "environment variable", which is a way of making a small piece of
> information available to your software. These are often set for the entire operating system,
>  but Conda lets us set them differently in different environments. 
{: .callout}
