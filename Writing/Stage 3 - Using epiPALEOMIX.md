EpiPALEOMIX is a program for creating methylation maps, developed by Kristian Hanghoj. 

# Install epiPALEOMIX

EpiPALEOMIX requires the following: 
- python 2.7.3+
- pysam 0.8+
- R v2.15
## Conda environment setup

To make installation a little easier, I first created a conda environment: 

`conda create -n epipaleomix-env`

Then, installed the correct versions of python and pysam from the channel conda-forge:

`conda install -n epipaleomix-env -c conda-forge python=2.7`
`conda install -n epipaleomix-env -c conda-forge pysam=0.8`

However, the version of R that is required is not available from conda channels, so wget was used to retrieve R v2.15 from CRAN: 

`cd /home/natassja/miniconda3/envs/epipaleomix-env/bin`
`wget https://cran.r-project.org/src/base/R-2/R-2.15.3.tar.gz`
`gunzip R-2.15.3`
`tar -xvf R-2.15.3`
`cd R-2.15.3`
`./configure`
`make`

Finally, run `conda activate epipaleomix-env`. 
## Generating the makefile
EpiPALEOMIX requires three inputs: a BAM alignment file (indexed), a BED file containing coordinates for the regions of interest, and a reference genome in FASTA format (also indexed). Paths to these files and names must be provided in a makefile. EpiPALEOMIX can generate a verbose and a simple makefile. 

To create a template makefile, run: 

`$ epiPALEOMIX simple makefile > simulated_seqs.yaml`

This template indicates where the paths to the input files must be specified. 
The input file paths were specified as follows: 

Reference FASTA file: 
BAM file with sequences aligned: 
BED file with regions of interest: /home/natassja/sequences/cpgs_all.bed



