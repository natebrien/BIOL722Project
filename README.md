# BIOL722Project
Bioinformatics course final project - analysing methylation data

## Overview
This project will use simulated ancient DNA sequences (using *gargammel*) to compare the outputs of two ancient DNA methylation tools, *epiPALEOMIX* and *DamMet*. 

## Methods Summary

First, the program gargammel was used to simulate aDNA sequence data (Renaud et al., 2017). Then, trimming and filtering of reads was performed using fastp (Chen et al., 2018). Bioinformatic USER treatment was performed using the command line tool awk and the package seqtk. USER and non-USER treated sequences were each aligned to the human genome using the Burrows-Wheeler backtrack algorithm (aln) (Li & Durbin, 2009), and using ancient settings (Karpinski et al., 2020) to generate SAM and BAM files. Following alignment, damage profiles of both USER and non-USER treated sequences were generated using mapDamage2 (Jónsson et al., 2013). BAM files from alignments of non-USER and USER treated sequences were used to generate methylation estimates in epiPALEOMIX (Hanghøj et al., 2016) and DamMet (Hanghøj et al., 2019). 

## Structure of the Repository

The reproducible code used to run these analyses can be found in the files titled "Step 1", "Step 2" and so on. Also included is the "code notebook" (titled "Daily Notes") which are my rough notes taken while doing research, analysing, troubleshooting, etc.
