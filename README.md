# PrimerServer2
PrimerServer2: a high-throughput primer design and specificity-checking platform

![screenshot](https://raw.githubusercontent.com/billzt/figure/master/PrimerServer-UI-typeA.png) 

## Description

PrimerServer was proposed to design genome-wide specific PCR primers. It uses candidate primers produced by Primer3, uses BLAST and nucleotide thermodynamics to search for possible amplicons and filters out specific primers for each site. By using multiple threads, it runs very fast, ~0.4s per site in our case study for more than 10000 sites.

This repository is based on Python3 and acts as the successor of legacy [PrimerServer](https://github.com/billzt/PrimerServer).

## External Dependencies

**Add these two softwares to your system PATH**

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

## Running **PrimerServer2**

The key command in for use in **PrimerServer2** is `primertool`, which takes two parameters, a `query` file containing targets to design primers for and a `template` file with the FASTA sequences containing those targets. The `query` file can take two formats...

### Targets defined directly in FASTA Format

If you have parts of template sequences, you can indicate a target directly in FASTA format by surrounding it with square braces (`[ ]`)...

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
This means primers that primers should be designed outside of this target to amplify the section in braces. This is the default mode.

Alternatively, if you have genomic coordinates for each site (e.g. SNPs), you can input target coordinates using the name of the FASTA sequence, the start position of the target within that sequence, and the length of the target, separated by tabs or other white space. For example...

```
seq1  200   10
seq1  400   15
```

This means that each target site is specified on a separate line. In this example, the first target site is located in `seq1`, starts at position `200`, and the size of the desired region to amplify is `10` base pairs (this means that the target for which primers are to be designed is `seq1:200-209`). The second site is in `seq1`, starts at position `400`, and and the size of the desired region to amplify is `15` base pairs (means `seq1:400-414`).

To set a microsatellite as a target, you should enter the start position as start of the microsatellite region within the FASTA file **minus** some desired size of a "buffer" before the microsatellite into which the primer sequence should not extend, e.g., `10` bp. The length of the target region, enter the size (in bp) of the microsatellite region **plus** 2 times the length of the desired buffer, e.g., plus 2 x 10 bp = plus `20`. For example, the following could be used as a `query` file for defining two target microsatellites, one located on `seq1` starting at bp 200 and extending for 30 bases with a buffer of 10 bp on either side of the microsatellite (i.e., the target minus buffer start is bp 200 - 10 = 190 and the length is 30 + 2 x 10 = 50).

```
seq1  190   50
seq2  390   50
```

For details, see the [wiki page](https://github.com/billzt/PrimerServer2/wiki) for the original PrimerServer2.

## Need to run the Web UI?

Please refer to the [wiki](https://github.com/billzt/PrimerServer2/wiki).

## Warning: About the reference genome
If you use reference genomes with many unplaced scaffolds, be caution since such scaffolds with great homology with main chromosomes might influence your results.
If possible, delete (some or all of ) these unplaced scaffolds.
For the human genome, we recommend the [no_alt_analysis_set](https://hgdownload.soe.ucsc.edu/goldenPath/hg19/bigZips/analysisSet/), which has all the PAR regions marked with N, to be used.

## Comparison of the CLI and Web version
|                       | CLI              | Web UI             |
| ----                  | ----             | ----               |
| Design primers        |:heavy_check_mark:| :heavy_check_mark: |
| Checking specificity  |:heavy_check_mark:| :heavy_check_mark: |
| Progress monitor      |:heavy_check_mark:| :heavy_check_mark: |
| Number of tasks       |:High            :| :Low:              |
| Alternative isoforms  |:heavy_check_mark:| :x:                |
| Exon-exon junction    |:heavy_check_mark:| :x:                |
| Pick internal oligos  |:heavy_check_mark:| :x:                |
| Custom Tm temperature |:heavy_check_mark:| :x:                |
| Custom max amplicons  |:heavy_check_mark:| :x:                |
| Visualization         |:x               :| :heavy_check_mark: |
