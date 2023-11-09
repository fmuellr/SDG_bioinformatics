# Functional analysis

>  🛠️ Nov 2023

This is an overview of carrying out functional analysis of a gene list of interest. This is particularly useful for example, if you have a list of differentially expressed genes (DEGs) and you are trying to identify what is their function, where are they located etc. 

There are two main programs I recommend for this analysis and I will go through how to use both of them. The first is using the (Database for Annotation, Visualization, and Integrated Discovery)[https://david.ncifcrf.gov], also known as **DAVID**. The second is a program run in the networking App (Cytoscape)[https://cytoscape.org] called **ClueGO**. Both have their pro's and con's, so I usually carry out both sets of analysis and decide which is best for what and how I want to show my data. 

Finally, I will go through how to visualise the results using barcharts, heatmaps, or dotplots. 

## DAVID functional analysis

One of the main benefits of using DAVID is the vast amount of categories that you obtain as well as various statistical and functional analysis options. However, this is also one of its downsides, meaning it requires more effort and time for appropriate visualisation. As well, due to the nature of how terms are names, particularly when using functional annotations known as KEGG pathways, terms can be non-descriptive or false-descriptive (i.e., the name is not appropriate to your list of gene), but more on this later.

First, lets navigate to the (DAVID homepage)[https://david.ncifcrf.gov]. The click the tab at the top that says *Start Analysis*. You are now presented with a screen divided into two sections: on the left you have three tabs called *Upload*, *List*, and *Background* where you input your data. On the right, you will see the output from your analysis. 

![David_start](./Images/David_a.png)

Select your list of interest, for example a list of DEGs. I usually split these into upregulated and downregulated because I am interested in which categories are specifically associated with these different gene states, but this might change depending on your question.

> When selecting your list, you will need to choose the appropriate significance level. Generally, when publishing papers the desired significance cutoff is either FDR<0.1 or FDR<0.05. However, sometimes we use p-value when FDR is too stringent and we loose too many genes. In this case, you could try a p-value<0.001 with a fold change cutoff

    - You can test different levels of significance. I usually start with FDR<0.1. However, if you have many genes in your list (>1000), you might want to be more stringent and use, for example, FDR<0.05. If you have very few genes when using FDR (<20), it might be worth trying p-value<0.05 + LogFoldChange cutoff (we commonly use ±0.58, as Log2FoldChange(0.58)=Fold change of 1.5). 


On the left is where you input your data. Make sure you are first in the upload tab. 
Select your upregulated genes first (or whichever genes are of interest – I usually split them into upregulated and downregulated and run the analysis separately). I usually do different levels of significance. I start with FDR<0.1. if I don’t have enough genes (<20).  I do Pvalue<0.05 +Fold change>+/-0.58; and finally if I still don’t have enough genes, I do pvalue <0.05 (but this is not recommended, most reviewers want FDR, but unfortunately this is not always possible due to sequencing depth). 
I usually paste the ensemble ID, as the gene name can cause errors. Since you only have gene name, (we can convert them using BioMart either from the ensemble website, or if you have a big list using R – I will attach the script). But lets skip this step for now for ease. 
Paste your gene name in to the “Paste a list” space. 
Then under Step 2: select identifier, choose Official Gene Symbol. And then you get a box for the species, and put mus musculus. Then under step 3: list type select Gene List. 
Then click submit list. 

## ClueGO functional analysis in Cytoscape

## Alternative programs to use

revigo
panther
