## Background on gargammel.
Gargammel will be used to simulate aDNA sequence data for this project. Although gargammel has the capability to simulate both modern human and bacterial contamination in the sample, only the endogenous human sequences will be simulated. The resulting simulated sequences have fragmentation patterns and deamination patterns characteristic of aDNA, and have Illumina adapters added to simulate raw sequencing reads. Gargammel outputs gzipped fastq files. Gargammel also does not simulate the effects of bisulfite treatment.
### Simulate sequences with gargammel

First activate conda environment:
`$ conda activate /home/sam/miniconda3/envs/gargammel`

Create the folders necessary for gargammel: 
`mkdir data`
`cd data`
`mkdir endo`
`mkdir bact`
`mkdir cont`'

Download the reference genome into endo:
`cd endo`
`wget https://ftp.ncbi.nlm.nih.gov/genomes/all/GCA/000/001/405/GCA_000001405.29_GRCh38.p14/GCA_000001405.29_GRCh38.p14_genomic.fna.gz`
`mv GCA_000001405.29_GRCh38.p14_genomic.fna.gz GRCh38.p14.genome.fa.gz`
`gunzip GRCh.p14.genome.fa.gz`
`samtools faidx GRCh38.p14.genome.fa`

We want gargammel to create simulated ancient DNA fragments with methylation and deamination damage. To do this we need to run gargammel using the following flags:

`gargammel -o results/sim_seqs -f FLD.tab -n 1000000 -damage 0.024,0.36,0.0097,0.68 --methyl -matfilemeth /home/sam/gargammel/src/matrices/double- -matfilenonmeth /home/sam/gargammel/src/matrices/double- data`
## Quality control checks
Quality control at this stage means checking that the fragmentation and deamination patterns of the simulated sequences match what we would expect from ancient DNA. 
#### FastQC
Run fastqc on simulated data files sim_data_s1.fq.gz and sim_data_s2.fq.gz:
`fastqc results/sim_data_s1.fq.gz results/sim_data_s2.fq.gz --outdir quality_control/`
#### Fastp
Fastp is used to trim, merge, and quality filter reads. Reads need to be trimmed and filtered before they are suitable for input to bwa. 
`fastp -i /2/scratch/natassja/Bio722/term_project/gargammel/results/sim_seqs_s1.fq.gz -I /2/scratch/natassja/Bio722/term_project/gargammel/results/sim_seqs_s2.fq.gz -o sim_seqs_s1_trimmed -O sim_seqs_s2_trimmed`
### Alignment
#### bwa-aln
Run bwa-aln using ancient settings to align to an indexed human reference genome:

First index reference genome:
`bwa index GRCh38.p14.genome.fa`
(this had been done prior to this project)

Align: 
`bwa aln GRCH38.p14.genome.fa -n 0.01 -o 2 -l 16500 -t 10 /2/scratch/natassja/Bio722/term_project/gargammel/quality_control/sim_seqs_s1_trimmed.fq.gz > sim_seqs_s1_trim_aln.sai
Repeat for second file. 

Then run the following to generate a SAM file from the two SAI files:
`bwa sampe GRCh38.p14.genome.fa sim_seqs_s1_trim_aln.sai sim_seqs_s2_trim_aln.sai sim_seqs_s1_trimmed sim_seqs_s2_trimmed > sim_seqs_s1s2_trim_aln.sam `

Cover the SAM file to a BAM file
`samtools view -b sim_seqs_s1s2_trim_aln_sam > sim_seqs_s1s2_trim_aln.bam`

Sort the BAM file:
`samtools sort sim_seqs_s1s2_trim_aln.bam > sim_seqs_s1s2_trim_aln_sorted.bam`
#### mapDamage

Run mapDamage: 
(if you are not on info115 `logout` then `ssh info115`)

`mapDamage -i sim_seqs_trim_aln.sam -r GRCh38.p14.genome.fa`





