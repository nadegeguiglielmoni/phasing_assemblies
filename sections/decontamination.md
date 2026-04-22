## Decontamination

[BLAST](https://www.ncbi.nlm.nih.gov/geo/query/blast.html) v2.10.0
```sh
blastn -query assembly.fasta -db nt -outfmt "6 qseqid staxids bitscore std sscinames scomnames" \
               -max_hsps 1 -evalue 1e-25 -out assembly.fasta.blast.out
```

[minimap2](https://github.com/lh3/minimap2) v2.24
```sh
# for Nanopore reads
minimap2 -ax map-ont assembly.fasta ont_reads.trimmed.fastq.gz | samtools sort > minimap2.assembly.bam

# for HiFi reads
minimap2 -ax map-hifi assembly.fasta hifi_reads.fastq.gz | samtools sort > minimap2.assembly.bam
```

[BUSCO](https://gitlab.com/ezlab/busco) v5.8.0
```sh
docker run -u $(id -u) -v $(pwd):/busco_wd ezlabgva/busco:v5.8.0_cv1 busco -i assembly.fasta -o busco_nematoda_odb12_assembly -m genome -l nematoda_odb12
```

[BlobToolKit](https://blobtoolkit.genomehubs.org/) v4.3.2
```sh
blobtools add --fasta assembly.fasta --cov minimap2.assembly.bam --hits assembly.fasta.blast.out --busco busco_nematoda_odb12_assembly/run_nematoda_odb12/full_table.tsv --taxdump taxdump --cr
eate blobdir_out
blobtools view blobdir_out
```
