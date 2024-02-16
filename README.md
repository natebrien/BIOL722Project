# BIOL722Project
Bioinformatics course final project - analysing methylation data

## Overview
This project will use simulated ancient DNA sequences (using *gargammel*) to compare the outputs of two ancient DNA methylation tools, *epiPALOMIX* and *DamMet*. 

## Methods

### Simulating aDNA sequence data using *gargammel*
  Gargammel will be used to simulate aDNA sequence data for this project. Although gargammel has the capability to simulate both modern human and bacterial contamination in the sample, only the endogenous human sequences will be simulated. The resulting simulated sequences have fragmentation patterns and deamination patterns characteristic of aDNA, and have Illumina adapters added to simulate raw sequencing reads. Gargammel does not simulate quality scores, so the output is in .fa format. Gargammel also does not simulate the effects of bisulfite treatment. 

### Bioinformatic USER treatment of simulated sequence data
  USER treatment (UDG + endoVIII) is used to correct for deamination profiles in aDNA. USER (a specific enzyme sold by NEB) has a combination of uracil deglycosylase and endonuclease VIII activity, which excises uracils and creates single-nucleotide gaps in the phosphodiester backbone, respectively. Since deamination of unmethylated cytosines both occurs at a lower frequency than methylated cytosines, and occurs primarily at the ends of fragments, the majority of these uracils will be present within the first and last 5 bp of reads. Therefore, a computational USER treatment can be performed by trimming the ends of reads. 

### Regional methylation estimation using *epiPALEOMIX*

### Regional methylation estimationg using *DamMet*

## Discussion and Conclusions
