# 16S rRNA Sequencing

# Piphilin

Currently deactivated!

# PUMA

<aside>
💡 Don't install PUMA on the HPC cluster - the packages are configured for a Mac system and HPC is noarch or Linux. Have to install on personal Mac or create a partition on your windows.

</aside>

Make sure you have Anaconda installed on your Mac. 

Then download the git repository to a local folder (I placed mine in Documents) using `git clone https://github.com/keithgmitchell/PUMA.git` in the terminal.

Activate conda, then check if you have both conda-forge and bioconda channels installed. To do this use `conda config --append channels conda forge` and `conda config --append channels bioconda` .

Navigate to the downloaded PUMA folder; `cd PUMA`.

Once you have done this, create the new PUMA environment using `conda create --name puma --file requirements.txt`.

Once the environment has been created, activate the environment using `conda activate puma`. Then run `pip install -r pip_requirements.txt; mkdir temp`. 

To run PUMA with Piphillin files, use the command `python [RunPuma.py](http://runpuma.py/)`

Submit your metadata file (.txt) and your zip folder containing the pathway and feature Piphillin output. 

# STAMP

[STAMP](https://ucla.app.box.com/s/tydb8aeu1txiwjpuhves28j73oqla418) is a program that allows us to performs a variety of statistical analysis on our metagenomic profiles. Due to this being an older program, we need python 2.7 to run it. Since python 2.7 is no longer in use, we need to use Anaconda to create a special python 2.7 environment to allow us to run STAMP. 

To install STAMP, you need to make sure you have a copy of python 2.7 on your anaconda following [this guide](https://docs.anaconda.com/anaconda/user-guide/tasks/switch-environment/). After, use the code below to create a new stamp environment using python 2.7. If you come across any issues, look at [this chat](https://github.com/dparks1134/STAMP/issues/34). 

```bash
*conda create -n stamp python=2.7
conda activate stamp
pip install numpy
pip install cython
pip install biom-format==2.1.7
pip install STAMP
conda install -n stamp -c asmeurer pyqt=4
STAMP*
```

Once you have installed STAMP successfully, use the command `STAMP` to run STAMP. On the main interface, select File > Load Data. Under 'Profile file' insert either your pathways and gene abundance file from the PUMAA output. 

### Pathways

Parent level: Entire Sample

Profile level: I prefer to use Pathway L2 as this provides a nice description of pathways involved but is not too broad. 

On the right hand side under group legend, change this to group to see your experimental and control categories. 

If you have two groups, use two group analysis. If you have more than two groups, use multiple group analysis. 

For multiple analysis, I usually use the statistical settings:

- Statistical test: ANOVA for parametric and Kruskal-Wallis H-test for non-parametric
- Post-hoc test: Tukey-Kramer with at least a 0.95 confidence interval.
- Effect size: ETA-squared
- Multiple corrections: Storey's FDR (more powerful method than Bonferroni and Benjamin-Hochberg FDR) - but sometimes if the samples are not uniformly distrubuted, use BH FDR instead.

Under filtering, set the 1-value filter to 0.05.

You can also select specific features to show, which i would recommend if you would like to show pathways related to certain categories individually.