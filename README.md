
# 1 - Dependencies management

**[0.5] What is Docker, and how it differs from dependencies management systems? From virtual machines?**

Docker is a software development tool and a virtualization technology that makes it easy to develop, deploy, and manage applications by using containers. A container refers to a lightweight, stand-alone, executable package of a piece of software that contains all the libraries, configuration files, dependencies, and other necessary parts to operate the application. Docker provides the ability to package and run an application in a loosely isolated environment called a container. 
Docker container is not only different sets of software and their dependencies (as packages in dependencies management systems) but an exemplar of an operating system inside which you can build several environments with different sets of software.


**The problems of virtual machines (solved in Docker):**
* There is waste of resources.
*  One VM cannot use resources of another VM.
* OS is required on each VM which needs RAM, Hard disk and license.  
* You have hardware limitations. You cannot create unlimited virtual machines.
Docker uses host OS and VM has it's own OS so it's bigger and slower 


**What are the advantages and disadvantages of using containers over other approaches?**
**Advantages of Docker**
* It is light weight because it does not require any resource pre-allocation (RAM). Whenever it needs resources it acquires them from host OS. It is of less cost.
* Continuous integration efficiency – Docker enables you to build a container image and use that same image across every step of the deployment process.
* It can run on physical hardware, virtual hardware and on cloud.
* You can re-use the image.
* It takes very less time to create.


**Disadvantage of Dockers**
* Docker is not good for application that requires rich GUI
*  It is difficult to manage large amount of containers
*  Docker does not provide cross-platform compatibility means if an application is designed to run in a Docker container on windows, then it cannot run on Linux Docker container
*  Docker is suitable when development OS and testing OS are same
*  It does not provide any solution for data backup and recovery

Docker is great for developing web applications, but if your end-product is a desktop application, then we would suggest you not to use Docker. As it doesn't provide the environment for running the software with a graphical interface, you would need to perform additional workarounds.

**Explain how Docker works: what are Dockerfiles, how are containers created, and how are they run and destroyed**

Docker builds images automatically by reading the instructions from a *Dockerfile* -- a text file that contains all commands, in order, needed to build a given image. A Dockerfile adheres to a specific format and set of instructions.

A Docker image consists of read-only layers each of which represents a Dockerfile instruction. The layers are stacked and each one is a delta of the changes from the previous layer.

*Create container*

Create a container to run it later on with required image.

>docker create --name <container-name> <image-name>

*Run docker container*

Run the docker container with the required image and specified command / process. ‘-d’ flag is used for running the container in background.

>docker run -it -d --name <container-name> <image-name> bash

*Kill container*

We can kill the running container.

>docker kill <container-id/name>

*Destroy container*

Its preferred to destroy container, only if present in stopped state instead of forcefully destroying the running container.

>docker rm <container-id/name>


**Name and describe at least one Docker competitor (i.e., a tool based on the same containerization technology).**
* Podman is a container management product that is ideal if you’re working with or developing your own containers. Podman can create customized versions of containers and share them with other users. 

* Online LXD test tool. LXD is a container and virtual machine management tool. It builds off LXC and, confusingly, also has its own CLI tool, lxc. Both LXC and LXD can replace Docker for some use cases. LXC gives you a virtual machine–like environment but doesn’t include a full kernel. It allows you to run *multiple processes per container*, in contrast to one in Docker.

* Containerd is included as part of Docker but has now been released separately. It provides a layer of abstraction between containers and your system, allowing you to transfer images and manage storage and network connections. Containerd can give you access to low-level functions that make it much easier to use.

* Buildah is maintained by the containers organization responsible for Podman and other projects. You can use it to build containers, and it works well in tandem with Podman. Using Buildah to build containers and Podman to manage them lets you replace Docker’s functionality *without the security risk of having your containers run as root*.

* Kaniko lets you build images from Docker files inside containers or from Kubernetes clusters. That’s useful if you can’t easily or securely run Docker in those locations.

