# Install DamMet
`conda create -n dammet`
`conda activate dammet`
`conda install -n dammet -c bioconda dammet`

# Using DamMet - Inputs
DamMet requires three files as inputs: 
- BAM file with aligned sequences (use -b flag)
- Reference genome file (.fa formatted) (-r flag)
- Chromosome of interest (-c flag)
