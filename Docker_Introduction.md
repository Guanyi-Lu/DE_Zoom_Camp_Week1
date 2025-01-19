A Docker image is a lightweight, standalone, and executable package that contains everything needed to run a piece of software, including:
•	Code: The application or software you want to run.
•	Dependencies: Required libraries, frameworks, or packages.
•	Configuration: Environment variables, configuration files, and default settings.
•	Operating System (OS) Files: Minimal OS components to provide the runtime environment.
Docker images are the building blocks for Docker containers.
Key Concepts of a Docker Image
1.	Immutable: Docker images are read-only. Once built, they cannot be changed. Any changes are made by creating a new image.
2.	Layers: Docker images are built in layers. Each layer represents an instruction in the Dockerfile (e.g., RUN, COPY, CMD). This makes them efficient because layers can be reused across images.
3.	Portable: Docker images can be shared, downloaded, and run on any system that has Docker installed, ensuring consistent behavior across environments.
How is a Docker Image Created?
You can create a Docker image using a Dockerfile, a text file with instructions for building the image.
Why Are Docker Images Useful?
1.	Consistency: Ensures that applications run the same way regardless of the underlying environment.
2.	Reusability: Layers and images can be reused across projects, saving time and storage.
3.	Portability: Easy to share and run on any system with Docker installed.
4.	Scalability: Quickly deploy multiple containers from the same image in cloud environments.

$docker run -it ubuntu bash:
•	Docker run: This tells Docker to start a container from an image.
•	-it: ensures you can interact with the container in real time
•	Ubuntu: Specifies the Docker image to use. Here, it’s the official Ubuntu Linux image.
•	Bash: bash is a shell (command-line interface) used to interact with the operating system. Running bash opens an interactive session where you can type Linux commands.
When I run this command, it opens an interactive bash shell inside the container, then I can type some commands (like, ls,cd or apt-get) which are typical Linux commands provided by the Ubuntu base image
$ docker run -it python:3.9:
     •  The command downloads the Python 3.9 image to your local Docker environment (not your system like a typical installation).
•  It then runs a container with Python 3.9 inside it, isolated from your host system.

We can download python packages (pandas etc) outside of python environment, such as in bash
Step 1: docker run -it –entrypoint=bash python:3.9
What Does This Command Do?
•	Starts a Python 3.9 container, but instead of running Python, it runs the bash shell in the container.
•	It opens an interactive terminal inside the container, allowing you to type any commands you like within that container's environment.
•	This is useful if you want to interact with the container, inspect its file system, or run other commands (like installing additional software) before starting your Python code.
Step 2: # pip install pandas

Write a docker file:
FROM python:3.9
RUN pip install pandas
ENTRYPOINT [ "bash" ]

1. FROM python:3.9
•	What it does: This line specifies the base image for your container.
•	Explanation: Docker images are built on top of other images. The python:3.9 image is an official image that has Python 3.9 pre-installed. This line tells Docker to start building the image from the Python 3.9 base image, which means the container will already have Python 3.9 installed.
________________________________________
2. RUN pip install pandas
•	What it does: This line runs a command to install the pandas library inside the container during the build process.
•	Explanation:
o	The RUN command executes commands within the image during the build.
o	In this case, pip install pandas installs the pandas package using pip (the Python package manager).
o	This means that once the Docker image is built, pandas will be available for use in the container.
________________________________________
3. ENTRYPOINT [ "bash" ]
•	What it does: This line sets the default command that should be run when the container starts.
•	Explanation:
o	ENTRYPOINT specifies the command to run when the container starts.
o	In this case, bash is specified, which means when you run the container, it will start with the Bash shell.
o	This is useful if you want to interact with the container manually (e.g., running commands in the terminal) rather than automatically running a Python script or other command
Then go back to the git prompt:
docker build -t test:my_pandas .
Explanation of Each Part:
1.	docker build:
o	This command tells Docker to build a new image based on the instructions in the Dockerfile in the current directory.
2.	-t test:my_pandas:
o	-t is used to tag the image with a name and optionally a tag.
o	test:my_pandas is the name and tag of your image:
	test is the name of the image.
	my_pandas is the tag. By default, if you don’t specify a tag, it uses latest. In this case, you’re using my_pandas as the tag.
	Why do we need both name and tag: Name Describes what the image is (e.g., my_app). Tag Specifies the version or variation of the image (e.g., v1, latest, prod).
3.	. (period):
o	The period (.) represents the current directory, meaning Docker will look for the Dockerfile in the current directory to build the image.
