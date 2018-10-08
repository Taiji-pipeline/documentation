Input data format
=================

The input data file can be written in either the `YAML <https://en.wikipedia.org/wiki/YAML>`_ or the `TSV <https://en.wikipedia.org/wiki/Tab-separated_values>`_ format.

Examples:
---------

* :download:`example input file (YAML) <data/example_input.yml>`.
* :download:`example input file (TSV) <data/example_input.tsv>`.

Supported input data types are listed below:

ATAC-seq
--------

+-------------------+---------------+---------------+---------------------------+
| Supported formats | Description   | Required tags | Optional tags             |
+===================+===============+===============+===========================+
| ``*.fastq`` or    | Raw reads     | None          | None                      |
| ``*.fastq.gz``    |               |               |                           |
+-------------------+---------------+---------------+---------------------------+
| ``*.bam``         | Aligned reads | None          | "filtered": do not further|
|                   |               |               | filter this file          |
+-------------------+---------------+---------------+---------------------------+
| ``*.bed`` or      | Aligned reads | None          |                           |
| ``*.bed.gz``      |               |               |                           |
+-------------------+---------------+---------------+---------------------------+

RNA-seq
-------

+-------------------+---------------+------------------------+--------------+
| Supported formats | Description   | Required tags          | Optional tags|
+===================+===============+========================+==============+
| ``*.fastq`` or    | Raw reads     | None                   | None         |
| ``*.fastq.gz``    |               |                        |              |
+-------------------+---------------+------------------------+--------------+
| Plain Text        | Expression    | "GeneQuant"            | None         |
|                   | profile       |                        |              |
+-------------------+---------------+------------------------+--------------+

When the input format is "plain text", Taiji assumes it contains two columns
separated by Tabs. The first column is the names of genes and the second column is
the expression levels. For example:

```
Gene1 <TAB> 12
Gene2 <TAB> 20
```

HiC
---

+-------------------+-----------------+----------------+--------------+
| Supported formats | Description     | Required tags  | Optional tags|
+===================+=================+================+==============+
| Plain Text        | a list of Loops |"ChromosomeLoop"| None         |
+-------------------+-----------------+----------------+--------------+

Currently the pipeline do not analyze HiC data, so the user need to
provide the end result - a list of loops, in following format:

```
chrom_1 <TAB> start_1 <TAB> end_1 <TAB> chrom_2 <TAB> start_2 <TAB> end_2
```

For example:

```
chr21 <TAB> 29343 <TAB> 500000 <TAB> chr21 <TAB> 1009340 <TAB> 1023400
```


Data in the internet
---------------------

Taiji can automatically download data from ENCODE portal and GEO database.
