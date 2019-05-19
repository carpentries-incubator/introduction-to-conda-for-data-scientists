---
title: "Using Packages and Channels"
teaching: 15
exercises: 15
questions:
- "What are Conda packages?"
- "What are Conda channels?"
- "Why should I be explicit about which channels my research project uses?"
objectives:
- "Search for a package on currently configured channels."
- "Install a package from a specific channel."
- "Install a package by exact version number."
- "Install a package where version number satisfies certain constraints."
- "Add channels to your Conda configuration."
keypoints:
- "First key point. Brief Answer to questions. (FIXME)"
- "A Conda channel is a URL to a directory containing a Conda package(s)."
- "Explicitly including the channels (and their priority!) in a project's environment file is necessary for another research to completely re-create that project's software environment." 
---

## What are Conda packages?

From the 
[Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/concepts/packages.html#what-is-a-conda-package)...

A conda package is a compressed tarball file (.tar.bz2) that contains:

* system-level libraries
* Python or other modules
* executable programs and other components
* metadata under the `info/` directory
* a collection of files that are installed directly into an `install` prefix

## What are Conda channels?
Again from the [Conda documentation](https://conda.io/projects/conda/en/latest/user-guide/concepts/channels.html#what-is-a-conda-channel)

{% include links.md %}

