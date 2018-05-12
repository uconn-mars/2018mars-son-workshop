Intro to Microbial Communities
========================================================
author: Kendra Maas
date: Aug 15-19, 2016
autosize: true

Why are we here?
========================================================

 - Microbes control everything!^*
 - We can!

![Sequencing Costs by year](ncbi-genomes.png)

^* Well almost, sort of

Community profile
===============
- Conserved gene 
    - Balance homology and variability 
 ![16S varibility](16s.10.1093nar30.1.183.Tthe_B_SSU_var_150dpi.gif)

Community profile
===============
- Conserved gene
    - Balance homology and variability  
![16S varibility2](amplicon-sequencing-introduction-9-638.jpg)

Cartoon of wet lab process
===============

![wetlab](microbial_community_analysis.jpg)


Why are we here?
==============

 - What to do with MB-GB of sequencing data?
    - Great there are programs!
    - Oh they're only command line
 - How to make sense of complex communities?
    - Multivariate
    - Non-linear
    - Non-normal
    - Independent variables?


Why are we here?
=====================
- Command line
    - Learning curve
    - Customization
    - Learning curve

- **Reproducible Research**

Reproducible Research
=============
- Document process so it can be replicated
- Enable collaborative analysis
- Reduce time repeating analyses

Your most important collaborator is yourself 3 months ago and that collaborator doesn't answer emails.

Cautionary tale
==============
![](repro.research.ebola1.jpg)


Cautionary tale
==============
![](repro.research.ebola2.jpg)

Cautionary tale
==============
![](repro.research.ebola3.jpg)


mothur
=============
Detailed, well documented guide for mothur  
<http://www.mothur.org/wiki/MiSeq_SOP>

Download the RDP trainset and the Silva db

My scripts for working with sequence data  
<https://github.com/krmaas/bioinformatics/>

Etherpad for this workshop
<https://public.etherpad-mozilla.org/p/microbiome>

Basespace
==================

Download all samples in Workshop project

Seqs are barcode names, folder is client name  
I have a one-liner to deal with that but first 

bash terminal
============
Command | Action
--------|------
cd | change directory  
mv | move (mv -r move folder)  
ls (-lh) | list files (list details)  
less | print to screen  
ctl-shift-c | copy  
ctl-shift-v | paste

bash tools
========
Tool | function
---|---
sed | find and replace
awk | text manipulator 
vim | text editor


prep seqs for mothur
=========
Make files file  

sample  path/to/R1.fastq    path/to/R2.fastq

make.file()

Reduce PCR and seqeuencing errors
================

Command | Purpose | Finer Points  
--------|--------|-------
make.contigs() | Overlap forward and reverse read | Similar to Pandaseq, q score difference not cutoff
summary.seqs() | Check length distribution  |  
screen.seqs(maxambig, maxlength) | Remove ambiguous bases, too short/long seqs | We remove any seq with any N, length trim *could* have phylogenetic bias  

Housekeeping
============
Command | Purpose | Finer Points  
--------|--------|-----  
unique.seqs() | Reduces computation cost | count replicats but remove from fasta  
count.seqs() | Reduces computation cost | replaces names and groups  
summmary.seqs() |  |  
pcr.seqs() | create database w/ just v4 | only need to do once per target
system(mv) | call bash commands within mothur |  
summary.seqs() | check seqs in silva alignment |  

Continue cleaning seqs
==================
Command | Purpose | Finer Points  
--------|--------|-----  
align.seqs() | Align to reference | Any alignment is acceptable  
summary.seqs() |  |    
screen.seqs(start, end, maxhomop) | remove seqs misaligned, homopolymers | Further cleaning PCR/sequencing errors using secondary structure 
filter.seqs(vertical) | remove columns that are all gaps | trump=T removes columns if any gap  
summary.seqs() |  |  

Chimera checking
=============
Command | Purpose | Finer Points  
--------|-------- | -----  
pre.cluster(diffs) | cluster at 1% differences to reduce computation cost | greedy clustering, balance reducing data w/maintaining diversity  
summary.seqs() |  | Check change in unique sequences
chimera.uchime(dereplicate) | de novo find chimera within each sample | dereplicate, remove chimera from all samples?  
remove.seqs() | | defaults remove all flagged seqs  
summary.seqs() | |  

OTU clustering
============  
Command | Purpose | Finer Points  
--------|--------|-----  
classify.seqs(reference, taxonomy) | | default Wang/RDP  
remove.lineage(taxon) | remove wetlab bycatch | Chloroplast-Mitochondria-unknown-Archaea-Eukaryota  
summary.tax() | | file for Krona plots  
cluster.split(splitmethod, taxlevel, cutoff) | heuristic cluster within taxon | limiting by classification reduces computation without greedy  
make.shared(label) | create OTU table | uneven seqs/sample  

Alpha and Beta diversity
=============  
Command | Purpose | Finer Points  
--------|--------|-----   
count.groups()  |   | determine subsample level  
summary.single(calc, subsample)| alpha diversity |  
dist.shared(calc, subsample, iters) | beta diversity | reduce iters if in a hurry  
classify.otu() | attach names to OTUs | Doesn't impact clustering  
get.oturep(method) | abudance or centroid |  
sub.sample(shared, size) | | For feature selection  
