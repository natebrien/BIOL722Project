
## What I'd like a bioinformatic USER treatment to do

Ideally, a bioinformatic USER treatment would recapitulate the enzymatic action of the USER enzyme. 
## What this bioinformatic USER treatment actually did

Examined misincorporation files output from mapDamage to obtain frequencies of C -> T and G -> A conversions: 

```
awk -F' ' '$2<0.02{print$1}' 5pCtoT_freq.txt | head -1
```
```
awk -F' ' '$2<0.02{print$1}' 3pGtoA_freq.txt | head -1
```

Used the output of these to trim off bases from the sequences using Seqtk:

```
seqtk trimfq -b 9 -e 9 sim_seqs_s1_trimmed.fastq.gz > sim_seqs_s1_trim_USER
```
Repeat for second file. 

Once the sequences have been trimmed we need to align them to the reference genome as before: 
```
bwa aln GRCH38.p14.genome.fa -n 0.01 -o 2 -l 16500 -t 10 /2/scratch/natassja/Bio722/term_project/gargammel/bioinformatic_USER/sim_seqs_s1_trim_USER.fq.gz > sim_seqs_s1_trim_USER_aln.sai
```
Repeat for second file. 

Run bwa sampe: 
```
bwa sampe GRCh38.p14.genome.fa sim_seqs_s1_trim_USER_aln.sai sim_seqs_s2_trim_USER_aln.sai sim_seqs_s1_trim_USER sim_seqs_s2_trim_USER > sim_seqs_trim_USER_aln.sam
 ```

Then, convert the SAM file to a BAM file:
```
samtools view -b sim_seqs_trim_USER_aln.sam > sim_seqs_trim_USER_aln.bam
```

Sort the BAM file (required later):
```
samtools sort sim_seqs_trim_USER_aln.bam > sim_seqs_trim_USER_aln_sorted.bam
```

Run mapDamage to generate a plot: 
```
mapDamage -i sim_seqs_trim_USER_aln.bam -r GRCh38.p14.genome.fa
```

The plot should show significantly reduced damage patterns (absence of 'smile'). 
