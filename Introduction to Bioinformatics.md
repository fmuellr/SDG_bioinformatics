# 👆 Introduction to Bioinformatics

An overview to setting up your computer for computational analysis in the Di Giovanni lab

>  🛠️ Mac Version: Apple M1 Max, Ventura 13.5.1 // Oct 2023

## The research data store and high performance computing

### HPC account setup
Setting up your High Performance Computing account at Imperial is essential. This will allow you to carry out intensive and complex tasks using your laptop or computer without requiring high processor and avoid clogging up your laptop. 

You will need to discuss setting up your account with your PI or a user who has admin authorisation to add you to the list. Simone already has an account **hpc-sdigiova**. If you are a post-doc, PhD, and research postgrad student, you will need to ask Simone to register you. This can be done by following the instructions for [Get Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) under *Get access to High Performance Computing* and selecting *registering members of your group*.  Next, your PI should also set up a research data store (RDS)project for you. This is a 2 TB space you can use to store, analyse and process your data. This can either be set up as your personal space or as a collective space which multiple users can access - this will change depending on the nature of your project. 

There are no [charges](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/) associated with account creation however the RDS project has a monthly charge, though it is very low. 

#### Connecting to the RDS
To connect to your project, there are several options. If you are working within the Imperial network, make sure you are connected to **Imperial-WPA wifi**. If you are doing this outside of the Imperial network, turn your VPN on - see below. 
The first and easiest is to connect using [MACs *Connect to server...*](https://wiki.imperial.ac.uk/display/HPC/RDS+on+macOS) which will allow you to view and access your files, but not perform any analysis. For that we need to use the Terminal. 

- Select *Go* from the finder menu
- Select *Connect to server...*
- Enter the RDS Directory Path in the Server Address box in this format: `smb://rds.imperial.ac.uk/rds/user/<username>`. For example, in my case this would look like: `smb://rds.imperial.ac.uk/RDS/user/fm2817`. You may be prompted to input your imperial user name (full email, e.g. fm2817@ic.ac.uk) and password.
- This will bring up a window where you have three folders, **ephemeral**, **home**, and **projects**. Your two most important folders are your **home** and **projects** folders.

#### Accessing the HPC off campus
As you might be off campus to do some of this work, you will need access to the Imperial network in order to access the system and use its facilities. 

While Imperial is rolling out its new **Unified Access**, just use [OpenVPN Connect](https://openvpn.net/client/) instead.  Download it and follow the instructions. When you use it the first time, you will need to download and open the OpenVPN configuration files found [here](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/virtual-private-network-vpn/). Under MacOS, click **download the OpenVPN** file. Once downloaded, click it and it should open in OpenVPN connect. Connect using your college username and password. 

### But what even is the HPC?
Before we go into further detail, let's understand some basics. This information has been adapted from the [Beginner's guide to HPC at Imperial](https://imperialcollegelondon.app.box.com/s/fsoiljuf43f7ko4tfvqifbyrfj8yslbi). 

Bioinformatic analysis can persist of intensive, large scale data modeling requiring a lot of computing power. This is where Imperial's HPC cluster comes into play. A **computer cluster** is a set of connected computers that work together as a single system. The main advantage of using this system is that you can run programs which process large bits of information in parallel, cutting down the time required to analyse your data. 

![Computer cluster](./Images/Computer%20cluster.png). 

Inside of one of these computer racks, there are individual computers, also known as **nodes**. 

![Node](./Images/Node.png)

Nodes are made up of multiple **cores** that can access the same memory. A core is the basic computation unit inside a processor that can run a single process. Imperial's HPC processors contain up to 14 cores. This means that 14 parallel processes can run at the same time, accessing the same memory, to carry out analysis. 

Some other terms to know are:
- A **Job** is a program you run on the cluster
- When you **submit a job**, you are providing a set of instructions to the computer cluster to run your programs. 
- **Memory** refers to the main memory also known as **RAM**, which is the fast memory directly connected to the processor. When you run a program, it uses this space to store any temporary data it requires to carry out its analysis. 
- **Serial code** refers to running a program on one core. 
- **Parallel code** refers to running a program on two or more cores.  


Some examples of different types of jobs you can run include:

- High-throughput tasks - when you are running a large number of relatively small jobs
- Parallel jobs on a single node (threaded)
- Parallel jobs on multiple nodes (from 2 to 270 nodes)
- Large single node memory jobs (up to TBs of memory)
- Jobs using GPUs (such as machine learning tasks)
- Long-running jobs (up to 1000 hr)

### Using the cluster
All computers in the HPC resource are connected to one parallel file system - the Research Data Store. This gives you access to:

- your home directory `$HOME`- personal working space; 1 TB allocation (up to 10 mil files)

- temporary storage `$EPHEMERAL` - additional individual working space, unlimited allocation, but files are deleted after 30 days

- allocated project space `$RDS_PROJECT` - your project allocations. 

As the HPC system is a linux-based operating system, the main way that we will interact with it (e.g., submit jobs) is by using the Terminal. 
The first thing you want to do is open you Terminal App on your Mac laptop. Next we want to login to your person RDS space. To do this type in the Terminal: `ssh your_username@login.hpc.ic.ac.uk`. Replace "your_username" with your Imperial username. You will then be prompted to enter your Imperial password. Do this and hit enter. 

> You will notice as you type your password, you will not see any letters being typed. This is normal and done for security purposes to protect your personal information. 
> If you are working from home, make sure your VPN is turned on and connected to Imperial, otherwise you will not be able to access the RDS.

![Terminal Login](./Images/Terminal%20Login.png)

You will now see a screen which gives you an overview of your project allocations and your individual allocations. You can see how much space is left in each of your projects and your home folder. I will discuss the individual folders in more detail below and what to do if you run out of space and archiving projects. 

You will also see a box that contains some information. One of these is to remember, when you have logged in to the RDS, you are logging into a space that is shared amongst all users (i.e., it is a shared computer space). This means that you SHOULD NEVER run any jobs directly in this space, as it will slow down or prevent other users accessing the login nodes. This will make more sense later on when we submit our first job. 

You will also find some website links where you can check information and the service status of the RDS.

![Logged in](./Images/Logged%20in.png)

You can view which login node you are connected to by typing `hostname` and hitting enter. This is useful if, for example, you are having issues, occasionally some login nodes fail due to various issues, and in this case I'd recommend disconnecting and then reconnecting. 

![Hostname](./Images/Hostname.png)

Here you can see I am currently connected to login node **login-b**. If you type in `who | wc -l` you can also check how many people are connected to that login node to see if its currently under heavy usage. In the example below, i can see that 105 other users are currently connected to the same login node. 

![Who is connected](./Images/Who.png)


#### Running our first job
Great, now that you're logged in, let's set up our system and run our first job!

The main programs that we will be using are R, python and the command line to execute our jobs. When we submit a job, we are going to do this by instructing the **cluster resource manager** to execute our job. This is essential, as mentioned above, you do not want to run a script directly on your login node, no matter how small or quick, because there are only a certain number of login nodes. If all users compute here, the nodes will be overwhelmed and the rest of the cluster will be idle. 

Instead, the cluster resource manager takes care of sorting your requests into job classes, scheduling your requests, starting jobs when computing resources become available, monitoring jobs, and producing jobs. To communicate with the resource manager, we will need to write a **shell script**, something I will also be referring to as your **PBS script**. This instructs the resource manager what to do and what requirements you need. Passing your finished script to the resource manager is called **submitting a job**. 

The resource manager needs to know your **command** and also what **resources** your job needs. Every job script must also include the **run time**, **memory** required, and the number of **cores** it needs to run the job.

It's important that you correctly select your resources, which sometimes requried a bit of trial and error, especially when running new programs. If the resources you request are exceeded while the job is running, then the job is terminated. Also remember that the longer the job takes, the longer you have to wait for it to start. 

Now, before we submit our first script, we want to make sure certain programs are set up. 

When you submit a script, you will ask it to load a **module** to set the environment to use a software package. This about this like an App; if you want to write a document, you will open Word; if you want to carry out a t-test, you will first need to Open Excel (the module) so that you can input your data and instruct excel to carry out the analysis.  Similarly, if you want to use a certain software program to do specific analysis, you will need to load it first. In this case, module refers to a pre-installed program in the HPC system. In your Terminal, type in `module avail` to show a list of all modules available from the HPC (might take a sec to load).

![Modules](./Images/Modules.png)

You can scroll through and see that the HPC has a tone of available resources already for you to load and use. However, some specific packages are not available through the HPC directly, and while you can submit a request via [ICT ASK](https://imperial.service-now.com/ask) and search for "RCS Software Install Request", it is quicker and easier to either directly install packages into your home directory or check if they are available through **Anaconda** and install them. 

Some basic commands when looking at modules. Where it refers to loading and unloading a module, think about this as opening and closing an App. 

- List all modules available: `module avail`
- Find a module for your software: `module avail your_software`
- Load a module: `module load your_software`
- Load a specific version: `module load you_software/version_number`
- List all loaded modules: `module list`
- Swap modules: `module switch your_software your_software/version_number`
- Unload a module: `module unload your_software/version_number`
- Get rid of all loaded modules: `module purge`

***Setting up Anaconda:***
**Anaconda** is one of the main modules I use to install many different software packages. I will discuss how to use it in more detail below, but we will need to set it up before we use it. You will only need to do this once. 

In the Terminal, type in:

```bash
module load anaconda3/personal
anaconda-setup
```

This is done only once. Afterwards, use `module load anaconda3/personal` to setup your personal python environment. Next, to activate anaconda, you will either need to enter `source activate` or `conda activate` depending on the version you have (try both, see which works for you). Once you do this, you will now see that your Terminal prompt has changed to say: `(base) [fm2817@login-b ~]$` for example. 

![Active Anaconda](./Images/Active%20Anaconda.png)

Anaconda is great because it enables you to create separate  environments that contain specific software packages for your projects. This helps to keep everything organised but more importantly, lets you using different versions and avoid something known as dependency conflicts (more later). Frustratingly, not all software developers maintain their software up to date. This means that sometimes software will no longer work if, for example, it is no longer compatible with an updated programming language. To avoid these issues, you can create an environment in Anaconda which serves as a separate, isolated space where you can load older versions which are compatible. 

***Setting up R in Anaconda:***
R and R libraries can be installed using your personal Anaconda. While you can load R modules from the HPC, I find that they don't maintain it up to date and I prefer to start off by using the most recent version of R.

To do this we want to set up an environment in Anaconda that contains R. 
First we want to create an environment and name it (choose your name wisely - you will need to input it each time you want to load it). If you ever forget what environments you've created, you can simple type in `conda info --envs` to obtain a list. 

For example, in my case, I called my R environment "r_env": `conda create -n r_env`.
Then we want to activate it: `source activate r_env`. When you hit enter on this command, you can now see that instead of (base) the environment has now changes to (r_env). 

![r_env](./Images/r_env.png)

Now we need to install R. To do this, we first want to configure our channels so that we can use the **conda-forge** channel. Conda channels are basically locations were packages are stored and managed. The conda-forge channel is one of the biggest and best channels to download packages from because they are maintained up to date and is free to use. Use:

```bash
conda config --add channels conda-forge
conda config --set channel_priority strict
```
You can always change the priority from strict to flexible later if required - this is explained in more detail [here](https://conda.io/docs/user-guide/tasks/manage-channels.html), but not required at this time.

Now we can install R from the conda-forge channel; type in `conda install r -c conda-forge`.
We also want to install some essential packages in R that we will use:

```bash
conda install r-essentials e-base
conda install r-dev-tools 
conda install r-biocmanager
```

***Running our first script***
Now that we have Anaconda and R set up, lets write, submit, and view our first script called hellohpc!








### Transferring files between your computer and the HPC cluster 

#### Using Globus

#### Using FileZilla
> reference to geo transfer. 

## Programme Installation and Setup

Most of our analysis will be run using three main languages: Command line, Python, and R. 

You will also need to set up a Research Data Store (RDS) account to give you access to the High Performance Computing (HPC) system at Imperial. This will allow you to perform intensive data analysis that won't clog up your laptop memory. 

In addition to this, we will also set up your personal GitHub account. This is a great way to keep track of all your codes in once place and makes it easy to share your code with publishers when you are submitting papers. 

We will also install some Apps on your laptop, such as Visual Studio Code, R Studio, Jupyter, and Anaconda to get you going. 

These guides are written from the Mac point of view, if you have windows - i can't help you. Just kidding, this information can also be applied for Windows. There are some slight modifications in our setup but the actual data analysis will be the same more or less. 

### Visual Studio Code 
To write, edit, and view your scripts we need a text editor. While your laptop has a built in text editor (aka the textEdit app), it is very limited. I'd recommend using [Visual Studio Code](https://code.visualstudio.com), it's a great app for code editing that comes with additional features such as code debugging, GitHub connection, and supports a ton of languages. It's my favourite editor (what im currently using to write this document in) and is great for writing scripts and working with your files. 

include here about using this to write PBS scripts -> easier. 

### GitHub
[Github](https://github.com) place that you can use to organise your codes , keep track of what you've done etc. It is easily shared with journals if they require it. Imperial also offer an introductory course, which can explain the benefits in more detail. 

### R and R Studio
To start off with, let's look at R. 
 
- Before installing R, we need to install several tools that allow R to function correctly on a Mac. One of those tools is [**XQuartz**](https://www.xquartz.org). Open the link and under *Quick Downloads* click the XQuartz package and download it.  Open the package and follow the installation instructions. It is recommended that every time you have a major update on your Mac that you reinstall XQuartz. 
- We also need to install and setup [**Xcode**](https://developer.apple.com/xcode/). If you don't already have Xcode on your Mac, go to your App Store, search for Xcode and download the App. Then open Xcode and follow the installation instructions. You will need to create a free account when you do this. Once this has completed, we need to setup Xcode. To do this, open the **Terminal** app and type in: `sudo xcode-select --install`. Once this has finishing installing, your Xcode will be setup. 
- We will also need something known as **GNU Fortran**, because Xcode doesn't contain a Fortran compiler. To do this, navigate to the [Tools](https://mac.r-project.org/tools/) page from CRAN and install the fortran package and follow the download instructions. ![Fortran](./Images/GNU%20Fortran.png). 

Installing Xcode and GNU Fortran are both important steps for using R because some of the tools you will use to analyze your data require a process known as "compile from source". This is because some packages don't come neatly in one downloadable file, but require you to "build" the package together to make a functioning app. Xcode and GNU Fortran enable you to do this. 

- Finally, we can install R. Navigate to the [Comprehensive R Archive Network (CRAN)](https://cran.r-project.org). 
- Select **Download R for macOS**. 
- Under *Latest release:* navigate to the appropriate download for your Mac. For example, I have an Apple silicon M1 Mac, so I download the R package for Apple silicon (M1/M2) Macs. If you have an Intel Mac, download that package instead. If you have no idea what your Mac is, click the Apple button (top left) and select *About this Mac* and where it says *Chip* you will find this information. 
- Once R is installed, you can now download [**RStudio**](https://posit.co/download/rstudio-desktop/). Again follow the download instructions. RStudio is an Integrated Development Environment (IDE) and is what we will be using when we run R code. 

#### R Studio

When you first open R Studio, you will see three windows. On your left, you have a tab open with the options *Console*, *Terminal*, and *Background Jobs*. Top right you have a window that shows your environment, history, connections, and tutorial. Bottom right, you have a window that shows files, plots, packages, help, viewer, and presentation. 

What we first want to do is install two important packages that we will need to run data analysis. The first one to install is called **Processx**. This is a tool that is used to run system processes in the background among other things, but what is important is that we need it to install the next package **devtools**. Devtools is used to to simplify many common tasks and is used to build packages. 

To install these packages, in your bottom right window, select **Packages**. Now you should see a tab **Install**. A window should pop up, and under **Packages (separate multiple with space or comma)** type in *processx*, select it and click **Install**. Now do the same for *devtools*. Once you have done this, you usually need to restart RStudio. 

### Command Line

### Python and Jupyter

#### Anaconda