* RunC is another tool that was originally part of Docker but has been released separately and is available on GitHub. It allows you to spawn containers from the command line or to run your own containerization solution, free of the baggage that comes with Docker.

[0.25] What is conda? How it differs from apt, yarn, and others?

Conda is a cross platform package and environment manager that installs and manages conda packages from the Anaconda repository as well as from the Anaconda Cloud. Conda packages are binaries. There is never a need to have compilers available to install them. Additionally conda packages are not limited to Python software. They may also contain C or C++ libraries, R packages or any other software.

This highlights a key difference between conda and pip. Pip installs Python packages whereas conda installs packages which may contain software written in any language. 




**Anaconda:**
*This part I have made on colab notebook*
[1] Install conda, create a new virtual environment, and install all necessary packages.
[0.75] You won't be able to install some tools - that's fine. List their names, and explain what should be done to make them conda-friendly (conda-forge channel, bioconda channel).
[0.25] Export the environment (example) to the file and verify that it can be rebuilt from the file without problems.
<code><pre>
!which python # should return /usr/local/bin/python
!python --version #Python 3.8.16 in my colab
!echo $PYTHONPATH #return /env/python
%env PYTHONPATH=
</code></pre>
INSTALL CONDA
<code><pre>
%%bash
MINICONDA_INSTALLER_SCRIPT=Miniconda3-4.5.4-Linux-x86_64.sh
MINICONDA_PREFIX=/usr/local
wget https://repo.continuum.io/miniconda/$MINICONDA_INSTALLER_SCRIPT
chmod +x $MINICONDA_INSTALLER_SCRIPT
./$MINICONDA_INSTALLER_SCRIPT -b -f -p $MINICONDA_PREFIX

!which conda # should return /usr/local/bin/conda
!conda --version # conda 4.5.4 in my case
!which python # still returns /usr/local/bin/python
!python --version # returns Python 3.6.5 :: Anaconda, Inc.
</code></pre>
Update and configuring conda
<code><pre>
%%bash
conda install --channel defaults conda python=3.6  --yes
conda update --channel defaults --all --yes

%%bash
conda install --channel defaults conda python=3.8 --yes
conda update --channel defaults --all --yes
!conda --version # conda 22.11.1
!python --version #Python 3.8.15


!conda create --name dependencies --no-default-packages
!conda config --add channels defaults
!conda config --get channels

!conda config --add channels bioconda
!conda config --get channels

!conda config --add channels conda-forge
!conda config --get channels
</code></pre>
*installing tools with appropriate versions*
<pre><code>
!conda install fastqc=0.11.9
!conda install star=2.7.10b
!conda install bedtools=2.30.0
!conda install samtools=1.16.1
!conda install salmon=1.9.0
!conda install multiqc=1.13 #not successfull
</code></pre>
*writing  history to file  environment.yml*
<pre><code>!conda env export --from-history > environment.yml</code></pre>
in environment.yml :
<code><pre>
name: base
channels:
'  - conda-forge
  - bioconda
  - defaults
dependencies:
  - conda
  - python=3.8
  - fastqc=0.11.9
  - star=2.7.10b
  - bedtools=2.30.0
  - salmon=1.9.0
  - multiqc=1.13'
prefix: /usr/local
</code></pre>


Than I deleted the dependencies environment:

<code><pre>!conda env remove -n dependencies</code></pre>

And recover the environment on the basis of the environment.yml file:

<code><pre>!conda env create -n dependencies --file environment.yml</code></pre>

Аll the packages of particular versions are in environment.yml
<code><pre>
name: base
channels:
  - conda-forge
  - bioconda
  - defaults
dependencies:
  - conda
  - python=3.8
  - fastqc=0.11.9
  - star=2.7.10b
  - bedtools=2.30.0
  - salmon=1.9.0
  - multiqc=1.13
prefix: /usr/local
</code></pre>
** Docker**
*Installing the Docker - made it on UBUNTU Virtual machine locally, collab doesn't work*
<code><pre>

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
    
