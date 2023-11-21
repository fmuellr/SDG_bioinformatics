# 🎶 RNA sequencing

>  🛠️ Mac Version: Apple M1 Max, Ventura 13.5.1 // Nov 2023

# What do we actually mean when we talk about RNA sequencing?

# Overview of Analysis Process

1. Download the BiocManager from [Bioconductor](https://www.bioconductor.org/install/). In RStudio, input;
    
    ```r
    if (!require("BiocManager", quietly = TRUE))
        install.packages("BiocManager")
    BiocManager::install(version = "3.15")
    ```
    
2. Acquire raw data - either in the form of fastq or fasta files.
    - You can use the SRA run selector to download raw data from GEO [see getSRR](https://github.com/fm2817/get_SRR). Or you will need to demultiplex your reads using the `bcl2fastq` packages on RStudio.
3. Align your sequences to a reference sequence and map to genes. We use Salmon to do this. Salmon is able to produce a highly-accurate, transcript-level quantification estimates from RNA-seq data. For additional details, please refer to [getting started](https://combine-lab.github.io/salmon/getting_started) and the [combine lab github](https://github.com/COMBINE-lab/salmon). 
4. Carry out analysis using DESeq2, edgeR or another appropriate analysis method.
    - DESeq2: [beginner's guide](https://bioc.ism.ac.jp/packages/2.14/bioc/vignettes/DESeq2/inst/doc/beginner.pdf), [vignettes](http://bioconductor.org/packages/devel/bioc/vignettes/DESeq2/inst/doc/DESeq2.html), and [DESeq2 package](https://bioconductor.org/packages/release/bioc/html/DESeq2.html).
    - edgeR: [vignettes](http://bioconductor.org/packages/release/bioc/vignettes/edgeR/inst/doc/), and [edgeR package](https://bioconductor.org/packages/release/bioc/html/edgeR.html).
        
        <aside>
        💡 Note: I recommend using DESeq2. Also here is a really nice explanation of the differences.
        
        </aside>

        # Quality Control

## FastQC

FastQC is a quality control analysis tool for high throughput sequencing data. 

### Introduction

FastQC is a program designed to spot potential problems in high throughput sequencing datasets. It runs a set of analyses on one or more raw sequencing files in *fastq* or *bam* format and produces a report which summarises the results. FastQC will highlight any areas where this library looks unusual and where you should take a closer look. The program is not tied to any specific type of sequencing technique and can be used to look at libraries coming from a large number of different experiment types including genomic sequencing, ChIP-seq, RNA-seq, BS-seq, etc. For additional information, see the [documentation](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/).

### Installing FastQC

There are two methods of running FastQC, in the interactive mode or via the command line. 

**Interactive Mode**

If you want to check the quality of a few runs, using the interactive version is suitable. [Download](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/) FastQC to your platform (MacOSX or Win/Linus) and follow the installation and setup [instructions](https://raw.githubusercontent.com/s-andrews/FastQC/master/INSTALL.txt). 

**Command Line Mode** 

If you want to run FastQC on many files, it is quicker to use the command line. 

Method 1: (recommended)

Installing FastQC via Anaconda is quick and easy. This is also recommended when using FastQC on the HPC cluster. 

```bash
$ conda create --name fastqc
$ conda activate fastqc
$ conda install -c bioconda fastqc
```

Check which version of FastQC you have with `fastqc --version`. I would also check which Java version you have installed using `java --version`. If Java is not installed, you can install it into your environment using:

```bash
$ conda activate fastqc 
$ conda install -c conda-forge openjdk
```

Now all you need to do is activate your environment and then you can run FastQC from any location.

Method 2:

First make sure you have a [Java Runtime Environment](https://adoptopenjdk.net), regardless of which platform you’re using. Install the *latest release* for you platform. Once installed, head to *Terminal* (Mac) and use `java -version` to check if your install worked. You should see an output like this:

```bash
$ java version "14.0.1" 2020-04-14
$ Java(TM) SE Runtime Environment (build 14.01.1+7)
$ Java HotSpot(TM) 64-Bit Server VM (build 14.0.1+7, mixed mode, sharing)
```

Head back to [FastQC install](https://www.bioinformatics.babraham.ac.uk/projects/download.html#fastqc) and download the [FastQC v0.11.9 (Win/Linux zip file](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.9.zip) regardless of whether you use a Mac or Windows. Open the zip file and save the folder to an appropriate location.

Now, head to Terminal. You will need to make the FastQC executable by using `$ <PATH TO FILE> chmod 755 fastqc`. Now you can run FastQC directly.

### Running FastQC

To use FastQC, use the syntax `fastqc [OPTIONS] seqfile1 seqfile2..seqfileN`.

You can use `fastqc --help` to see additional options. However, generally the standard options are fine for your data: `fastqc -d -o seqfile 1` where -d specifies the temporary directory the program uses to process files (not important for you local machine but important for HPC closer. I usually set this to my *fm2817/home* directory but check you have enough space); -o specifies the output directory, followed by your file names that you want to analyse.

If you want to process multiple files, I usually submit a PBS script to the HPC cluster. For example, for the dataset GSE111497:

```bash
#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=8:mem=16gb
#PBS -N fastqc_111497
#PBS -o ./logs/stdout/fastqc_gse111497
#PBS -e ./logs/stderr/fastqc_gse111497

cd $PBS_O_WORKDIR
cd ../projects/genomics-data/live/fran

module load anaconda3/personal
source activate fastqc

for fn in ./dev_single/fastq_gse111497/SRR68119{27..71};
do
samp=`basename ${fn}`
echo "Processing sample ${samp}"
fastqc -d . -o ./fastqc/gse111497 \
    ${fn}/${samp}_pass.fastq
done
```

This loop tells the system that for *SRR folders 27 to 71* in the *fastq_gse111497* folder, go into each folder and process each *.fastq* file with `fastqc` and put them into the  output directory *fastqc/gse111497*

<aside>
💡 Note: for fastqc, you need to create your output directory folder prior to running the script. For some reason, it doesn’t want to make a new folder and will give you an error.

</aside>

### FastQC Reports

There is a lot of information that you get from running FastQC. I recommend reading through these resources as they give a nice overview of the output.

- [Analysis Modules](https://www.bioinformatics.babraham.ac.uk/projects/fastqc/Help/3%20Analysis%20Modules/)
- [FastQC Manual](https://dnacore.missouri.edu/PDF/FastQC_Manual.pdf)
- [Galaxy Project - FastQC](https://training.galaxyproject.org/training-material/topics/sequence-analysis/tutorials/quality-control/tutorial.html)
- [Using FastQC](https://hbctraining.github.io/Intro-to-rnaseq-hpc-salmon/lessons/qc_fastqc_assessment.html)
- [Incorrect encoding of phred scores](https://sequencing.qcfail.com/articles/incorrect-encoding-of-phred-scores/)
- [Phred scores](http://drive5.com/usearch/manual/quality_score.html)

## MultiQC

MultiQC is a great software to aggregate results from bioinformatics analyses across many samples into a single report.

### Introduction

[MultiQC](https://multiqc.info) is a reporting tool that combines summarising statistics from results and log files generated by other bioinformatic tools. It can be used with a variety of [pre-alignment, alignment, and post-alignment platforms](https://multiqc.info/docs/#multiqc-modules), including FastQC, Salmon, and STAR. When you use MultiQC, it recursively searches through any provided file paths and finds files that it recognises, pareses relevant information and generates a single stand-alone HTML report file.

### Installing MultiQC

MultiQC can be installed in a variety of ways, but I prefer using Anaconda on your local machine.

Activate your conda environment, and make sure you configure you channels correctly first so that  conda-forge is set as the highest priority.

```bash
$ conda config --add channels defaults
$ conda config --add channels bioconda
$ conda config --add channels conda-forge
```

Once this is done, set up a new environment where we will install MultiQC.

```bash
$ conda create --name multiqc
$ conda activate multiqc
$ conda install -c bioconda -c conda-forge multiqc
```

Now you have a new environment set up with MultiQC. An additional step you need to take is to check which version of python you have. MultiQC needs Python version 3.5+.

There are other methods of installing MultiQC, see the [MultiQC documentation](https://multiqc.info/docs/#installing-multiqc).

### Running MultiQC

Once installed, go to your analysis directory and run `multiqc`, followed by a list of directories to search. At it's simplest, it can just be `.` (the current working directory). That's it! MultiQC will scan the specified directories and produce a report based on details found in any log files that it recognises.

Syntax:  `multiqc [OPTIONS] folder containing files you want`.

For example, `multiqc -o ./Reports ./QC`. Here we are telling MultiQC to use the `-o` output directory *Reports* and to search all files in *QC*.

For further options, please see the [documentation](https://multiqc.info/docs/#running-multiqc).

### MultiQC Reports

See [Using MultiQC Reports](https://multiqc.info/docs/#using-multiqc-reports) for more information about how to use the generated report. There is also a [nice video](https://www.youtube.com/watch?v=qPbIlO_KWN0) that briefly goes through the toolbox functions.

### Quick Scripts

<aside>
💡 Note: Access from Users/franziska

</aside>

- MultiQC - Star
    
    ```bash
    $ multiqc -o ./Documents/PhD/Bioinformatics/qc_2/star/Logs/gse110950 ./Documents/PhD/Bioinformatics/qc_2/star/Logs/gse110950
    ```
    
- MultiQC - Qualimap
    
    ```bash
    $ multiqc -o ./Documents/PhD/Bioinformatics/qc_2/Qualimap ./Documents/PhD/Bioinformatics/qc_2/Qualimap
    ```
    
- MultiQC - Qualimap total view
    
    ```bash
    $ multiqc -o ./Documents/PhD/Bioinformatics/qc_2/MultiQC/110950 -m fastqc -m star -m qualimap -m salmon --file-list ./Documents/PhD/Bioinformatics//qc_2/MultiQC/file_lists/file_list_110950.txt
    ```
    
- MultiQC - Salmon
    
    ```bash
    $ multiqc -o ./Documents/PhD/Bioinformatics/Salmon/quants/quants_gse63143 ./Documents/PhD/Bioinformatics/Salmon/quants/quants_gse63143
    ```
    

## Trimming using TrimGalore

MISSING SUECTION

```bash
#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=4:mem=4gb
#PBS -N trim_cited2
#PBS -o ./logs/stdout/trim_cited2_stdout
#PBS -e ./logs/stderr/trim_cited2_stderr

cd $PBS_O_WORKDIR
cd ../projects

module load anaconda3/personal
source activate trimgalore

for fn in ./development/live/igf/IGFQ001066_di-giovanni_5-10-2020_mRNA/fastq/2020-11-04/HK5CFBBXY/IGFQ001066_di-giovanni_5-10-2020_mRNA/IGF1164{37..42};
do
samp=`basename ${fn}`
echo "Processing sample ${samp}"
trim_galore --quality 20 --phred33 --fastqc \
--adapter AGATCGGAAGAGCACACGTCTGAACTCCAGTCA \
--adapter2 AGATCGGAAGAGCGTCGTGTAGGGAAAGAGTGT \
--stringency 3 --length 50 \
-paired ${fn}/*R1_001.fastq.gz ${fn}/*R2_001.fastq.gz \
-o ./development/live/igf/IGFQ001066_di-giovanni_5-10-2020_mRNA/trimgalore/${samp}_trim
done
```

Used the options:
--quality: removed all bases less than 20 Phred
--phred33: correct Illumina program
--fastqc: to create a FastQC output in the end
—adapter and —adapter2: Illumina universal adapters
—stringency: set this to 3. The default is 1 which means that it will remove even 1 basepair if it overlaps the adapter. Since the genomics facility demultiplexes and runs a adapter trimming program (bcl2fastq) I though this was too stringent, so used 3 instead. I have seen posts that use all settings 1-6.
—paired: need to put this for paired end reads
-o: where the output files should go.

Set this in the form of a for loop. Yes could use arrays but still need to learn how an array works.

# Salmon

[Salmon](https://salmon.readthedocs.io/en/latest/salmon.html) is a fast program to produce highly-accurate, transcript-level quantification estimates from RNA-seq data. 

## Installation

In order to run Salmon, since it is a Linux/Unix based system you will need *Anaconda* in order to install it. This [link](https://combine-lab.github.io/salmon/getting_started/) is a great beginners guide to installing and using Salmon.

Make sure your Anaconda is setup and ready to go. As Salmon uses high computing power, I’d recommend setting it up on the HPC server to reduce computing power on your own laptop/computer. 

Once you have a new Anaconda environment setup for Salmon, [install it using](https://anaconda.org/bioconda/salmon): command `conda install -c bioconda salmon`.

To activate your conda environment: `conda activate salmon`

You should now see that where it said `(base) username ~ %` it should now say `(salmon) username ~ %`. Then to check that salmon is running on your system correctly;

`salmon -h`

Your output should say:

`salmon v1.1.0 Usage: salmon -h|--help or salmon -v|--version or salmon -c|--cite or salmon [--no-version-check] <COMMAND> [-h | options] Commands: index Create a salmon index quant Quantify a sample alevin single cell analysis swim Perform super-secret operation quantmerge Merge multiple quantifications into a single file`

## How and why to use Salmon

Salmon is super easy to use and very fast compared to traditional methods such as cufflinks/Bowtie/Tophat. Kallisto is another great alternative, and is even faster than Salmon, but the payoff is accuracy, and salmon was found to be more accurate. 

- https://gencore.bio.nyu.edu/salmon-kallisto-rapid-transcript-quantification-for-rna-seq-data/
- https://liorpachter.wordpress.com/2017/08/02/how-not-to-perform-a-differential-expression-analysis-or-science/

Salmon is able to quantify your reads in super time and accuracy. The first thing you need to do is build an index with your reference genome, then you can use mapping-based mode to quantify your reads. 

### Building your index

If you want to use Salmon in mapping-based mode, then you first have to build a salmon index for your transcriptome. Assume that `transcripts.fa` contains the set of transcripts you wish to quantify. We generally recommend that you build a *decoy-aware* transcriptome file.

There are two options for generating a decoy-aware transcriptome:

- The first is to compute a set of decoy sequences by mapping the annotated transcripts you wish to index against a hard-masked version of the organism’s genome. This can be done with e.g.[MashMap2](https://github.com/marbl/MashMap), and we provide some simple scripts to greatly simplify this whole process. Specifically, you can use the [generateDecoyTranscriptome.sh](https://github.com/COMBINE-lab/SalmonTools/blob/master/scripts/generateDecoyTranscriptome.sh) script, whose instructions you can find [in this README](https://github.com/COMBINE-lab/SalmonTools/blob/master/README.md).
- The second is to use the entire genome of the organism as the decoy sequence. This can be done by concatenating the genome to the end of the transcriptome you want to index and populating the decoys.txt file with the chromosome names. Detailed instructions on how to prepare this type of decoy sequence is available [here](https://combine-lab.github.io/alevin-tutorial/2019/selective-alignment/). This scheme provides a more comprehensive set of decoys, but, obviously, requires considerably more memory to build the index.

<aside>
💡 I recommend using version 2.

</aside>

To build your decoy aware transcriptome, i followed this [guide](https://combine-lab.github.io/alevin-tutorial/2019/selective-alignment/). The first thing you want to do is download your reference genome and transcriptome. I prefer to download from gencode, though you can also use ensembl. On the HPC cluster, navigate to the folder where you want to download your index. Don’t put it in your home folder, its a big file, so put it in a project. 

Use: 

```bash
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_mouse/release_M25/gencode.vM25.transcripts.fa.gz
wget ftp://ftp.ebi.ac.uk/pub/databases/gencode/Gencode_mouse/release_M25/GRCm38.primary_assembly.genome.fa.gz
```

Check that the version is the correct version you want. As of 16/09/22 the latest version is M30 (GRCm39)

Next, use the grep function to “grab” the genome targets from the above references and paste these in your decoy aware file. Salmon indexing requires the names of the genome targets to function properly. 

```bash
grep "^>" <(gunzip -c GRCm38.primary_assembly.genome.fa.gz) | cut -d " " -f 1 > decoys.txt
sed -i.bak -e 's/>//g' decoys.txt
```

Sometimes an error occurs with this, so check the decoy.txt file in excel. If there are any symbols in front of names, remove them. 

Along with a list of decoys, salmon also needs the concatenated transcriptome and genome reference file for index. It is important that the genome targets (so the decoys) come after the transcriptome targets in the reference. 

```bash
cat gencode.vM23.transcripts.fa.gz GRCm38.primary_assembly.genome.fa.gz > gentrome.fa.gz
```

Now we are ready to run the index. Prepare your PBS script as follows:

```bash
#PBS -S /bin/bash
#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=8:mem=36gb
#PBS -N m25_index_salmon
#PBS -j	oe 

cd $PBS_O_WORKDIR
cd ../projects/seqdataset/live/fran/salmon

module load anaconda3/personal
source activate salmon_1.2.0

salmon index -t gentrome.fa.gz -d decoys.txt -p 8 -i m25_index --gencode
```

Notes:

- #PBS -N m25_index_salmon ← change the name as you prefer
- cd ../projects/seqdataset/live/fran/salmon ← change to your folder
- source activate salmon_1.2.0 ← activate your salmon

You only need to make an index once, unless you want to update your gencode version, then make a new one. 

### Running Mapping-based mode to quantify

Now that we made our index, we can actually run salmon. Now we can provide any set of reads or read and quantify these directly against the index using the salmond quant command:

For paired-end:

```bash
salmon quant -i transcripts_index -l <LIBTYPE> -1 reads1.fq -2 reads2.fq --validateMappings -o transcripts_quant
```

For single-end:

```bash
salmon quant -i transcripts_index -l <LIBTYPE> -r reads.fq --validateMappings -o transcripts_quant
```

- salmon: activates the salmon program
- quant: uses salmon in quantification mode
- -i: point to the folder where your index is located
- -l: provide the library type. I usually leave this as A (automatic)
- -1: read 1
- -2: read 2
- —validateMappings: use quant in mapping based more.
- -o: where your output file goes

I usually add some extra options:

- -p: the number of threads running in parallel. I set this at 8. (this is basically how many things can salmon do at the same time to reduce time)
- -- gcBias: This enables Salmon to learn and correct for fragment-level GC buases in the input data. Since DRGs have high GC content, i would recommend using this flag. This reduces isoform quantification errors.
- -- seqBias: the enables Salmon to learn and correct for sequence-specific biases in the input data. It will attempt to correct for random hexamer priming bias, which results in the preferential sequencing of fragments starting with certain nucleotide motifs. By default, Salmon learns the sequence-specific bias parameters using 1,000,000 reads from the beginning of the input. If you wish to change the number of samples from which the model is learned, you can use the `--numBiasSamples` parameter. Salmon uses a variable-length Markov Model (VLMM) to model the sequence specific biases at both the 5’ and 3’ end of sequenced fragments. This methodology generally follows that of Roberts et al. [2](https://salmon.readthedocs.io/en/latest/salmon.html#roberts)
, though some details of the VLMM differ.
    - If you use gc and seq Bias together, salmon does an additional step. Salmon will learn a conditional fragment-GC bias model. So it will learn 3 different fragment-GC bias models based on the GC content of the fragment start and end contexts.

When you put these codes together and prepare your file for PBS, it should look something like this

```bash
#PBS -l walltime=24:00:00
#PBS -l select=1:ncpus=8:mem=36gb
#PBS -N salmon_c1_trim
#PBS -o ./logs/stdout/salmon_c1_trim_stdout
#PBS -e ./logs/stderr/salmon_c1_trim_stderr

cd $PBS_O_WORKDIR
cd ../projects

module load anaconda3/personal
source activate salmon_1.2.0

salmon quant -i <PATH TO INDEX> -l A \
    -1 <PATH TO READ1> \
    -2 <PATH TO READ2> \
    -p 8 --validateMappings --gcBias --seqBias -o <PATH TO OUTPUTFOLDER_quant>
```

Notes:

- -N salmon_c1_trim ← give it the name you want
- #PBS -o ./logs/stdout/salmon_c1_trim_stdout
#PBS -e ./logs/stderr/salmon_c1_trim_stderr ← dump the log and error files into whichever folder in your home you prefer
- cd ../projects ← navigate to the appropriate folder
- source activate salmon_1.2.0 ← load your salmon anaconda
- -1/2: you can also use <(gunzip -c <PATH TO FASTQ>) if your fastq files are gzipped.
    - If you have **technical** replicates, you can include this part of one flag. eg. -1 <PATH TO TECHNICAL REPLICATE 1> <PATH TO TECHNICAL REPLICATE 2>
- -o: for your output folder, make sure to name your folder eg. control1_quant ← include the “_quant” at the end because this will make your RNAseq analysis easier.

# Further information

## Output files

Salmon’s main output is its quantification file. This file is a plain-text, tab-separated file with a single header line (which names all of the columns). This file is named `quant.sf` and appears at the top-level of Salmon’s output directory. The columns appear in the following order:

Each subsequent row describes a single quantification record. The columns have the following interpretation.

- **Name** — This is the name of the target transcript provided in the input transcript database (FASTA file).
- **Length** — This is the length of the target transcript in nucleotides.
- **EffectiveLength** — This is the computed *effective* length of the target transcript. It takes into account all factors being modeled that will effect the probability of sampling fragments from this transcript, including the fragment length distribution and sequence-specific and gc-fragment bias (if they are being modeled).
- **TPM** — This is salmon’s estimate of the relative abundance of this transcript in units of Transcripts Per Million (TPM). TPM is the recommended relative abundance measure to use for downstream analysis.
- **NumReads** — This is salmon’s estimate of the number of reads mapping to each transcript that was quantified. It is an “estimate” insofar as it is the expected number of reads that have originated from each transcript given the structure of the uniquely mapping and multi-mapping reads and the relative abundance estimates for each transcript.

## **Command Information File**

In the top-level quantification directory, there will be a file called `cmd_info.json`. This is a JSON format file that records the main command line parameters with which Salmon was invoked for the run that produced the output in this directory.

## **Auxiliary Files**

The top-level quantification directory will contain an auxiliary directory called `aux_info` (unless the auxiliary directory name was overridden via the command line). This directory will have a number of files (and subfolders) depending on how salmon was invoked.

### **Meta information**

The auxiliary directory will contain a JSON format file called `meta_info.json` which contains meta information about the run, including stats such as the number of observed and mapped fragments, details of the bias modeling etc. If Salmon was run with automatic inference of the library type (i.e. `--libType A`), then one particularly important piece of information contained in this file is the inferred library type. Most of the information recorded in this file should be self-descriptive.

### **Unique and ambiguous count file**

The auxiliary directory also contains 2-column tab-separated file called `ambig_info.tsv`. This file contains information about the number of uniquely-mapping reads as well as the total number of ambiguously-mapping reads for each transcript. This file is provided mostly for exploratory analysis of the results; it gives some idea of the fraction of each transcript’s estimated abundance that derives from ambiguously-mappable reads.

### **Observed library format counts**

When run in *mapping-based* mode, the quantification directory will contain a file called `lib_format_counts.json`. This JSON file reports the number of fragments that had at least one mapping compatible with the designated library format, as well as the number that didn’t. It also records the strand-bias that provides some information about how strand-specific the computed mappings were.

Finally, this file contains a count of the number of *mappings* that were computed that matched each possible library type. These are counts of *mappings*, and so a single fragment that maps to the transcriptome in more than one way may contribute to multiple library type counts. **Note**: This file is currently not generated when Salmon is run in alignment-based mode.

### **Fragment length distribution**

The auxiliary directory will contain a file called `fld.gz`. This file contains an approximation of the observed fragment length distribution. It is a gzipped, binary file containing integer counts. The number of (signed, 32-bit) integers (with machine-native endianness) is equal to the number of bins in the fragment length distribution (1,001 by default — for fragments ranging in length from 0 to 1,000 nucleotides).

### **Sequence-specific bias files**

If sequence-specific bias modeling was enabled, there will be 4 files in the auxiliary directory named `obs5_seq.gz`, `obs3_seq.gz`, `exp5_seq.gz`, `exp5_seq.gz`. These encode the parameters of the VLMM that were learned for the 5’ and 3’ fragment ends. Each file is a gzipped, binary file with the same format.

It begins with 3 32-bit signed integers which record the length of the context (window around the read start / end) that is modeled, follwed by the length of the context that is to the left of the read and the length of the context that is to the right of the read.

Next, the file contains 3 arrays of 32-bit signed integers (each of which have a length of equal to the context length recorded above). The first records the order of the VLMM used at each position, the second records the *shifts* and the *widths* required to extract each sub-context — these are implementation details.

Next, the file contains a matrix that encodes all VLMM probabilities. This starts with two signed integers of type `std::ptrdiff_t`. This is a platform-specific type, but on most 64-bit systems should correspond to a 64-bit signed integer. These numbers denote the number of rows (*nrow*) and columns (*ncol*) in the array to follow.

Next, the file contains an array of (*nrow* * *ncol*) doubles which represent a dense matrix encoding the probabilities of the VLMM. Each row corresponds to a possible preceeding sub-context, and each column corresponds to a position in the sequence context. Unused values (values where the length of the sub-context exceed the order of the model at that position) contain a 0. This array can be re-shaped into a matrix of the appropriate size.

Finally, the file contains the marginalized 0:sup:th-order probabilities (i.e. the probability of each nucleotide at each position in the context). This is stored as a 4-by-context length matrix. As before, this entry begins with two signed integers that give the number of rows and columns, followed by an array of doubles giving the marginal probabilities. The rows are in lexicographic order.

### **Fragment-GC bias files**

If Salmon was run with fragment-GC bias correction enabled, the auxiliary directory will contain two files named `expected_gc.gz` and `observed_gc.gz`. These are gzipped binary files containing, respectively, the expected and observed fragment-GC content curves. These files both have the same form. They consist of a 32-bit signed int, *dtype* which specifies if the values to follow are in logarithmic space or not. Then, the file contains two signed integers of type `std::ptrdiff` which give the number of rows and columns of the matrix to follow. Finally, there is an array of *nrow* by *ncol* doubles. Each row corresponds to a conditional fragment GC distribution, and the number of columns is the number of bins in the learned (or expected) fragment-GC distribution.

## **Fragment Library Types**

There are numerous library preparation protocols for RNA-seq that result in sequencing reads with different characteristics. For example, reads can be single end (only one side of a fragment is recorded as a read) or paired-end (reads are generated from both ends of a fragment). Further, the sequencing reads themselves may be unstranded or strand-specific. Finally, paired-end protocols will have a specified relative orientation. To characterize the various different typs of sequencing libraries, we’ve created a miniature “language” that allows for the succinct description of the many different types of possible fragment libraries. For paired-end reads, the possible orientations, along with a graphical description of what they mean, are illustrated below:

!https://salmon.readthedocs.io/en/latest/_images/ReadLibraryIllustration.png

The library type string consists of three parts: the relative orientation of the reads, the strandedness of the library, and the directionality of the reads.

The first part of the library string (relative orientation) is only provided if the library is paired-end. The possible options are:

`I = inward
O = outward
M = matching`

The second part of the read library string specifies whether the protocol is stranded or unstranded; the options are:

`S = stranded
U = unstranded`

If the protocol is unstranded, then we’re done. The final part of the library string specifies the strand from which the read originates in a strand-specific protocol — it is only provided if the library is stranded (i.e. if the library format string is of the form S). The possible values are:

`F = read 1 (**or** single-end read) comes **from** **the** forward strand
R = read 1 (**or** single-end read) comes **from** **the** reverse strand`

So, for example, if you wanted to specify a fragment library of strand-specific paired-end reads, oriented toward each other, where read 1 comes from the forward strand and read 2 comes from the reverse strand, you would specify `-l ISF` on the command line. This designates that the library being processed has the type “ISF” meaning, **I**nward (the relative orientation), **S**tranded (the protocol is strand-specific), **F**orward (read 1 comes from the forward strand).

The single end library strings are a bit simpler than their pair-end counter parts, since there is no relative orientation of which to speak. Thus, the only possible library format types for single-end reads are `U` (for unstranded), `SF` (for strand-specific reads coming from the forward strand) and `SR` (for strand-specific reads coming from the reverse strand).

A few more examples of some library format strings and their interpretations are:

`IU (an unstranded paired-end library where the reads face each other)`

`SF (a stranded single-end protocol where the reads come **from** **the** forward strand)`

`OSR (a stranded paired-end protocol where the reads face away **from** **each** other,
     read1 comes **from** **reverse** strand **and** read2 comes **from** **the** forward strand)`

# Additional information

Should we remove duplicates? And what does this actually mean?

- [This thread](https://www.biostars.org/p/55648/) discusses whether you should remove read duplicates from RNA-seq data
- [Salmon](https://combine-lab.github.io/salmon/faq/) removes transcript duplicates but records all decisions it made
- More on [duplicates](https://github.com/COMBINE-lab/salmon/issues/214)
- RNA-seq [workflow](http://bioconductor.org/packages/release/workflows/vignettes/rnaseqDTU/inst/doc/rnaseqDTU.html#salmon-quantification)
- [Gene or transcript level?](https://github.com/crazyhottommy/RNA-seq-analysis/blob/master/salmon_kalliso_STAR_compare.md)Combine lab salmon [GitHub](https://github.com/COMBINE-lab/salmon) [Documentation](https://salmon.readthedocs.io/en/latest/)

# Importing Salmon output for DESeq2

- https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#data-transformations-and-visualization
- https://www.bioconductor.org/help/course-materials/2014/SeattleOct2014/B02.1.1_RNASeqLab.html#eda
- https://bioconductor.github.io/BiocWorkshops/index.html
- https://github.com/hbctraining/DGE_workshop_salmon

## Tximport

Tximport is a software to import and summarise transcript-level abundance estimates for transcript- and gene-level analysis with Bioconductor packages such as edgeR, DESeq2, and limma-voom. 

*> update 04/06/22*

### Introduction

Tximport is is especially important for transcript quantification performed with Salmon, Kallisto, and RSEM to convert data for DESeq2 friendly usage.

Some advantages of using the above methods for transcript abundance estimation are (i) this approach corrects for potential changes in gene length across samples (e.g from differential isoform usage), (ii) some of these methods are substantially faster and require less memory and disk usage compared to alignment-based methods that require creation and storage of BAM files, and (iii) it is possible to avid discarding those fragments that can align to multiple genes with homologous sequence, thus increasing sensitivity.

Full details on the motivation and methods for importing transcript level abundance and count estimates, summarising to gene-level count matrices and producing an offset which corrects for potential change in average transept length across sample are described in [Soneson, Love, and Robinson, 2015](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4712774/).

<aside>
💡 Note: another Bioconductor package, [Tximeta] extends Tximport, offering the same functionality, plus additional benefit of automatic addition of annotation metadata for commonly used transcriptome (GENCODE, ensemble, RefSeq for human and mouse). Whereas Tximport outputs a simple list of matrices, Tximeta will output a SummarizedExperiment object with appropriate Granges added if the transcriptome is from one of the sources above for human and mouse.

</aside>

Since we have used Salmon, we are interested in adding annotation of metadata from GENCODE (or ensemble if this was used as your reference genome). So, we will be using Tximeta instead of Tximport.

## Tximeta

Tximeta performs a numerous annotation and metadata gathering tasks on behalf of users during the import of transcript quantifications from Salmon into R/Bioconductor. Metadata and transcript ranges are added automatically, facilitating combining multiple genomic datasets and helping to prevent bioinformatic errors.

### Introduction

The Tximeta package extends the Tximport package for import of transcript-level quantification data into R/Bioconductor. It facilitates addition of annotation data when the RNA-seq data has been quantified with Salmon or for scRNAseq data quantified with Alvin.

<aside>
💡 Note: Tximeta requires that the entire output directory of Salmon/Alevin is present and unmodified in order to identify the provenance of the reference transcripts. In general , it’s a good idea not to modify or re-arrange the output directory of bioinformatic software as other downstream software rely on and assume a consistent directory structure.

</aside>

### Installing Tximeta

To install Tximeta, make sure you first have Rstudio setup correctly and that you have downloaded the R/Bioconductor package. I recommend creating a new R project to setup a new environment (similar to Anaconda). You will need this list of packages installed in this order:

1. [processx](https://cran.r-project.org/web/packages/processx/index.html)
2. [devtools](https://www.r-project.org/nosvn/pandoc/devtools.html)
3. [BiocManager 3.11](https://bioconductor.org/install/)
4. [rnaseqgene](https://bioconductor.org/packages/release/workflows/html/rnaseqGene.html)
Once you have done this, use this code to install Tximeta in RStudo (version “4.0”).

```bash
if (!requireNamespace("BiocManager", quietly = TRUE))
install.packages("BiocManager")
BiocManager::install("tximeta")
```

After this, we will need to load the packages so that our packages will work. You can either click to square to activate the packages or use the `library()` function.

### Building a Sample Table

The first step in using tximeta is to read in the sample table, which will become the *column data*, `colData`, of the final object, a `SummarizedExperiment`. The sample table should contain all the information we need to identify the
Salmon quantification directories.

# DESeq2

# Exploratory analysis and visualization

> There are two separate paths in this workflow; the one we will see first involves transformations of the counts in order to visually explore sample relationships. In the second part, we will go back to the original raw counts for statistical testing. This is critical because the statistical testing methods rely on original count data (not scaled or transformed) for calculating the precision of measurements.
> 

## Pre-filtering the dataset

<aside>
👌🏼 Optional!

</aside>

## Variance stabilising transformation and the rlog

Many common statistical methods for exploratory analysis of multidimensional data, for example clustering and principal components analysis (PCA), work best for data that generally has the same range of variance at different ranges of the mean values. When the expected amount of variance is approximately the same across different mean values, the data is said to be homoskedastic. For RNA-seq counts, however, the expected variance grows with the mean. For example, if one performs PCA directly on a matrix of counts or normalized counts (e.g. correcting for differences in sequencing depth), the resulting plot typically depends mostly on the genes with highest counts because they show the largest absolute differences between samples.

A simple and often used strategy to avoid this is to take the logarithm of the normalized count values plus a pseudocount of 1; however, depending on the choice of pseudocount, now the genes with the very lowest counts will contribute a great deal of noise to the resulting plot, because taking the logarithm of small counts actually inflates their variance.

As a solution, DESeq2 offers two transformations for count data that stabilize the variance across the mean: the **variance stabilizing transformation (VST)** for negative binomial data with a dispersion-mean trend (Anders and Huber 2010), implemented in the vst function, and the **regularized-logarithm transformation or rlog** (Love, Huber, and Anders 2014).

For genes with high counts, both the VST and the rlog will give similar result to the ordinary log2 transformation of normalized counts. For genes with lower counts, however, the values are shrunken towards a middle value. The VST or rlog-transformed data then become approximately homoskedastic (more flat trend in the meanSdPlot), and can be used directly for computing distances between samples, making PCA plots, or as input to downstream methods which perform best with homoskedastic data.

**Which transformation to choose?** The VST is much faster to compute and is less sensitive to high count outliers than the rlog. The rlog tends to work well on small datasets (n < 30), potentially outperforming the VST when there is a wide range of sequencing depth across samples (an order of magnitude difference). We therefore recommend the VST for medium-to-large datasets (n > 30). You can perform both transformations and compare the meanSdPlot or PCA plots generated, as described below.

Note that the two transformations offered by DESeq2 are provided for applications *other* than differential testing. For differential testing we recommend the *DESeq* function applied to raw counts, as described later in this workflow, which also takes into account the dependence of the variance of counts on the mean value during the dispersion estimation step.

Both *vst* and *rlog* return a *DESeqTransform* object which is based on the *SummarizedExperiment* class. The transformed values are no longer counts, and are stored in the *assay* slot. The *colData* that was attached to `dds` is still accessible:

```r
vsd <- vst(dds, blind = FALSE)
head(assay(vsd), 3)
colData(vsd)
```

Again, for the *rlog*:

```r
rld <- rlog(dds, blind = FALSE)
head(assay(rld), 3)
```

In the above function calls, we specified blind = FALSE, which means that differences between cell lines and treatment (the variables in the design) will not contribute to the expected variance-mean trend of the experiment. The experimental design is not used directly in the transformation, only in estimating the global amount of variability in the counts. For a fully unsupervised transformation, one can set blind = TRUE (which is the default).

To show the effect of the transformation, in the figure below we plot the first sample against the second, first simply using the log2 function (after adding 1, to avoid taking the log of zero), and then using the VST and rlog-transformed values. For the log2 approach, we need to first estimate size factors to account for sequencing depth, and then specify normalized=TRUE. Sequencing depth correction is done automatically for the vst and rlog.
