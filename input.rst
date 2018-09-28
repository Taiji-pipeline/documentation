Input data format
=================

The input data file can be written in either the `YAML <https://en.wikipedia.org/wiki/YAML>`_ or the `TSV <https://en.wikipedia.org/wiki/Tab-separated_values>`_ format.

Examples:
---------

* :download:`example input file <data/example_input.yml>`.
* :download:`example input file <data/example_input.tsv>`.

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

HiC
---

+-------------------+-----------------+----------------+--------------+
| Supported formats | Description     | Required tags  | Optional tags|
+===================+=================+================+==============+
| Plain Text        | a list of Loops |"ChromosomeLoop"| None         |
+-------------------+-----------------+----------------+--------------+

Taiji can automatically download data from ENCODE portal and GEO database.