sudo mkdir -p /etc/apt/keyrings

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
  
sudo apt-get update
  
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
</code></pre>

*Create a Dockerfile*
Write text file "Dockerfile" 
mkdir Docker #create special directory
nano Dockerfile

<code><pre>

FROM ubuntu:latest
MAINTAINER "masha.ampleeva@gmail.com"
#apt-transport-https & wget
RUN apt update && \
    apt -y install apt-transport-https wget

#unzip
RUN apt install unzip

#java- openjdk
RUN apt -y install openjdk-11-jdk 

#perl
RUN apt -y install perl

#create /.bashrc for aliases
#execute ". /.bashrc" command right after the container is run
RUN touch /.bashrc

#STAR v2.7.10b
RUN wget https://github.com/alexdobin/STAR/releases/download/2.7.10b/STAR_2.7.10b.zip && \
    unzip ./STAR_2.7.10b.zip && \
    rm ./STAR_2.7.10b.zip && \
    chmod a+x ./STAR_2.7.10b/Linux_x86_64_static/STAR && \
    mv ./STAR_2.7.10b/Linux_x86_64_static/STAR /usr/local/bin/STAR && \
    rm -r ./STAR_2.7.10b
    
#FastQC v0.11.9
RUN wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip && \
    unzip fastqc_v0.11.9.zip && \
    rm fastqc_v0.11.9.zip && \
    chmod a+x /FastQC/fastqc && \
    mv  /FastQC/fastqc /usr/local/bin/fastqc && \
    echo 'alias fastqc="/FastQC/fastqc"' >> /.bashrc # && \
    
# picard v2.27.5
RUN wget https://github.com/broadinstitute/picard/releases/download/2.27.5/picard.jar -O /usr/local/bin/picard.jar && \
    chmod a+x /usr/local/bin/picard.jar && \
    echo 'alias picard="java -jar /usr/local/bin/picard.jar"' >> /.bashrc
    
# samtools v1.16.1
RUN wget https://github.com/samtools/samtools/archive/refs/tags/1.16.1.zip -O ./samtools-1.16.1.zip && \
    unzip ./samtools-1.16.1.zip && \
    rm ./samtools-1.16.1.zip && \
    mv ./samtools-1.16.1/misc /samtools && \
    rm -r ./samtools-1.16.1 && \
    mv  /samtools/samtools.pl /usr/local/bin/samtools.pl && \
    echo 'alias samtools="/usr/local/bin/samtools.pl"' >> /.bashrc
    

# bedrools v2.30.0
RUN wget https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary -O /usr/local/bin/bedtools.static.binary && \
    chmod a+x /usr/local/bin/bedtools.static.binary && \
    echo 'alias bedtools="/usr/local/bin/bedtools.static.binary"' >> /.bashrc
    
#python
RUN apt -y install python3-pip

# MultiQC v1.13
RUN pip install multiqc==1.13

#salmon v1.9.0 plus libgomp1 and libtbb12 
RUN wget https://github.com/COMBINE-lab/salmon/releases/download/v1.9.0/salmon-1.9.0_linux_x86_64.tar.gz && \
    tar -zxvf ./salmon-1.9.0_linux_x86_64.tar.gz && \
    rm salmon-1.9.0_linux_x86_64.tar.gz && \
    chmod a+x ./salmon-1.9.0_linux_x86_64/bin/salmon && \
    mv ./salmon-1.9.0_linux_x86_64/bin/salmon /usr/local/bin/salmon && \
    rm -r ./salmon-1.9.0_linux_x86_64 && \
    apt install libgomp1 libtbb12

</code></pre>

*Create Dockerimage using Dockerfile*

<code><pre>sudo docker build -t superimage .   </code></pre>

*RUN  Docker container in sudo mode*

<code><pre>sudo docker run --rm -it superimage:latest </code></pre>

Exit to escape container
 
I tried to make $PATH, but it worked not for all tools 

