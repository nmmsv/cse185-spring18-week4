# Week 4: Why don't snakes have legs? (Part I)
Skills covered: Genome browsers and data visualization, RNA-seq analysis

You are interested in determining which genes and regulatory regions are most important for controlling the development of limbs (legs and arms), and whether those regions of the genome might explain why some animals are missing limbs (e.g., most snakes). You decide to first tackle this question by analyzing tissues from mouse early development. 

Today, you will use RNA-sequencing data to perform differential expression analysis.
On Thursday, you will use additional epigenomics datasets to further analyze a limb-specific enhancer sequence and trace it's evolution across distantly related species.

You reason that by comparing expression in limb tissues vs. non-limb you can determine genes specific to pathways controlling limb development. You go ahead and collect samples for three tissues from a developing mouse: hind limb (HL), fore limb (FL), and mid-brain (MB). For each tissue, you collect two replicates so you can perform more robust differential expression analysis.

## 1. Data inspection and quality control

As in previous weeks, start by making a clone of your repository on `ieng6`:

```
git clone https://github.com/cse185-sp18/cse185-week4-<username>.git week4
```

Data for this week can be found in the `public/week4` directory. Among other things, you can find fastq files for our RNA-sequencing experiments there:

* Forelimb: `FL_Rep1_chr5_*.fq.gz`, `FL_Rep2_chr5_1.fq.gz` 
* Hindlimb: `HL_Rep1_chr5_*.fq.gz`, `HL_Rep2_chr5_1.fq.gz` 
* Midbrain: `ML_Rep1_chr5_*.fq.gz`, `ML_Rep2_chr5_1.fq.gz` 

(Note: these reads have been downsampled from the original experiment to only contain chromosome 5)

First, take a look at the fastq files. **Do not unzip them!** See the UNIX tip below for how to deal with `.gz` files. What read length and how many reads were used for each experiment? Record the answer in your lab notebook and describe in the methods section of your report.

<blockquote>
**UNIX TIP**: Using a compression method like `gzip` or `bgzip` can save tons of space when dealing with huge files. Gzipped files aren't directrly human-readable. However, you can use the `zcat` command to write the contents of the file to standard output. For instance, to see the head of a gzipped file, you can do `zcat file.gz | head`. You can similarly pipe the output of `zcat` to other commands like `wc`.
</blockquote>

Run `fastqc` on the fastq files. You do not need to include the figures in your lab report, although you should keep track of the output `html` files. Comment on anything flagged as problematic in the methods section of your report.

## 2. RNA-seq sequence alignment

Since we'd like to visualize where our RNA-seq reads are falling in the genome, we'll need to align the reads to a reference transcriptome. Depending on the alignment method used, this step can either consume tons of memory (e.g. STAR) or take a long time to run (e.g. TopHat). Luckily, you are working with a bioinformatician in the lab who has already gone ahead and aligned the reads to the latest mouse reference for you.

The BAM files containing alignments can be found in the `week4` directory (`*_chr5.bam`).

Unforunately, your labmate forgot to write down some details about how they performed the alignment. In particularly, we'd like to know:

* What program and command did he or she use to align the reads?
* What reference genome were the sequences aligned to?

You wonder if the BAM header files might have some more information about how those files were generated. Type
```
samtools view
```
to learn how to output the header of a BAM file. Take a look at the [SAM specification](https://samtools.github.io/hts-specs/SAMv1.pdf) to see a description of different standard header tags (top of page 5). Note the "@PG" tag gives info about the program that was used, and many tools (including the one used here) will use that tag to document the exact command used to generate the BAM file.

Take a look at reads, for instance by doing `samtools view FL_Rep1_chr5.bam`. If you scroll down, you'll notice the CIGAR scores have some extra characters in them we haven't seen before (See week 1 slides for a refresher on CIGAR scores). In the past, we have seen "M" for match, "I" for insertion, and "D" for deletion. Now we see many reads have "N" in the CIGAR scores (e.g. read ID SRR3950230.31710737). In the SAM specification, go to page 6 to read more about CIGAR scores and find out what "N" represents. What biological feature do you think the "N"'s stand for?

<blockquote>
**UNIX TIP**: `less` is really helpful for looking at and scrolling through files. A helpful way to visualize a sam file is to run `samtools view file.bam | less -S`. The `-S` parameter tells the terminal not to wrap lines, and instead allow you to scroll through long lines horizontally. This makes files with long lines much more readable. Another trick: once you're looking at a file using `less`, you can use `ctrl-v` to scroll down more quickly than using the down button.
</blockquote>

## 2. Visualizing data using a genome-browser


make tdf (igvtools)

download+Open IGV and load tdf + ref genome

look at the region from above with "N"s there. see that it's at a splice site

look at example region (MECOM locus from paper)
see differential expression across samples


## 3. Quantifying gene expressiom

kallisto (do for rest of replicates, make them do 2 of them (1 HL, MB)

compare replicates (pairwise r2 of everyone)

## 4. Differential expression analysis
run sleuth in R

## 5. Global changes in gene expression
convert to gene names
identify top ones
heatmap + GO Analysis?

