---
title: "Combining Conda with Docker"
teaching: 45
exercises: 15
questions:
- "Why would I want to combine Conda with Docker?"
- "How do I combine Conda with Docker?"
objectives:
- "Understand how to write a `Dockerfile` that injects a user provided Conda environment into a Docker image."
keypoints:
- If you have a production use-case, then consider using [jupyter-repo2-docker](https://repo2docker.readthedocs.io/en/latest/`.
---

## Why not just use Conda (+ pip)?

While Conda (+ pip) solves most of my day-to-day data science environment and package management issues, incorporating Docker into my Conda (+ pip) development workflow has made it much easier to port my data science workflows from my laptop/workstation to remote cloud computing resources. Incorporating Docker into my development workflow has also made my work more reproducible by removing non-obvious OS-level dependencies (particularly when moving from local laptops/workstations running Mac OSX and Windows to remote servers running Linux).
However, getting Conda (+ pip) to work as expected inside Docker containers turned out to be much more challenging than I anticipated. Most of the difficulties I encountered involved getting the environment to activate properly inside the image so that the UX when using Conda (+ pip) inside a container was identical to the UX when using Conda (+ pip) outside a container.
All these difficulties could be solved by getting the Dockerfile “just right”. In the next section I will walk you step-by-step through the Dockerfile that I developed.

## Writing the Dockerfile
The trick to getting Conda (+ pip) and Docker to work smoothly together is to write a good Dockerfile. In this section I will take you step by step through the various pieces of the Dockerfile that I developed. Hopefully you can use this Dockerfile without modification on you next data science project.
From here onwards, I assume that you have organized your project directory similar to my Python data science project template. In particular, I will assume that you store all Docker related files, in particular, the Dockerfile inside a docker sub-directory within your project root directory.

### Use a standard base image

Every Dockefile has a base or parent image. For the parent image I use Ubuntu 16.04 which is one of the most commonly used flavor of Linux in the data science community (and also happens to be the same OS installed on my workstation).

```
FROM ubuntu:16.04
```

### Make bash the default shell

The default shell used to run Dockerfile commands when building Docker images is /bin/sh. Unfortunately /bin/sh is currently not one of the shells supported by the conda init command. Fortunately it is possible to change the default shell used to run Dockerfile commands using the SHELL instruction.

```
SHELL [ "/bin/bash", "--login", "-c" ]
```

Note the use of the --login flag which ensures that both ~/.profile and ~/.bashrc are sourced properly. Properly sourcing both ~/.profile and ~/.bashrc is necessary in order to use various conda commands to build the Conda environment inside the Docker image.

### Create a non-root user

It is a Docker security “best practice” to create a non-root user inside your Docker images. My preferred approach to creating a non-root user uses build arguments to customize the username, uid, and gid of the non-root user. I use standard defaults for the uid and gid; the default username is set to al-khawarizmi (in honor of the famous Persian polymath whose name is on the building where I work)

```
# Create a non-root user
ARG username=al-khawarizmi
ARG uid=1000
ARG gid=100
ENV USER $username
ENV UID $uid
ENV GID $gid
ENV HOME /home/$USER
RUN adduser --disabled-password \
    --gecos "Non-root user" \
    --uid $UID \
    --gid $GID \
    --home $HOME \
    $USER
```

### Copy over the config files

After creating the non-root user I copy over all of the config files that I will need to create the Conda environment (i.e., environment.yml, requirements.txt, postBuild). I also copy over a Bash script that I will use as the Docker ENTRYPOINT (more on this below).

```
COPY environment.yml requirements.txt /tmp/
RUN chown $UID:$GID /tmp/environment.yml /tmp/requirements.txt
COPY postBuild /usr/local/bin/postBuild.sh
RUN chown $UID:$GID /usr/local/bin/postBuild.sh && \
    chmod u+x /usr/local/bin/postBuild.sh
COPY docker/entrypoint.sh /usr/local/bin/
RUN chown $UID:$GID /usr/local/bin/entrypoint.sh && \
    chmod u+x /usr/local/bin/entrypoint.sh
```

Newer versions of Docker support copying files as a non-root user, however the version of Docker available on DockerHub does not yet support copying as a non-root user so if you want to set up automated builds for your Git repositories you will need to copy everything as root.

### Install Miniconda as the non-root user.

After copying over the config files as root, I switch over to the non-root user and install Miniconda.

```
USER $USER
# install miniconda
ENV MINICONDA_VERSION 4.8.2
ENV CONDA_DIR $HOME/miniconda3
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-$MINICONDA_VERSION-Linux-x86_64.sh -O ~/miniconda.sh && \
    chmod +x ~/miniconda.sh && \
    ~/miniconda.sh -b -p $CONDA_DIR && \
    rm ~/miniconda.sh

# make non-activate conda commands available
ENV PATH=$CONDA_DIR/bin:$PATH

# make conda activate command available from /bin/bash --login shells
RUN echo ". $CONDA_DIR/etc/profile.d/conda.sh" >> ~/.profile

# make conda activate command available from /bin/bash --interative shells
RUN conda init bash
```

### Create a project directory

Next I create a project directory inside the non-root user home directory. The Conda environment will be created in a env sub-directory inside the project directory and all other project files and directories can then be mounted into this directory.

```
# create a project directory inside user home
ENV PROJECT_DIR $HOME/app
RUN mkdir $PROJECT_DIR
WORKDIR $PROJECT_DIR
```

### Build the Conda environment

Now I am ready to build the Conda environment. Note that I can use nearly the same sequence of conda commands that I would use to build a Conda environment for a project on my laptop or workstation.

```
# build the conda environment
ENV ENV_PREFIX $PWD/env
RUN conda update --name base --channel defaults conda && \
    conda env create --prefix $ENV_PREFIX --file /tmp/environment.yml --force && \
    conda clean --all --yes
# run the postBuild script to install any JupyterLab extensions
RUN conda activate $ENV_PREFIX && \
    /usr/local/bin/postBuild.sh && \
    conda deactivate
```

### Insure Conda environment is activated at runtime

Almost finished! Second to last step is to use an ENTRYPOINT script to ensure that the Conda environment is properly activated at runtime.

```
ENTRYPOINT [ "/usr/local/bin/entrypoint.sh" ]
```

Here is the /usr/local/bin/entrypoint.sh script for reference.

```
#!/bin/bash --login
set -e
conda activate $ENV_PREFIX
exec "$@"
```

### Specify a default command for the Docker container

Finally, I use the CMD instruction to specify a default command to run when a Docker container is launched. Since I install JupyerLab in all of my Conda environments I tend to launch a JupyterLab server by default when executing containers.

```
# default command will launch JupyterLab server for development
CMD [ "jupyter", "lab", "--no-browser", "--ip", "0.0.0.0" ]
```

## Building the Docker image

The following command (which should be run from within the docker sub-directory containing the Dockefile) builds a new image for your project with a custom $USER (and associated $UID and $GID) as well as a particular $IMAGE_NAME and $IMAGE_TAG. This command should be run within the docker sub-directory of the project as the Docker build context is set to ../ which should be the project root directory.

```
docker image build \
  --build-arg username=$USER \
  --build-arg uid=$UID \
  --build-arg gid=$GID \
  --file Dockerfile \
  --tag $IMAGE_NAME:$IMAGE_TAG \
  ../
```

## Running a Docker container

Once the image is built, the following command will run a container based on the image `$IMAGE_NAME:$IMAGE_TAG`. This command should be run from within the project's root directory.

```
docker container run \
  --rm \
  --tty \
  --volume ${pwd}/bin:/home/$USER/app/bin \
  --volume ${pwd}/data:/home/$USER/app/data \ 
  --volume ${pwd}/doc:/home/$USER/app/doc \
  --volume ${pwd}/notebooks:/home/$USER/app/notebooks \
  --volume ${pwd}/results:/home/$USER/app/results \
  --volume ${pwd}/src:/home/$USER/app/src \
  --publish 8888:8888 \
  $IMAGE_NAME:$IMAGE_TAG
```

## Using Docker Compose

It is quite easy to make typos whilst writing the above docker commands by hand. A less error-prone approach is to use Docker Compose. The above docker commands can be encapsulated into the docker-compose.yml configuration file as follows.

```
version: "3.7"
services:
  jupyterlab-server:
    build:
      args:
        - username=${USER}
        - uid=${UID}
        - gid=${GID}
      context: ../
      dockerfile: docker/Dockerfile
    ports:
      - "8888:8888"
    volumes:
      - ../bin:/home/${USER}/app/bin
      - ../data:/home/${USER}/app/data
      - ../doc:/home/${USER}/app/doc
      - ../notebooks:/home/${USER}/app/notebooks
      - ../results:/home/${USER}/app/results
      - ../src:/home/${USER}/app/src
    init: true
    stdin_open: true
    tty: true
```

The above docker-compose.yml file relies on variable substitution. to obtain the values for $USER, $UID, and $GID. These values can be stored in an a file called .env as follows.

```
USER=$USER
UID=$UID
GID=$GID
```

You can test your docker-compose.yml file by running the following command in the docker sub-directory of the project.

```
docker-compose config
```

This command takes the `docker-compose.yml` file and substitutes the values provided in the .env file and then returns the result.

Once you are confident that values in the .env file are being substituted properly into the docker-compose.yml file, the following command can be used to bring up a container based on your project's Docker image and launch the JupyterLab server. This command should also be run from within the docker sub-directory of the project.

```
docker-compose up --build
```

When you are done developing and have shut down the JupyterLab server, the following command tears down the networking infrastructure for the running container.

```
docker-compose down
```

## Summary

In this post, I walked through a Dockerfile that injects a Conda (+ pip) environment into a Docker image. I also detailed how to build the resulting image and launch containers using Docker Compose.
If you are looking for a production-quality solution that generalizes the approach outlined above, then I would encourage you to check out [jupyter-repo2docker](https://repo2docker.readthedocs.io/en/latest/).
jupyter-repo2docker is a tool to build, run, and push Docker images from source code repositories. repo2docker fetches a repository (from GitHub, GitLab, Zenodo, Figshare, Dataverse installations, a Git repository or a local directory) and builds a container image in which the code can be executed. The image build process is based on the configuration files found in the repository.
The Conda (+ pip) and Docker combination has significantly increased my data science development velocity while at the same time increasing the portability and reproducibility of my data science workflows.
Hopefully this post can help you combine these three great tools together on your next data science project!
