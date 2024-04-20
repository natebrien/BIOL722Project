## Background on gargammel.
Gargammel will be used to simulate aDNA sequence data for this project. Although gargammel has the capability to simulate both modern human and bacterial contamination in the sample, only the endogenous human sequences will be simulated. The resulting simulated sequences have fragmentation patterns and deamination patterns characteristic of aDNA, and have Illumina adapters added to simulate raw sequencing reads. Gargammel putputs gzipped fastq files. Gargammel also does not simulate the effects of bisulfite treatment.

Reference: Renaud, G., Hanghoej, K., Willerslev, E. & Orlando, L. (2016). gargammel: a sequence simulator for ancient DNA _Bioinformatics_, btw670. 

### Prep for running garammel
Infoserv has gargammel installed through conda, therefore the necessary conda environment needs to be activated before running gargammel or it will throw an error. Activate the gargammel conda env by running: 

	$ conda activate /home/sam/miniconda3/envs/gargammel

If this works correctly you should see (gargammel) before [yourname@info2020]. 

### Parameters for gargammel
We want gargammel to create simulated ancient DNA fragments with methylation and deamination damage. To do this we need to run gargammel using the following flags (with accompanying explanations).

	- o
	- n 1000
	- l 35
	- damage [0.024, 0.36, 0.0097, 0.68]

These flags specify (in order): the output directory, the number of fragments to simulate, the fragment length, to add methylation-specific damage and the damage parameters. 

I'm using n=1000 because a general shotgun sequencing run will generate 1M reads, but only a small portion of those reads will be endogenous, sometimes as small as 1%. I'm also using a fragment length of 35 based on FLDs from ancient DNA projects that typically indicate that counts peak around the 30's. The deamination damage patterns are taken from the publication Briggs et al. 2007 which is recommended in gargammel documentation. The values represent the following: 

| Values | Meaning |
| ------ | ------- |
| 0.024  |         |
| 0.36   |         |
| 0.0097 |         |
| 0.68   |         |


Ran: 
`$ time gargammel -o results/sim_data -l 35 -n 1000 -damage 0.024,0.36,0.0097,0.68 data`

Files output: 
-rw-rw-r-- 1 natassja natassja  59407 Mar  8 11:59 sim_data_a.fa.gz
-rw-rw-r-- 1 natassja natassja      0 Mar  8 11:59 sim_data.b.fa.gz
-rw-rw-r-- 1 natassja natassja      0 Mar  8 11:59 sim_data.c.fa.gz
-rw-rw-r-- 1 natassja natassja  47209 Mar  8 11:59 sim_data_d.fa.gz
-rw-rw-r-- 1 natassja natassja  48296 Mar  8 11:59 sim_data.e.fa.gz
-rw-rw-r-- 1 natassja natassja  96092 Mar  8 11:59 sim_data_s1.fq.gz
-rw-rw-r-- 1 natassja natassja  99841 Mar  8 11:59 sim_data_s2.fq.gz
## Quality control checks
Quality control at this stage means checking that the fragmentation and deamination patterns of the simulated sequences match what we would expect from ancient DNA. 

#### FastQC
Run fastqc on simulated data files sim_data_s1.fq.gz and sim_data_s2.fq.gz:
`$ fastqc results/sim_data_s1.fq.gz results/sim_data_s2.fq.gz --outdir quality_control/`

Output: 
-rw-rw-r-- 1 natassja natassja  915289 Mar  8 12:23 sim_data_s1_fastqc.html
-rw-rw-r-- 1 natassja natassja 1754191 Mar  8 12:23 sim_data_s1_fastqc.zip
-rw-rw-r-- 1 natassja natassja  931640 Mar  8 12:23 sim_data_s2_fastqc.html
-rw-rw-r-- 1 natassja natassja 1804408 Mar  8 12:23 sim_data_s2_fastqc.zip

FastQC does not help us determine whether sequences are deaminated properly or not. 

#### Fastp
Fastp is used to trim, merge, and quality filter reads. Reads need to be trimmed and filtered before they are suitable for input to bwa. 

#### bwa-mem

Ran: 
` bwa mem GRCh38.p14.genome.fa /2/scratch/natassja/Bio722/term_project/gargammel/quality_control/sim_data_s1_trimmed.fq /2/scratch/natassja/Bio722/term_project/gargammel/quality_control/sim_data_s2_trimmed.fq > sim_aln.sam
#### mapDamage

Install mapDamage: 

`conda create -n mapdamage-env`

`conda install -c bioconda mapdamage2`

Activate environment: 

`conda activate mapdamage-env`

Run mapDamage: 

`mapDamage -i sim_aln.sam -r GRCh38.p14.genome.fa`

Results: 

![](./Pasted%20image%2020240401114039.png)




