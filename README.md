# PrimerServer2
PrimerServer2: a high-throughput primer design and specificity-checking platform

![screenshot](https://raw.githubusercontent.com/billzt/figure/master/PrimerServer-UI-typeA.png) 

## Description

PrimerServer was proposed to design genome-wide specific PCR primers. It uses candidate primers produced by Primer3, uses BLAST and nucleotide thermodynamics to search for possible amplicons and filters out specific primers for each site. By using multiple threads, it runs very fast, ~0.4s per site in our case study for more than 10000 sites.

This repository is based on Python3 and acts as the successor of legacy [PrimerServer](https://github.com/billzt/PrimerServer).

## External Dependencies

** Add these two softwares to your system PATH**
* [Samtools](https://www.htslib.org/) (>=1.9).
* [NCBI BLAST+](https://blast.ncbi.nlm.nih.gov/Blast.cgi) (>=2.2.18)

## Install **PrimerServer2**

Do not use Python 3.8 or above since the [primer3-py](https://pypi.org/project/primer3-py/) module does not seem to be supported for newer versions of **Python** yet

1. Create a new **conda** environment for the software...

```bash
$ conda create -n primer python=3.7
$ conda activate primer
```

2. Clone this repository from **GitHub**...

```bash
$ git clone https://github.com/difiore/PrimerServer2.git
$ cd PrimerServer2
```

2. Downgrade **setuptools** from that distributed with the **conda** distribution of **Python 3.7** (65.6.3) to 58.2.0 (needed to run the `setup.py` script)...

```bash
$ pip3 install setuptools==58.2.0
```
3. Run the `setup.py` install script...

```bash
$ python3 setup.py install
```

> NOTE: The release version of **primerserver2** can be installed into your (primer) **conda** environment using `pip3 install primerserver2`. However, this will install the 2.0.0b20 release of the software from https://github.com/billzt/PrimerServer2 not the modified version from https://github.com/difiore/PrimerServer2.git. Installing one version and then another will not override the first installation. To remove an installation, use `pip3 uninstall primerserver2` before running the alternative installation.

## Running test commands

> NOTE: To run the following, you will need to have downloaded the `tests` directory located within the **GitHub** repository.

```bash
** full mode: design primers and check specificity
$ primertool full tests/query_design_multiple tests/example.fa

** design mode: design primers only
$ primertool design tests/query_design_multiple tests/example.fa

** check mode: check specificity only
$ primertool check tests/query_check_multiple tests/example.fa
```

## Running **PrimerServer2

The key command in for use in **PrimerServer2** is `primertool`, which takes two parameters, a `query` file and a `template` file. The `query` file 


## Input Format (The First Parameter)
### in FASTA Format
If you have parts of template sequences, you can directly input in FASTA format:
```
>site1
TGTGATATTAAGTAAAGGAACATTAAACAATCTCGACACCAGATTGAATATCGATACAGA
TACCCCAACTGCCGCCAATTCAACCGACCCTTCACCACAAAAAAACTAATATTTATCAGC
CAATA[GTTACCTGTGTG]ATTAATAGATAAAGCTACAAAAGCAAGCTTGGTATGATAGT
TAATAATAAAAAAAGAAAAAACAAGTATCCAAATGGCCAACAAAGGCTGTATCAACAAGT
>site2
ACCAGATTGAATATCGATACAGATACCCCAACTGCCGCCAATTCAACCGACCCTTCACCA
CAAAAAAACTAATATTTATCA[GC]CAATAGTTACCTGTGTGATTAATAGATAAAGCTAC
AAGCAAGCTTGGTATGATAGTATTAATAATAAAAAAAGAAAAACAAGTATCCAAATGGCC
```
Note there is a pair of square brackets `[]` indicating target in each sequences. It means primers should be put around the target. This is the default mode.

### in Text Format
If you have genomic coordinates for each site (e.g. SNPs), you can input coordinates like:
```
seq1 200 10
seq1 400 10
```
It means that two sites (one site per line) are needed to design primers. The first site is in `seq1` and starts in position `200` and the region length is `10` (means `seq1:200-209`). The second site is in `seq1` and starts in position `400` and the region length is `10` (means `seq1:400-409`).

AD: for a microsatellite, enter start position as start of msat in FASTA -10 and region length as length of msat + 20

For details, see the [wiki](https://github.com/billzt/PrimerServer2/wiki).

## Need to run the Web UI?
Please refer to the [wiki](https://github.com/billzt/PrimerServer2/wiki).

## Warning: About the reference genome
If you use reference genomes with many unplaced scaffolds, be caution since such scaffolds with great homology with main chromosomes might influence your results.
If possible, delete (some or all of ) these unplaced scaffolds.
For the human genome, we recommend the [no_alt_analysis_set](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/analysisSet/), which has all the PAR regions marked with N, to be used.

## Comparison of the CLI and Web version
|        |    CLI    |    Web UI    |
|    ----    |    ----    |    ----    |
|    Design primers    |     :heavy_check_mark:    |     :heavy_check_mark:    |
|    Checking specificity    |     :heavy_check_mark:    |     :heavy_check_mark:    |
|    Progress monitor    |     :heavy_check_mark:    |     :heavy_check_mark:    |
|    Number of tasks    |    High    |    Low    |
|    Alternative isoforms    |     :heavy_check_mark:    |     :x:    |
|    Exon-exon junction    |     :heavy_check_mark:    |     :x:    |
|    Pick internal oligos    |     :heavy_check_mark:    |     :x:    |
|    Custom Tm temperature    |     :heavy_check_mark:    |     :x:    |
|    Custom max amplicons    |     :heavy_check_mark:    |     :x:    |
|    Visualization    |     :x:    |     :heavy_check_mark:    |




