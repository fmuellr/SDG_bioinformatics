# 👆 Introduction to Bioinformatics

An overview to setting up your computer for computational analysis in the Di Giovanni lab

>  🛠️ Mac Version: Apple M1 Max, Ventura 13.5.1 // Oct 2023

## Programme Installation and Setup

Most of our analysis will be run using three main languages: Command line, Python, and R. 

You will also need to set up a Research Data Store (RDS) account to give you access to the High Performace Computing (HPC) system at Imperial. This will allow you to perform intensive data analysis that won't clog up your laptop memory. 

In addition to this, we will also set up your personal GitHub account. This is a great way to keep track of all your codes in once place and makes it easy to share your code with publishers when you are submitting papers. 

We will also install some Apps on your laptop, such as Visual Studio Code, R Studio, Jupyter, and Anaconda to get you going. 

These guides are written from the Mac point of view, if you have windows - i can't help you. Just kidding, this information can also be applied for Windows. There are some slight modifications in our setup but the actual data analysis will be the same more or less. 

### Visual Studio Code 
To write, edit, and view your scripts we need a text editor. While your laptop has a built in text editor (aka the textEdit app), it is very limited. I'd recommed using [Visual Studio Code] (https://code.visualstudio.com), it's a great app for code editing that comes with additional features such as code debugging, GitHub connection, and supports a ton of languages. It's my favourite editor (what im currently using to write this document in) and is great for writing scripts and working with your files. 

### R and R Studio
To start off with, let's look at R. 
 
- Before installing R, we need to install several tools that allow R to function correctly on a Mac. One of those tools is [**XQuartz**] (https://www.xquartz.org). Open the link and under *Quick Downloads* click the XQuartz package and download it.  Open the package and follow the installation instructions. It is recommended that everytime you have a major update on your Mac that you reinstall XQuartz. 
- We also need to install and setup [**Xcode**] (https://developer.apple.com/xcode/). If you don't already have Xcode on your Mac, go to your App Store, search for Xcode and download the App. Then open Xcode and follow the installation instructions. You will need to create a free account when you do this. Once this has completed, we need to setup Xcode. To do this, open the **Terminal** app and type in: `sudo xcode-select --install`. Once this has finishing installing, your Xcode will be setup. 
- We will also need something known as **GNU Fortran**, because Xcode doesn't contain a Fortran compiler. To do this, navigate to the [Tools] (https://mac.r-project.org/tools/) page from CRAN and install the fortran package and follow the download instructions. ![Fortran](./Images/GNU%20Fortran.png). 

Installing Xcode and GNU Fortran are both important steps for using R because some of the tools you will use to analyze your data require a process known as "compile from source". This is because some packages don't come neatly in one downloadable file, but require you to "build" the package together to make a functioning app. Xcode and GNU Fortran enable you to do this. 

- Finally, we can install R. Navigate to the [Comprehensive R Archive Network (CRAN)] (https://cran.r-project.org). 
- Select **Download R for macOS**. 
- Under *Latest release:* navigagte to the appropriate download for your Mac. For example, I have an Apple silicon M1 Mac, so I download the R package for Apple silicon (M1/M2) Macs. If you have an Intel Mac, download that package instead. If you have no idea what your Mac is, click the Apple buttom (top left) and select *About this Mac* and where it says *Chip* you will find this information. 
- Once R is installed, you can now download [**RStudio**] (https://posit.co/download/rstudio-desktop/). Again follow the download instructions. RStudio is an Integrated Development Environment (IDE) and is what we will be using when we run R code. 

#### R Studio

When you first open R Studio, you will see three windows. On your left, you have a tab open with the options *Console*, *Terminal*, and *Background Jobs*. Top right you have a window that shows your environment, history, conections, and tutorial. Bottom right, you have a window that shows files, plots, packages, help, viewer, and presentation. What we first want to do is install two important packages that we will need when running our data analysis. The first one to install is called **Processx**. 




### Command Line

### Python and Jupyter

#### Anaconda

