# Week 4: Why don't snakes have legs? (Part 2)
Skills covered: Epigenomics, accessing public databases, multiple sequence alignment, motif analysis

TODO intro:
zoom in on Shh, should have been 2nd hit from analysis last time
analyze well known enhancer region ZRS
human disease, limb develpoment
today: look at putative enhancers nearby, analyze across species, determine motif/mechanism

Lot of web tools rather than command line today. learn about resources/where to get data/vis tools. will publish commands we used to get a lot of these datasets for your info

## 7. Loading more info to IGV
Launch IGV and load the session you started last Tuesday. You should have already 6 tracks: 2 for each RNA-seq replicate of HL, FL, and MB. Additionally the default Refseq genes should be present at the bottom. We will be focusing today on the "Sonic hedgehog" region. In the search bar at the top, navigate to region chr5:28,278,817-29,447,265.

We'd like to identify potential regulatory regions for the differentially expressed gene *Shh*. Our labmates have generated some ChIP-sequencing data that will be useful for identifying putative enhancers in this region. We'll start by adding the ChIP-seq datasets and some additional tracks to IGV to help us interpret some features of this region.

In the `public/week4` directory, you'll find (bedGraph)[https://genome.ucsc.edu/FAQ/FAQformat.html#format1.8] files (`*.bedGraph`) for ChIP-sequencing experiments for H3K27ac and H3K4me1 (marks found near enhancer regions). Use `scp` to copy these to your desktop and load the files to IGV. Note these have been restricted to have data in our region of interest, so you if you scroll outside this region you won't see any data.

First take a look at where these marks (H3K27ac and H3K4me1) are falling. Are they near gene regions? Beginning or ends of genes? Other places? Discuss this in the results section of your lab report.

Before moving on, let's add one additional track to IGV about sequence conservation. For this, we'll load the PhyloP track, which gives a per-base pair measure of sequence conservation across species (see Wednesday slides). You can get the PhyloP track from the genome.ucsc.edu (another genome browser! which you can load tracks to similar to IGV.). As part of the genome browser, UCSC hosts many different "tracks" of information for many different genome builds. If you are ever looking for information on any sort of genomic annotation (e.g. gene annotations, conservation, genetic variation, and more), UCSC is a good place to start.

Use the Table Browser (`Tools > Table Browser`) to download the PhyloP track for our region of interest. From the home page, select "Tools->Table Browser". use the dropdown boxes to select the mouse mm10 genome build. Additionally choose the following options:
* Group: Comparative Genomics
* Track: Conservation
* Table: 60 Vert. Cons (phyloP60wayAll)
* Region: position chr5:23414443-36455956
* Filter: edit and select "Limit data output to" 10 million lines otherwise the output will be truncated
* Output format: custom track
* Output file: put a reasonable filename (e.g. `phyloP60wayAll_mm10_Shh_region.wig`)
After you click "get output" make sure "DATA VALUE" is selected on the next page, then click "Get custom track in file" which will download the file to your computer's default download location.

It's worth taking a second to go back to the table browser and see what kinds of info can be downloaded from here. It is a really flexible tool for a huge number of published genome-wide data tracks!

Now go back to IGV and load the PhyloP file. What regions seem to have highest PhyloP scores? Are there any highly conserved regions that are not protein-coding? Hypothesize what those might correspond to. Include a brief description of what you observe in your lab report.

## 8. Zooming in on ZRS

Use IGV to zoom in on region chr5:29,314,718-29,315,770. This region corresponds to the ZRS (Zone of polarizing activity regulatory sequence, also called MFCS1) which is one of the most deeply studied mammalian enhancer sequences known to regulate the *Shh* gene. Take note of the histone modification and conservation patterns at this locus. Is it well conserved across species? Based on the histone modifications, for which tissues does this look like a putative enhancer region? Take a screen shot of this region and include it as well as a description in your lab report.

## 9. Multiple sequence alignment of enhancer sequences

After discussing with your labmates and reading about the ZRS enhancer, you're convinced this is likely an important region involved in limb development. You also recall that the PhyloP scores showed this region is highly conserved. To study how this region differs across species with and without limbs, you look for regions similar to the mouse ZRS in other organisms. Remarkably, you are able to identify similar regions in human, mouse, cow, dolphin (with limbs) and python, rattlesnake, cobra, and boa (snakes without limbs). These are all collected in the file `zrs_sequences.fa` in the `public/week4` folder.

`cat` this fasta file to look at sequences. Each fasta header line gives more info on which genome build and coordinates each region was taken from.

We'd first like to perform multiple sequence alignment (MSA) to see exactly how similar these regions are across species. For that, we'll use the `mafft` tool. Type `mafft --help` at the command line to see usage information for this tool. It should take a fasta file of sequences as input and produce a similar fasta file with the MSA as output. Examine the output file. What do the "-" characters mean?

Now, we will use the Mview tool to visualize the alignment. You can either do this using the web version at https://www.ebi.ac.uk/Tools/msa/mview/ or using the command line `mview` tool. Type `mview --help` to see full usage details. The following command:
```
mview -html full my_msa.fa > my_msa.html
```
will produce a (not very colorful) html file to visualize the MSA. Play around with the options to make the plot nicer and more colorful. Visualize the resulting html file in your web browser.

Do you notice any regions that are conserved in all species except snakes? Take a screenshot of those regions.

You should be able to find at least one region that is deleted in all snakes but conserved across all other species. Extract that region plus surrounding sequence (extract ~20-30bp total) from the mouse ZRS sequence.

## 10. Motif analysis
You hypothesize that the region deleted in snakes might be forming a binding site for a transcription factor that binds to this enhancer. To find out what might be binding there, we can use the `fimo` tool to scan our sequence for any matching motifs. Make a fasta file `mouse_del_region.fa` with the 20-30bp extracted above and run `fimo` to scan for motifs:

```
fimo ../../public/week4/motif_databases/MOUSE/HOCOMOCOv11_full_MOUSE_mono_meme_format.meme mouse_del_region.fa
```
This file scans for motifs in the HOCOMOCO mouse database in the `public/week4` directory. This will create a folder `fimo_out`. Look at the file `fimo_out/fimo.txt` to find motifs with significant matches.

You will likely have many hits. Probably not all of these transcription factors are actually bound to the DNA at this region. For example, many of the top factors are not even expressed in any of the cell types we analyzed here.  Do you have any hypotheses about which factors are most relevant, or what further experiments or analyses could be done to determine which if any of these factors is relevant? Include this in the discusson of your lab report.

## 11. For your lab report

For this week's lab report, there are specific prompts and instructions included in the template document in the `labreports` folder. Each section lists how many points it will be worth, so be sure to complete all the items listed there.
