## A collection of high-quality human assemblies

### Data source

| Name             | Version | nHap | Description |
|:-----------------|:--------|-----:|:------------|
|[CHM13][chm13]    |2.0      |1     |Analysis set with HG002 chrY and rCRS chrM|
|[CN1][cn1]        |1.0.1    |2     |Chinese Han|
|[KSA001][ksa001]  |1.1.0    |2     |Saudi Arabia|
|[I002C][i002c]    |0.7      |2     |Indian|
|[KOREF1][koref1]  |2025     |2     |Korean|
|[YAO][yao]        |2.0      |2     |Chinese|
|[HPRC][hprc]      |r2-v1.0.1|464   |Human Pangenome Reference Consortium|
|[APR][apr]        |v1       |104   |UAE-based Arab Pangenome Reference|

Criteria in sample selection:

 * Publicly available, though the data use policy may vary between sources
 * Requiring accurate long reads such as PacBio HiFi or the latest ONT R10.4.1
 * Requiring ultra-long Nanopore reads for assembly through difficult regions
 * Requiring trio or Hi-C data for chromosome-scale phasing
 * Independent samples

Additional procedure:

 * APR male samples were processed with the [yak][yak] X/Y partition pipeline
   such that Y is placed in hap1 and X in hap2. HPRC samples were processed the
   same way by the consortium.

### Naming convention

A sample name matches regular expression `([0-9]{6})_([A-Z0-9]+)\.(pri|pat|mat|hap1|hap2)`.
The leading digits are a unique identifier for the contig set. The alphanumeric
string after the first underscore indicates the sample name. If the assembly of
a sample is updated, the sample name stays the same but the identifier will be
different. The ending code specifies the assembly type:

 * pri: primary assembly (for CHM13 only)
 * pat: paternal assembly from trio phasing, with chrY
 * mat: maternal assembly from trio phasing, with chrX
 * hap1: haplotype 1 from Hi-C phasing, with chrY (partitioned with [yak][yak])
 * hap2: haplotype 2 from Hi-C phasing, with chrX (partitioned with [yak][yak])

A contig name matches `([^\s#]+)#[012]#([^\s#]+)` where the first field
corresponds to the sample name and the last field to the contig or chromosome
name.  The number in the middle indicates haplotype with 0 in primary assembly,
1 for paternal or haplotype 1, and 2 for maternal or haplotype 2.

### Known issues

 * HG002 from HPRC and CHM13 share the same Y chromosome
 * HG00272 has a ~50Mb inversion misassembly on the X chromosome
 * NA20806 has X and Y chromosomes mispartitioned to the same haplotype
 * HG02145 has fragmented Y chromosome (see HPRC [noteworthy samples][hprc-noteworthy])

[chm13]: https://github.com/marbl/CHM13
[cn1]: https://www.nature.com/articles/s41422-023-00849-5
[ksa001]: https://www.nature.com/articles/s41597-024-04121-2
[i002c]: www.biorxiv.org/content/10.1101/2025.07.12.664550
[koref1]: https://www.biorxiv.org/content/10.1101/2025.11.17.688257
[yao]: https://www.biorxiv.org/content/10.1101/2025.08.01.667781
[hprc]: https://github.com/human-pangenomics/hprc_intermediate_assembly
[apr]: www.nature.com/articles/s41467-025-61645-w
[hprc-noteworthy]: https://github.com/human-pangenomics/hprc_intermediate_assembly/blob/main/data_tables/sample/noteworthy_samples.md
[yak]: https://github.com/lh3/yak
