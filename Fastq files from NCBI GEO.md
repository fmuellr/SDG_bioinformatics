# Downloading raw data files from NCBI GEO using the SRA ToolKit

>>>> Update this for HPC clusters !!!!

## SRA Run Selector 

### Download Metadata and Accession List

On GEO, find your selected GSE series using the GSE series ID. 
At the bottom, you will see a tab `SRA Run Selector`, clicking this 
will bring you to the SRA Run Selector. Select your runs of interest
and download both `Metadata` and `Accession List`. Make sure you 
know the location you've downloaded them into. If you're unsure, 
open Terminal and drag your file into it. The specific path for that
file can be observed. 

### Download SRA toolkit

> Note: Need to have XCode from Apple Store, Command line tools (via 
> Homebrew download, easiest option), "processx", and "devtools".
> Github link for additional info: https://github.com/ncbi/sra-tools

[Click here](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=software) 
to find download. Click on MacOS 64 bit architecture to download file. 
Follow the download instructions to unpack and install the toolkit. 
Be aware of the PATH, for most users the toolkit functions will not be 
located in their PATH environemtal variable. This means that you will need
to provide a directory to where it is located to use the toolkit. 

For example:

```{r}
~/franziska/Documents/PhD/Bioinformatics/tools/sratoolkit/bin/fastq-dump
```

Note: I recommend testing that your toolkit is configured correctly first. 
Details to how to do this are located [here](https://ncbi.github.io/sra-tools/install_config.html) under "testing the
toolkit configuration". 


Some additional help:
[This](https://trace.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?view=toolkit_doc) provides a very nice overview of some of the SRA toolkit functions and their use documentation. The [Edwards Lab](https://edwards.sdsu.edu/research/fastq-dump/) also provides a good review of important features to include for fastq-dump command (really recommend reading this!). Click [here](https://ncbi.github.io/sra-tools/fastq-dump.html) for a cheatsheet for frequently used options. For cheetsheets of all options, check out the SRA Tools [GitHub](https://github.com/ncbi/sra-tools) under tools.  


## Set PATH

Use PATHs to simplify accessing files. These are GLOBAL variables 
-aka variables that are accessible globally, or everywhere 
throughout the program. They remain in memory throughout the 
runtime of the program. 

Your project path: The path to where your SRA `Metadata` and `Accession List`are stored.

```{r simulate_PROJ}
PROJ_PATH='/Volumes/Frankee\ PhD/PhD/Bioinformatics/ncbi/public/'
```

Your SRA toolkit path: The path to where your SRA toolkit is located.

```{r simulate_SRA}
SRA_PATH='/Users/franziska/Documents/PhD/Bioinformatics/tools/sratoolkit/bin'
```

> Note: I recommend that your PROJ_PATH is set to an external hard 
> drive or cloud service as the fastq files are very large. If no 
> space is left on your disk, then the download will fail to complete. 


## Get Fastq Files

You first need to check where the runs are stored in the files that you have 
downloaded. If you have mutliple series that you have downloaded, start by checking 
only one of them. In this code, `project_ids` refers to multiple GSE series, 
`project_id` refers to an individual GSE series. You will then go on to read the 
SraRunTable.txt file. This will bring up a sample of the file. Find where the various 
SRR files are stored. This will usually be under `Run` - you should see a list 
saying something along the lines of `Factor w/ x levels "SRR#####, SRR#### ....`. This 
is where all runs in the series are stored. 


```{r simulate_initiate}
project_ids=c('GSE#####')
project_id= project_ids[1]
cat(project_id,'\n')
sample_info=read.delim(paste0(PROJ_PATH,project_id,'/SraRunTable.txt'),sep=',')
    str(sample_info)
file = sample_info$Run[1]
```


> Note: You will need to change your series ID `GSE####`, and check where your 
> `SraRunTable.txt` is stored. You can see I have written `/SraRunTable.txt` 
> which indicates that my table is stored in the specific project id directory. 


```{r}
cat('\t==================\t',file,'\t==================\n')
   setwd(PROJ_PATH)
    system(paste0(SRA_PATH,'/fastq-dump --outdir "." --split-files ',file))
```

Now that you know that your code is working, you can incorporate a for loop. Check you have the appropriate out directory. 


```{r}
for(project_id in project_ids){
  cat(project_id,'\n')
  sample_info=read.delim(paste0(PROJ_PATH,project_id,'/SraRunTable.txt'),sep=',')
    str(sample_info)
 for(file in sample_info$Run){
    cat('\t==================\t',file,'\t==================\n')
   setwd(PROJ_PATH)
    system(paste0(SRA_PATH,'/fastq-dump --outdir "." --split-files ',file))
 }
}
```


WORKING CODE - BUT IDK HOW TO GET THE OUTPUT TO PUT IT IN THE INDIVIDUAL SERIES FOLDERS. 

> Note: `Project_ids` refers to multiple GSE series. `Project_id` refers to an 
> individual GSE series. 


## Fastq Storage

Store your fastq files and data using Imperial's [Research data store](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/rds/ "ICL Data Store"). This allows 2 TB of free storage plus additional storage at a cost. 
This is ideal for storing your fastq files. You will also need to get Simone or Ilaria to grant you access to the [project space](https://selfservice.rcs.imperial.ac.uk/groups/manage/all/hpc-sdigiova/ "project space").  

> Note: FYI you will probably also need to download [Viscosity](https://www.sparklabs.com/viscosity/), an OpenVPN client to allow you to connect to Imperial (that is if you don't have a connected Ethernet cable directly into Imperial's network). Check out Imperial's info about [OpenVPN](https://wiki.imperial.ac.uk/display/HPC/Configuring+OpenVPN+for+HPC+users). 
For more information regarding Imperial's HPC accesss, see [Wiki](https://wiki.imperial.ac.uk/display/HPC/High+Performance+Computing) and [Research Computing Service](https://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/). Here information about [Getting Started ](http://www.imperial.ac.uk/admin-services/ict/self-service/research-support/rcs/support/getting-started/). 


## FastQ Control 

Another nice feature that you can use is [FastQC](https://hcs7806.readthedocs.io/en/latest/RNA-Seq/). This tool "reads a set of sequence files and produces from each one a quality control report consisting of a number of different modules, each one of which will help to identify a different potential type of problem in your data".