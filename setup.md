---
layout: page
title: "Setup"
permalink: /setup/
root: ..
---
## Installation Instructions

### Install Git

First you need to install [Git](https://git-scm.com/). Instructions for installing Git depend (slightly!)
 on your operating system and can be found in the [Introduction to Git for (Data) Scientists](https://
 github.com/kaust-vislab/introduction-to-git-for-data-scientists#installation-instructions) repository. 

### Install Conda

Next check whether the [Conda](https://docs.conda.io/en/latest/) package management system is installed on your local machine by running the following command in a terminal.

~~~
$ which conda
~~~
{: .language-bash}

If Conda already exists on your machine, then run the following commands to make sure that you have the most recent version and patches.

~~~
$ conda update -y conda
$ conda init
~~~
{: .language-bash}

If Conda has not been installed on your machine, then install the Python 3 version of [Miniconda](https://docs.conda.io/en/latest/miniconda.html) from Anaconda for your OS. Once you have installed Conda on your machine run the following commands in make sure that you have the most recent version and patches.

~~~
$ conda update -y conda
$ conda init
~~~
{: .language-bash}

{% include links.md %}
