Configuration
=============

To get started, modify the example file below:

* :download:`example configuration file <data/example_config.yml>`.

See sections below for explainations of each field.

Basic options
-------------

input
^^^^^

The path to a file specifying the input data sets. See :doc:`input` for instructions for creating the input file.

Example: "data/input.yml".

output_dir
^^^^^^^^^^

Directory containing the results.

Example: "output/".

picard
^^^^^^

The path to the picard tools.

Example: "/home/kai/software/picard-tools-1.140/picard.jar".


genome
^^^^^^

Complete genome in a SINGLE plain FASTA file. Genome can be downloaded from
Gencode (human and mouse), UCSC or ENSEMBL.
Example link for human:
http://hgdownload.cse.ucsc.edu/goldenPath/hg38/bigZips/hg38.fa.gz.
*Important*: Remember to ungzip the file using "gzip -d hg38.fa.gz"!
Tip: To make a single genome file from multiple fastq files, execute:
`cat chr1.fa chr2.fa chr3.fa > genome.fa`

Example: "/home/kai/genome/GRCh38/genome.fa".

annotation
^^^^^^^^^^

Genome annotation in *GTF* format. For human and mouse, Gencode annotations
are available at http://www.gencodegenes.org/.
*Very important*: chromosome names in the annotations GTF file have to match
chromosome names in the FASTA genome sequence file. For example, one can use
ENSEMBL FASTA files with ENSEMBL GTF files, and UCSC FASTA files with UCSC
FASTA files. However, since UCSC uses chr1, chr2,... naming convention,
and ENSEMBL uses 1, 2, ... naming, the ENSEMBL and UCSC FASTA and GTF files
cannot be mixed together.

Example: "/home/kai/genome/GRCh38/gencode.v25.annotation.gtf".

motif_file
^^^^^^^^^^

MEME format file containing motifs.
The naming convention for motifs is `TF_NAME+OTHER_STRING", where
the `TF_NAME` should match the gene names in your annotation file.
See these files for examples: :download:`Human <data/motifs/cisBP_human.meme>`
and :download:`mouse <data/motifs/cisBP_mouse.meme>` motif files.

Example: "/home/kai/motif_databases/cisBP_human.meme".


Genome Indices
--------------

.. note:
    You don't have to physically provide the following files. But you do need to
    specify the locations where these files will be *GENERATED AUTOMATICALLY WHEN
    FILES/DIRECTORIES DOES NOT EXIST*. If the specified directories or files
    already exist, the program will do nothing.
    If this is the first time you run the program, make sure delete existing
    files/directories first so indices can be generated properly.
    You only need to generate the indices once, *THEY CAN BE REUSED*.

seq_index
^^^^^^^^^

This is the *FILE* containing GENOME SEQUENCE INDEX.

Example: "/home/kai/genome/GRCh38/GRCh38.index".

bwa_index
^^^^^^^^^

This is the *DIRECTORY* containing BWA INDICES.

Example: "/home/kai/genome/GRCh38/BWAIndex/".

star_index
^^^^^^^^^^

This is the *DIRECTORY* containing STAR INDICES.

Example: "/home/kai/genome/GRCh38/STAR_index/".

rsem_index
^^^^^^^^^^

This is the *DIRECTORY* containing RSEM INDICES.

Example: "/home/kai/genome/GRCh38/RSEM_index/".
