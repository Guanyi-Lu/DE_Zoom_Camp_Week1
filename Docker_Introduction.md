# Docker Image Overview

## What is a Docker Image?

A Docker image is a lightweight, standalone, and executable package that contains everything needed to run a piece of software, including:

- **Code**: The application or software you want to run.
- **Dependencies**: Required libraries, frameworks, or packages.
- **Configuration**: Environment variables, configuration files, and default settings.
- **Operating System (OS) Files**: Minimal OS components to provide the runtime environment.

## Docker Images and Containers

Docker images are the building blocks for Docker containers.

### Key Concepts of a Docker Image

1. **Immutable**: Docker images are read-only. Once built, they cannot be changed. Any changes are made by creating a new image.
2. **Layers**: Docker images are built in layers. Each layer represents an instruction in the Dockerfile (e.g., `RUN`, `COPY`, `CMD`). This makes them efficient because layers can be reused across images.
3. **Portable**: Docker images can be shared, downloaded, and run on any system that has Docker installed, ensuring consistent behavior across environments.

## How is a Docker Image Created?

You can create a Docker image using a **Dockerfile**, which is a text file with instructions for building the image.

## Why Are Docker Images Useful?

1. **Consistency**: Ensures that applications run the same way regardless of the underlying environment.
2. **Reusability**: Layers and images can be reused across projects, saving time and storage.
3. **Portability**: Easy to share and run on any system with Docker installed.
4. **Scalability**: Quickly deploy multiple containers from the same image in cloud environments.

---

## Example Commands

### 1. Running Ubuntu in Interactive Mode

```bash
$ docker run -it ubuntu bash
```
docker run: This tells Docker to start a container from an image.

-it: Ensures you can interact with the container in real time.

ubuntu: Specifies the Docker image to use. Here, it’s the official Ubuntu Linux image.

bash: Bash is a shell (command-line interface) used to interact with the operating system. Running bash opens an interactive session where you can type Linux commands.

This command opens an interactive bash shell inside the container. You can then type commands (like ls, cd, or apt-get) that are typical of Linux systems.

### 2. Running Python 3.9 in Interactive Mode


```bash

$ docker run -it python:3.9
```
This command downloads the Python 3.9 image to your local Docker environment (not your system, like a typical installation).

It then runs a container with Python 3.9 inside it, isolated from your host system.

Installing Python Packages in Bash

Step 1: Start the container in bash mode:

```bash
$ docker run -it --entrypoint=bash python:3.9
```
What Does This Command Do?

Starts a Python 3.9 container but runs the bash shell instead of Python.
Opens an interactive terminal inside the container, allowing you to type any commands you like within the container's environment.
This is useful if you want to interact with the container, inspect its file system, or install additional software before running Python code.
Step 2: Install Python packages like pandas:

```bash
pip install pandas
```
Creating a Dockerfile
Here’s an example of a simple Dockerfile to create an image with Python 3.9 and pandas installed:

### what is in Dockerfile
```bash

FROM python:3.9
RUN pip install pandas
ENTRYPOINT ["bash"]
```

Explanation of Each Line:
```bash
FROM python:3.9
```
#### What it does: Specifies the base image for your container.
Explanation: Docker images are built on top of other images. The python:3.9 image is an official image that has Python 3.9 pre-installed. This line tells Docker to start building the image from the Python 3.9 base image.

```bash
RUN pip install pandas
```
#### What it does: Installs the pandas library inside the container during the build process.
Explanation: The RUN command executes commands during the build. In this case, pip install pandas installs the pandas package inside the image, making it available when the container runs.

```bash
ENTRYPOINT ["bash"]
```
#### What it does: Specifies the default command to run when the container starts.
Explanation: ENTRYPOINT specifies the command to run when the container starts. Here, bash is specified, so the container will start with the Bash shell. This is useful for manually interacting with the container instead of running a Python script automatically.

### Building the Docker Image
To build the image using the above Dockerfile, run the following command:

``` bash
$ docker build -t test:my_pandas.
```
Explanation of Each Part:
```bash
docker build:
```
#### This command tells Docker to build a new image based on the instructions in the Dockerfile in the current directory.

```bash
-t test:my_pandas:
```
-t is used to tag the image with a name and optionally a tag.

test:my_pandas is the name and tag of your image:

test is the name of the image.

my_pandas is the tag (e.g., version or variation). By default, if you don’t specify a tag, it uses latest.
```bash
. (period):
```
The period (.) represents the current directory. Docker will look for the Dockerfile in the current directory to build the image.
