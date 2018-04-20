# Week 4: Why don't snakes have legs? (Part 2)
Skills covered: Epigenomics, accessing public databases, multiple sequence alignment, motif analysis

TODO intro:
zoom in on Shh, should have been 2nd hit from analysis last time
analyze well known enhancer region ZRS
human disease, limb develpoment
today: look at putative enhancers nearby, analyze across species, determine motif/mechanism

## 6. Zooming in on ZRS

Launch IGV and load the session you started last Tuesday. You should have already 6 tracks: 2 for each RNA-seq replicate of HL, FL, and MB. Additionally the default Refseq genes should be present at the bottom. We will be focusing today on the "Sonic hedgehog" region. In the search bar at the top, navigate to region chr5:28,278,817-29,447,265.

We'd like to identify potential regulatory regions for the differentially expressed gene *Shh*. Our labmates have generated some ChIP-sequencing data that will be useful for identifying putative enhancers in this region. We'll start by adding the ChIP-seq datasets and some additional tracks to IGV to help us interpret some features of this region.

In the `public/week4` directory, you'll find (bedGraph)[https://genome.ucsc.edu/FAQ/FAQformat.html#format1.8] files (`*.bedGraph`) for ChIP-sequencing experiments for H3K27ac and H3K4me1 (marks found near enhancer regions). Use `scp` to copy these to your desktop and load the files to IGV. Note these have been restricted to have data in our region of interest, so you if you scroll outside this region you won't see any data.

First take a look at where these marks (H3K27ac and H3K4me1) are falling. Are they near gene regions? Beginning or ends of genes? Other places? Discuss this in the results section of your lab report.

Before moving on, let's add one additional track to IGV about sequence conservation. For this, we'll load the PhyloP track, which gives a per-base pair measure of sequence conservation across species (see Wednesday slides). You can get the PhyloP track from the genome.ucsd.edu (another genome browser! which you can load tracks to similar to IGV.). As part of the genome browser, UCSC hosts many different "tracks" of information for many different genome builds. If you are ever looking for information on any sort of genomic annotation (e.g. gene annotations, conservation, genetic variation, and more), UCSC is a good place to start.

Use the table browser to download the PhyloP track for our region of interest. From the home page, select "Tools->Table Browser". use the dropdown boxes to select the mouse mm10 genome build. Additionally choose the following options:
* Group: Comparative Genomics
* Track: Conservation
* Table: 60 Vert. Cons (phyloP60wayAll)
* Region: position chr5:23414443-36455956
* Filter: edit and select "Limit data output to" 10 million lines otherwise the output will be truncated
* Output format: custom track
* Output file: put a reasonable filename (e.g. `phyloP60wayAll_mm10_Shh_region.wig`)
After you click "submit" make sure "DATA VALUE" is selected on the next page, then click "Get custom track in file" which will download the file to your computer's default download location.

It's worth taking a second to go back to the table browser and see what kinds of info can be downloaded from here. It is a really flexible tool for a huge number of published genome-wide data tracks!

Now go back to IGV and load the PhyloP file. What regions seem to have highest PhyloP scores? Are there any highly conserved regions that are not protein-coding? Hypothesize what those might correspond to. Include a brief description of what you observe in your lab report.


## 7. Multiple sequence alignment of enhancer sequences

mafft for MSA
mafft --auto ../../public/week4/zrs_sequences_evgeny.fa > zrs_sequences_msa.fa
visualize with MVIEW https://www.ebi.ac.uk/Tools/msa/mview/

## 8. Motif analysis 
FIMO on the mouse sequence
what motif corresponds to the missing region

## 9. For your lab report

<blockquote>
**UNIX TIP**: template
</blockquote>
