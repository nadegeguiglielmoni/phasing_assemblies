# Assembly evaluation

[assembly-stats](https://github.com/sanger-pathogens/assembly-stats) v1.0.1
```sh
assembly-stats assembly.fasta
```

[BUSCO](https://gitlab.com/ezlab/busco) v5.8.0
```sh
docker run -u $(id -u) -v $(pwd):/busco_wd ezlabgva/busco:v5.8.0_cv1 busco -i assembly.fasta -o busco_nematoda_odb12_assembly -m genome -l nematoda_odb12
```

[KAT](https://github.com/TGAC/KAT) v2.4.2
```sh
kat comp -o kat_comp hifi_reads.fastq.gz assembly.fasta
```
