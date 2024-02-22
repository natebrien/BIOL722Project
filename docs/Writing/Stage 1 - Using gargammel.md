**Background on gargammel**

**Prep for running gargammel**
Infoserv has gargammel installed through conda, therefore the necessary conda environment needs to be activated before running gargammel or it will throw an error. Activate the gargammel conda env by running: 

	$ conda activate /home/sam/envs/gargammel

If this works correctly you should see (gargammel) before [yourname@info2020]. 

**Parameters for gargammel**
We want gargammel to create simulated ancient DNA fragments with methylation and deamination damage. To do this we need to run gargammel using the following flags (with accompanying explanations).

	- o
	- n 1000000
	- f (file.txt)
	- maxsize 100
	-- methyl
	- damage [0.024, 0.36, 0.0097, 0.68]

These flags specify (in order): the output directory, the number of fragments to simulate, the frequency distribution of fragment lengths, the maximum size of fragments, to add methylation-specific damage, and the damage parameters. 

**Quality control checks**
Quality control at this stage means checking that the fragmentation and deamination patterns of the simulated sequences match what we would expect from ancient DNA. 



