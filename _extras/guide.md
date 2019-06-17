---
title: "Instructor Notes"
---

## Conda on HPC systems

Many HPC systems make either Miniconda or Anaconda (or both!) available to users via 
[Environment Modules](http://modules.sourceforge.net/). In this case a user will need to load the 
relevant module instead of installing Conda.

~~~
$ module load miniconda
~~~
{: .language-bash}

> ## Avoid running the `conda init` command
> 
> Using the `conda init` command is not advisable on HPC systems that use Environment Modules 
> to make Miniconda or Anaconda available to users.  The reason is that running the `conda init` 
> command makes changes to a user's `~/.bashrc` file. If the user subsequently forgets to reverse 
> these changes using `conda init --reverse`, then `conda` will still be available even after 
> unloading or purging the `miniconda` module.  This opens the distinct possibility of a user 
> accidently leaving his/her shell environment in an unexpected state.
{: .callout}    

{% include links.md %}
