
## What I'd like a bioinformatic USER treatment to do

Ideally, a bioinformatic USER treatment would recapitulate the enzymatic action of the USER enzyme. 

## What this bioinformatic USER treatment actually did

Examined misincorporation files output from mapDamage to obtain frequencies of C -> T and G -> A conversions: 

`awk -F' ' '$2<0.01{print$1}' 5pCtoT_freq.txt | head -1`
`awk -F' ' '$2<0.01{print$1}' 3pGtoA_freq.txt | head -1`

Used the output of these to trim off bases from the sequences using fqtools: 

`conda activate fqtools-env`
`fqtools trim -o sim_data_s1_USER -s 7 sim_data_s1_trimmed.fastq.gz`

`fqtools trim -o sim_data_s2_USER -s 7 sim_data_s2_trimmed.fastq.gz`

Once the sequences have been trimmed we need to align them to the reference genome as before: 
`bwa aln ...`

Then, convert the SAM file to a BAM file:
`samtools view -b in.sam > out.bam`

Run mapDamage to generate a plot: 
`mapDamage -i BAM -r REFERENCE`


