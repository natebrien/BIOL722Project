DamMet is a program for estimating methylation in ancient DNA sequence data that takes into account sequence errors, true variants, and deamination damage. 
## Install DamMet
DamMet is available on bioconda. 

`conda create -n dammet`
`conda activate dammet`
`conda install -n dammet -c bioconda dammet`
## Using DamMet - Inputs
DamMet requires three files as inputs: 
- BAM file with aligned sequences (use -b flag)
- Reference genome file (.fa formatted) (-r flag)
- Chromosome of interest (-c flag)

We will also add a bedfile (flag -B) with coordinates for our region of interest. 

Since DamMet processes by chromosome, create a file called chromosomes.txt that contains the name of each chromosome on a line, e.g., 
`chr1
`chr2
`chr3`

Then, run the following: 
`for chr in ``cat chromosomes.txt```; do DamMet estDeam -b sim_seqs_s1s2_trim_aln_sorted.bam -r reference -c $chr -B /home/natassja/sequences/cpgs_all.bed -O sites_$chr; done

