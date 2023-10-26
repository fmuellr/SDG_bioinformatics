# 👆 Introduction to Bioinformatics

An overview to setting up your computer for computational analysis in the Di Giovanni lab

>  🛠️ Mac Version: Apple M1 Max, Ventura 13.5.1 // Oct 2023

## The research data store and high performance computing

### HPC account setup
Setting up your High Performance Computing account at Imperial is essential. This will allow you to carry out intensive and complex tasks using your laptop or computer without requiring high processor and avoid clogging up your laptop. 

You will need to discuss setting up your account with your PI or a user who has admin authorisation to add you to the list. Simone already has an account **hpc-sdigiova**. If you are a post-doc, PhD, and research postgrad student, you will need to ask Simone to register you. This can be done by following the instructions for [Get Access](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/get-access/) under *Get access to High Performance Computing* and selecting *registering members of your group*.  Next, your PI should also set up a research data store (RDS)project for you. This is a 2 TB space you can use to store, analyse and process your data. This can either be set up as your personal space or as a collective space which multiple users can access - this will change depending on the nature of your project. 

There are no [charges](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/service-offering/charging-structure/) asscoaited with account creation however the RDS project has a monthly charge, though it is very low. 

#### Connecting to the RDS
To connect to your project, there are several options. If you are doing this outside of the Imperial network, turn your VPN on - see below. 
The first and easiest is to connect using [MACs *Connect to server...*](https://wiki.imperial.ac.uk/display/HPC/RDS+on+macOS) which will allow you to view and access your files, but not perform any analysis. For that we need to use the Terminal. 

- Select *Go* from the finder menu
- Select *Connect to server...*
- Enter the RDS Directory Path in the Server Address box in this format: `smb://rds.imperial.ac.uk/rds/user/<username>`. For example, in my case this would look like: `smb://rds.imperial.ac.uk/RDS/user/fm2817`. You may be prompted to input your imperial user name (full email, e.g. fm2817@ic.ac.uk) and password.
- This will bring up a window where you have three folders, **ephemeral**, **home**, and **projects**. Your two most important folders are your **home** and **projects** folders.

### Accessing the HDS off campus
As you might be off campus to do some of this work, you will need access to the Imperial network in order to access the system and use its faciilities. 

While Imperial is rolling out its new **Unified Access**, just use [OpenVPN Connect](https://openvpn.net/client/) instead.  Download it and follow the instructions. When you use it the first time, you will need to download and open the OpenVPN configuration files found [here](https://www.imperial.ac.uk/admin-services/ict/self-service/connect-communicate/remote-access/virtual-private-network-vpn/). Under MacOS, click **download the OpenVPN** file. Once downloaded, click it and it should open in OpenVPN connect. Connect using your college username and password. 

### Transferring files between your computer and the HPC cluster 

#### Using Gloubs

#### Using FileZilla
> reference to geo transfer. 

## Programme Installation and Setup

Most of our analysis will be run using three main languages: Command line, Python, and R. 

You will also need to set up a Research Data Store (RDS) account to give you access to the High Performace Computing (HPC) system at Imperial. This will allow you to perform intensive data analysis that won't clog up your laptop memory. 

In addition to this, we will also set up your personal GitHub account. This is a great way to keep track of all your codes in once place and makes it easy to share your code with publishers when you are submitting papers. 

We will also install some Apps on your laptop, such as Visual Studio Code, R Studio, Jupyter, and Anaconda to get you going. 

These guides are written from the Mac point of view, if you have windows - i can't help you. Just kidding, this information can also be applied for Windows. There are some slight modifications in our setup but the actual data analysis will be the same more or less. 

### Visual Studio Code 
To write, edit, and view your scripts we need a text editor. While your laptop has a built in text editor (aka the textEdit app), it is very limited. I'd recommed using [Visual Studio Code](https://code.visualstudio.com), it's a great app for code editing that comes with additional features such as code debugging, GitHub connection, and supports a ton of languages. It's my favourite editor (what im currently using to write this document in) and is great for writing scripts and working with your files. 

### GitHub
[Github](https://github.com) place that you can use to orgnaise your codes , keep track of what you've done etc. It is easily shared with journals if they require it. Imperial also offer an introductory course, which can explain the benefits in more detail. 

### R and R Studio
To start off with, let's look at R. 
 
- Before installing R, we need to install several tools that allow R to function correctly on a Mac. One of those tools is [**XQuartz**](https://www.xquartz.org). Open the link and under *Quick Downloads* click the XQuartz package and download it.  Open the package and follow the installation instructions. It is recommended that everytime you have a major update on your Mac that you reinstall XQuartz. 
- We also need to install and setup [**Xcode**](https://developer.apple.com/xcode/). If you don't already have Xcode on your Mac, go to your App Store, search for Xcode and download the App. Then open Xcode and follow the installation instructions. You will need to create a free account when you do this. Once this has completed, we need to setup Xcode. To do this, open the **Terminal** app and type in: `sudo xcode-select --install`. Once this has finishing installing, your Xcode will be setup. 
- We will also need something known as **GNU Fortran**, because Xcode doesn't contain a Fortran compiler. To do this, navigate to the [Tools](https://mac.r-project.org/tools/) page from CRAN and install the fortran package and follow the download instructions. ![Fortran](./Images/GNU%20Fortran.png). 

Installing Xcode and GNU Fortran are both important steps for using R because some of the tools you will use to analyze your data require a process known as "compile from source". This is because some packages don't come neatly in one downloadable file, but require you to "build" the package together to make a functioning app. Xcode and GNU Fortran enable you to do this. 

- Finally, we can install R. Navigate to the [Comprehensive R Archive Network (CRAN)](https://cran.r-project.org). 
- Select **Download R for macOS**. 
- Under *Latest release:* navigagte to the appropriate download for your Mac. For example, I have an Apple silicon M1 Mac, so I download the R package for Apple silicon (M1/M2) Macs. If you have an Intel Mac, download that package instead. If you have no idea what your Mac is, click the Apple buttom (top left) and select *About this Mac* and where it says *Chip* you will find this information. 
- Once R is installed, you can now download [**RStudio**](https://posit.co/download/rstudio-desktop/). Again follow the download instructions. RStudio is an Integrated Development Environment (IDE) and is what we will be using when we run R code. 

#### R Studio

When you first open R Studio, you will see three windows. On your left, you have a tab open with the options *Console*, *Terminal*, and *Background Jobs*. Top right you have a window that shows your environment, history, conections, and tutorial. Bottom right, you have a window that shows files, plots, packages, help, viewer, and presentation. 

What we first want to do is install two important packages that we will need to run data analysis. The first one to install is called **Processx**. This is a tool that is used to run system processes in the background among other things, but what is important is that we need it to install the next package **devtools**. Devtools is used to to simplify many common tasks and is used to build packages. 

To install these packages, in your bottom right window, select **Packages**. Now you should see a tab **Install**. A window should pop up, and under **Packages (separate multiple with space or comma)** type in *processx*, select it and click **Install**. Now do the same for *devtools*. Once you have done this, you usually need to restart RStudio. 

### Command Line

### Python and Jupyter

#### Anaconda

