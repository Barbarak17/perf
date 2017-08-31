# PERF
## Introduction
PERF is a Python package developed for fast and accurate identification of microsatellites from DNA sequences. Microsatellites or Simple Sequence Repeats (SSRs) are short tandem repeats of 1-6nt motifs. They are present in all genomes, and have a wide range of uses and functional roles. The existing tools for SSR identification have one or more caveats in terms of speed, comprehensiveness, accuracy, ease-of-use, flexibility and memory usage. PERF was designed to address all these problems.

PERF is a recursive acronym that stands for "PERF is an Exhaustive Repeat Finder". It is compatible with both Python 2 (tested on Python 2.7) and 3 (tested on Python 3.5). Its key features are:
  - Fast run time, despite being a single-threaded application. As an example, identification of all SSRs from the entire human genome takes less than 7 minutes. The speed can be further improved ~3- to 4-fold using [PyPy](https://pypy.org/) (human genome finishes in less than 2 minutes using PyPy v5.8.0)
  - Linear time and space complexity (O(n))
  - Identifies perfect SSRs
  - 100% accurate and comprehensive - Does not miss any repeats or does not pick any incorrect ones
  - Easy to use - The only required argument is the input DNA sequence in FASTA format
  - Flexible - Most of the parameters are customizable by the user at runtime
  - Repeat cutoffs can be specified either in terms of the total repeat length or in terms of number of repeating units
  - TSV output and HTML report. The default output is an easily parseable and exportable tab-separated format. Optionally, PERF also generates an interactive HTML report that depicts trends in repeat data as concise charts and tables

## Installation
PERF can be directly installed using pip with the package name `perf_ssr`. 
```bash
$ pip install perf_ssr
```

This name was chosen for the package so as not to clash with the existing `perf` package.

Alternatively, it can also be installed from the source code:
```bash
# Download the git repo
$ git clone https://github.com/RKMlab/perf.git

# Install
$ cd perf
$ python setup.py install
```
Both of the methods add a console command `PERF`, which can be executed from any directory. It can also be used without installation by running the `core.py` file in the `PERF` subfolder:

```bash
$ git clone https://github.com/RKMlab/perf.git
$ cd perf/PERF
$ python core.py -h # Print the help message of PERF (see below)
```

## Usage
The help message and available options can be accessed using
```bash
$ PERF -h # Short option
$ PERF --help # Long option
```
which gives the following output
```
usage: PERF [-h] -i <FILE> [-o <FILE>] [-a] [-l <INT> | -u INT or FILE]
            [-rep <FILE>] [-m <INT>] [-M <INT>] [--version]

Required arguments:
  -i <FILE>, --input <FILE>
                        Input file in FASTA format

Optional arguments:
  -o <FILE>, --output <FILE>
                        Output file name. Default: Input file name + _perf.tsv
  -a, --analyse         Generate a summary HTML report.
  -l <INT>, --min-length <INT>
                        Minimum length cutoff of repeat
  -u INT or FILE, --min-units INT or FILE
                        Minimum number of repeating units to be considered.
                        Can be an integer or a file specifying cutoffs for
                        different motif sizes.
  -rep <FILE>, --repeats <FILE>
                        File with list of repeats (Not allowed with -m and/or
                        -M)
  -m <INT>, --min-motif-size <INT>
                        Minimum size of a repeat motif in bp (Not allowed with
                        -rep)
  -M <INT>, --max-motif-size <INT>
                        Maximum size of a repeat motif in bp (Not allowed with
                        -rep)
  --version             show program's version number and exit
```
The details of each option are given below:

### `-i or --input`
**Expects:** *FILENAME*<br>
**Default:** *None*<br>
This is the only required argument for the program. The input file must be a valid FASTA file. PERF uses [Biopython's](http://biopython.org/wiki/SeqIO) FASTA parser to read the input files. It accepts both single-line and multi-line sequences. Files with multiple sequences are also valid. To see more details about the FASTA format, see [this page](http://bioperl.org/formats/sequence_formats/FASTA_sequence_format).

### `-o or --output`
**Expects:** *FILENAME*<br>
**Default:** *Input Filname + _perf.tsv (see below)*<br>
The output is a tab-delimited file, with one SSR record per line. If this option is not provided, the default output filename will the same as the input filename, with its extension replaced with '_perf.tsv'. For example, if the input filename is `mySeq.fa`, the default output filename will be `mySeq_perf.tsv`. If the input filename does not have any extension, `_perf.tsv` will be appended to the filename. Please note that even in the case of no identified SSRs, the output file is still created (therefore overwriting any previous file of the same name) but with no content in the file.

The output columns follow the [BED](https://genome.ucsc.edu/FAQ/FAQformat.html) format. The details of the columns are given below:

| S.No | Column | Description |
|:----:| ------ | ----------- |
| 1 | Chromosome | |
| 2 | Repeat Start | |
| 3 | Repeat Stop | |
| 4 | Repeat Class | |
| 5 | Repeat Length | |
| 6 | Repeat Strand | |
| 7 | Motif Number | |
| 8 | Actual Repeat | |

An example output showing some of the largest repeats from *Drosophila melanogaster* is given below
```
X       22012826  22014795  ACTGGG  1969    -       328     TCCCAG
2RHet   591337    591966    AATACT  629     -       104     ATTAGT
4       1042143   1042690   AAATAT  547     +       91      AAATAT
2RHet   598244    598789    AATACT  545     -       90      AGTATT
XHet    122       663       AGAT    541     +       135     GATA
X       22422335  22422827  AGAT    492     +       123     GATA
3R      975265    975710    AAAT    445     -       111     TTAT
X       15442288  15442724  ACAGAT  436     +       72      ACAGAT
2L      22086818  22087152  AATACT  334     -       55      TATTAG
YHet    137144    137466    AAGAC   322     -       64      CTTGT
```

### `-a or --analyze`
**Expects:** None<br>
**Default:** False<br>
In addition to the default tab-separated output, PERF can also generate a fully interactive HTML report for easy downstream analysis of the repeat data. An example HTML report can be accessed [here](https://raw.githubusercontent.com/RKMlab/perf/html-report/test_data/test_input_perf.html) (Right click -> Save As).

### `-l or --min-length`
**Expects:** *INTEGER*<br>
**Default:** *12*<br>

## Contact
For queries or suggestions, please contact:

Divya Tej Sowpati - <tej@ccmb.res.in><br>
Akshay Kumar Avvaru - <avvaru@ccmb.res.in>

