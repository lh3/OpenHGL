## Introduction

The Open Human Genome Library (OpenHGL) is a collection of high-quality *de
novo* human assemblies that are publicly available in genomic databases (e.g.
[NCBI][ncbi] and [CNCB][cncb]) or from individual research papers. It provides
consistent naming and uniform formats across datasets, supporting efficient
subsequence retrieval and approximate string search.

The dataset currently consists of 579 huamn genomes with 1.7 trillion
basepairs. The primary data is archived [at Zenodo][zenodo] and also hosted at
the [AWS Open Data Registry][aws-open] along with derived data.

## Usage

At present, OpenHGL provides genome sequences in the [AGC][agc] format and
the corresponding FM-index in the [ropebwt3][rb3] format.

### Retrieving genome sequences

It is recommended to download precompiled AGC binary from its [release
page][agc-rel]. After copying the `agc` binary to your `PATH`, you can download
and retrieve sequences with
```sh
# download AGC archive
wget https://zenodo.org/records/17772289/files/human579.agc

# list assembly names
agc listset human579.agc

# list contig names in assembly 200125_HG02129.pat
agc listctg human579.agc 200125_HG02129.pat

# retrieve all sequences in assembly 200125_HG02129.pat
agc getctg human579.agc 200125_HG02129.pat > HG02129.pat.fa

# retrieve the first 100bp of contig HG02129#1#CM085853.1
agc getctg human579.agc HG02129#1#CM085853.1:0-99
```
**Importantly**, with AGC, the coordinate of the first base is 0. *start*-*end*
is a closed interval. This is different from common tools like samtools faidx
which use closed intervals but put the first base at coodinate 1.

### Finding sequence matches

[Ropebwt3][rb3] is required to string search:
```sh
# install ropebwt3
git clone https://github.com/lh3/ropebwt3
cd ropebwt3; make               # add "omp=0" if you see errors

# download FM-index
wget https://zenodo.org/records/17772289/files/human579.fmr.gz
wget https://zenodo.org/records/17772289/files/human579.fmd.ssa.gz
wget https://zenodo.org/records/17772289/files/human579.fmd.len.gz

# prepare the FM-index
gzip -d human579.fmr.gz human579.fmd.ssa.gz       # keep *.len.gz
ropebwt3 build -i human579.fmr -do human579.fmd   # convert to the faster static format

# exact match
echo CCAGGACCCCTGTCCAGTGTTAGACAGGAGCATGCAG | ropebwt3 mem -L human579.fmd -

# inexact match
echo CCAGGACCCCTGTCCAGTGTTAGACAGGAGCATGCAG | ropebwt3 sw -eN200 -Lm10 human579.fmd -
```

[ncbi]: https://www.ncbi.nlm.nih.gov/
[cncb]: https://www.cncb.ac.cn/?lang=en
[zenodo]: https://zenodo.org/records/13948741
[aws-open]: https://registry.opendata.aws/
[agc]: https://github.com/refresh-bio/agc
[rb3]: https://github.com/lh3/ropebwt3
[agc-rel]: https://github.com/refresh-bio/agc/releases
