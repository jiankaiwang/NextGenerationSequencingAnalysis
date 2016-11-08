# Next Generation Sequencing Workflow

### Requirement

* Python 2.7
* Perl 5v22
* R 2.3

### Workflow in NGS analysis

![](https://github.com/jiankaiwang/NextGenerationSequencing/blob/master/doc/images/20140318_finalflow.png)

### Description

| Code | Description | 
|---|---|
| extractLargeData.pl | To extract paired reads on the basis of the policy. The input is the divided file (due to limitation of the memory) from the aligned SAM one by bowtie programs. |
| getLargeData.pl | To count each transcript from all divided files and generate one file containing both transcript and its read count. The counting of input files would be dynamic changed due to the limitation of memory. |
| cbnCT.pl | To combine both control and treatment files generated by code, getLargeData.pl, transcript level data would show the read counts of both control and treatment simultaneously. |
| preprocess.py | The code combined (1) dividing origin alignmant file from bowtie into several subfiles (it depends on the size of memory) and (2) combining the above extractLargeData.pl, getLargeData.pl and cbnCT.pl with the Linux commands. So this code must be executed on the Linux environment. |
| extractGene.pl | To simplify the description of transcript name into the format consisting of ensembl gene name, transcript read count and control read count. |
| analysisGene.r | To normalize sets of the control and the treatment, and the output would be several results by analyzing methods, such as fold change, expression difference and probability (respresenting whether the difference is significant in the whole system). The used package for normalization in R was NOISeq. |
| gene_deep.r | Due to the incomplete analyses of fold change, expression difference or probability, it was necessary to calculate the trend line, to analyze the item far away from the line and to represent significant changes (differences) between the control and the treatment with normalization. Such analyzing method could be achieved by plotting within the code. |
| analysis.r | The similar processing with analysysGene.r was trying to normalize sets of the control and the treatment. But targets were transcripts, not genes. The package used in R was NOISeq. The output would be fold change, expression difference and probability. |
| extractGeTreData.py | The code was optional for executing and trying to extract the transcript name, gene name from the transcript label so as to simplify the output generated by analysis.r and to read the data more easily. |
| combineTtl.r | The similar processing with gene_deep.r was trying to plot the result (the control and treatment normalization) generated by analysis.r. The purpose of combinettl.r was to analyze the item whose expression difference between the control and the treatment was significant. |
| getTranscript.r | Both gene_deep.r and combineTtl.r would each output several potential items on the gene and the transcript level. These potential items of both levels did not mean that they were higher probability on the difference between the control and the treatment. Due to several reasons, it was necessay to extract items which were higher normalization and higher fold change (similar with higher difference). The code was trying to combine outputs from the control and the treatment by intersecting them in order to find items whose were potential hits on transcript and gene levels. |
| filterDb.py | Due to the limitation of the memory, the annotated file (.GTF) from GENCODE would be impossible to load the entire file. The code was designed to extract the potential data from the annotated database on the basis of the gene name in getTranscript.r. The process was first to get total gene name from the result generated by getTranscript.r and then executed the pattern matching in annotated file to extract potential entries to a new database. |
| annotated.pm | The self-designed package used in Perl environment was constructed by data type of "class" and was implemented by pointer. The package defined the column needed to be stored. And the package would be used in another perl code named getAnnotated.pl. |
| getAnnotated.pl | The code used the package, annotated.pm, and was implemented by dynamic allocation memory. The code would output the potential items with their transctipt level, control level and annotated data. The result would be further analyzed by biological experiments. |


