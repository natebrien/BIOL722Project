**Background on gargammel**.
Gargammel will be used to simulate aDNA sequence data for this project. Although gargammel has the capability to simulate both modern human and bacterial contamination in the sample, only the endogenous human sequences will be simulated. The resulting simulated sequences have fragmentation patterns and deamination patterns characteristic of aDNA, and have Illumina adapters added to simulate raw sequencing reads. Gargammel putputs gzipped fastq files. Gargammel also does not simulate the effects of bisulfite treatment.

Reference: 

**Prep for running gargammel**.
Infoserv has gargammel installed through conda, therefore the necessary conda environment needs to be activated before running gargammel or it will throw an error. Activate the gargammel conda env by running: 

	$ conda activate /home/sam/envs/gargammel

If this works correctly you should see (gargammel) before [yourname@info2020]. 

**Parameters for gargammel**.
We want gargammel to create simulated ancient DNA fragments with methylation and deamination damage. To do this we need to run gargammel using the following flags (with accompanying explanations).

	- o
	- n 1000
	- f (file.txt)
	- maxsize 100
	-- methyl
	- damage [0.024, 0.36, 0.0097, 0.68]

These flags specify (in order): the output directory, the number of fragments to simulate, the frequency distribution of fragment lengths, the maximum size of fragments, to add methylation-specific damage, and the damage parameters. 

I'm using n=1000 because a general shotgun sequencing run will generate 1M reads, but only a small portion of those reads will be endogenous, sometimes as small as 1%. I'm also using a maximum size of 100 based on fragment size distributions for ancient DNA. The deamination damage patterns are taken from the publication Briggs et al. 2007 which is recommended in gargammel documentation. 

**Quality control checks**.
Quality control at this stage means checking that the fragmentation and deamination patterns of the simulated sequences match what we would expect from ancient DNA. 



