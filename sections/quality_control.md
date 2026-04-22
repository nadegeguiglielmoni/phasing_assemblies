# Quality control

This section aims to conduct a first evaluation of the genome and the sequencing data before assembling the reads. 

## *k*-mer estimation of genomic features

From a high-accuracy sequencing dataset, we can predict general features of the target genome (size, heterozygosity, repetitive content...). In this study, we used already published short Illumina reads for the nematode *Plectus sambesii* available at [SRR7508118](https://www.ebi.ac.uk/ena/browser/view/SRR7508118). 

*k*-mers can be extracted from this high-accuracy dataset. *k*-mers are subsequences of *k* nucleotides. The first step is to identify these *k*-mers, which we do here with the tool Jellyfish, with a set value of *k*. We chose *k*=21 (thus 21-mers), a commonly used value for eukaryotes, but certain *k*-mer counters use values by default of 27, 31... 

The module `jellyfish count` identifies the *k*-mers and their representation in the genome, and the module `jellyfish histo` creates the histogram. The parameter `-m 21` sets the *k* length.

[Jellyfish](https://github.com/gmarcais/Jellyfish) v2.2.8

```sh
jellyfish count -C -m 21 -s 50000000000 illumina.fastq -o illumina.jf
jellyfish histo illumina.jf > illumina.histo
```

From the histogram, the tool GenomeScope2 is used to make predictions on the genomic features. The parameter `-k 21` sets the *k* length.

[GenomeScope2](https://github.com/tbenavi1/genomescope2.0) v2.0
```sh
genomescope.R -i illumina.histo -o genomescope_k21 -k 21
```

![linearplot](https://github.com/nadegeguiglielmoni/phasing_assemblies/fig/linear_plot.png)

From the output, we look at the linear plot. At a low coverage, we can see that there are many *k*-mers. These are erroneous *k*-mers, which are isolated sequences occuring due to sequencing errors. Then there are two peaks, a first peak with very high frequency (many different *k*-mers) at about 100X, and a second peak at about twice the coverage of the first peak, around 200X. These two peak suggest a diploid genome, with the first peak for heterozygous *k*-mers present on only one of the two haplotypes, and the second peak for homozygous *k*-mers on both haplotypes.
The genome size is predicted as 120,912,418 bp and the unique content at 79.5%, equivalent to 20.5% repetitive content. 

## Assessment of length and quality of long reads

[NanoPlot](https://github.com/wdecoster/NanoPlot) v1.46.2
```sh
NanoPlot --fastq ont_reads.fastq.gz -c blue --tsv_stats -o nanoplot_ont
NanoPlot --fastq hifi_reads.fastq.gz -c red --tsv_stats -o nanoplot_hifi
```
