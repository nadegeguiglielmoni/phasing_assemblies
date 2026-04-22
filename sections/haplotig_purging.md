## Haplotig purging

```sh
# for Nanopore reads
minimap2 -x map-ont assembly.fasta ont_reads.trimmed.fastq.gz | gzip -c - > minimap2.assembly.paf.gz

# for HiFi reads
minimap2 -x map-hifi assembly.fasta hifi_reads.fastq.gz | gzip -c - > minimap2.assembly.paf.gz
        
pbcstat minimap2.assembly.paf.gz 
calcuts PB.stat > cutoffs 2>calcults.log

hist_plot.py -c cutoffs PB.stat purge_dups.${pri_asm}.png

split_fa assembly.fasta > assembly.fasta.split
minimap2 -xasm5 -DP assembly.fasta.split assembly.fasta.split | gzip -c - > assembly.fasta.split.self.paf.gz

purge_dups -2 -T cutoffs -c PB.base.cov assembly.fasta.split.self.paf.gz > dups.bed 2> purge_dups.log
get_seqs dups.bed assembly.fasta
```
