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

> When selecting your list, you will need to choose the appropriate significance level. Generally, when publishing papers the desired significance cutoff is either FDR<0.1 or FDR<0.05. However, sometimes we use p-value when FDR is too stringent and we loose too many genes. In this case, you could try a p-value<0.001 with a fold change cutoff such as ±0.58, as Log2FoldChange(0.58) = Fold change of 1.5. 

Once you have selected your list and your significance, select either the *Gene Name* or the *Ensembl ID* and paste this into the **Step 1: Paste a list** space. 

> If you are missing the gene name, you can use the (Ensembl BioMart)[https://www.ensembl.org/biomart/martview/3fb321400d44779e551346dc093f71c6] to convert the Ensembl ID into gene name. You can also do this in R. 

Once you have pasted the list, we now need to select the appropriate identifier under **Step 2: Select Identifier**. If you have used the Ensembl ID, select **ENSEMBL_GENE_ID**. If you used the gene name, select **OFFICIAL_GENE_SYMBOL**. When using the gene name, you also have an extra step where you need to provide your species of interest, for example Mus Musculus. 

> Sometimes, using the gene name can cause errors, in that case use the Ensembl ID. 

Finally, under **Step 3: List Type** select **Gene List** and then press **Submit** under Step 4.

![](./Images/David_b.png)

Now, if you navigate to the *List* tab, you will see it has uploaded your list. Next, we need to upload a **Background**, this is important, because while we can use the provided mus musculus reference, depending on your tissue type, this can slightly vary. The only downside is that because the background is a very large list of genes (usually >20,000), it only lets you submit it using Ensembl ID, meaning that you can't use the Gene name for your gene list. To create your background is very straight forward, select all genes (both DEGs and nonDEGs) from your entire dataset and same the Ensembl ID only in the first column of a .txt file. 

![](./Images/David_c.png)





## ClueGO functional analysis in Cytoscape

## Alternative programs to use

revigo
panther
