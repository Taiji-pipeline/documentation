Input data format
=================

The input data file can be written in either
the `YAML <https://en.wikipedia.org/wiki/YAML>`_ or
the `TSV <https://en.wikipedia.org/wiki/Tab-separated_values>`_ format.

YAML format is more readable and powerful, but it is sensitive to
indentation and beginners usually get confused.
For simple cases, the TSV format is easier to work with and more straightforward.

See these examples: 

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

Taiji will use MACS2 to call peaks. It is also possible to use your own peaks.
To do so, include the peak files in your input file, for example:

::

    ATAC-Seq:
        - group: 'ATAC_CD4_day1'
          id: 'CD4_day1'
          replicates:
          - rep: 1
            files:
            - path: CD4.narrowpeak
              format: NarrowPeak
            - path: CD4.bed.gz

OR in TSV format:

::

    type <TAB> id <TAB> group <TAB> rep <TAB> path <TAB> format
    ATAC-seq <TAB> ATAC_CD4_day1 <TAB> CD4_day1 <TAB> 1 <TAB> CD4.narrowpeak <TAB> NarrowPeak
    ATAC-seq <TAB> ATAC_CD4_day1 <TAB> CD4_day1 <TAB> 1 <TAB> CD4.bed.gz <TAB> Bed

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

::

    Gene1 <TAB> 12
    Gene2 <TAB> 20
    Gene3 <TAB> 25

HiC
---

+-------------------+-----------------+----------------+--------------+
| Supported formats | Description     | Required tags  | Optional tags|
+===================+=================+================+==============+
| Plain Text        | a list of Loops |"ChromosomeLoop"| None         |
+-------------------+-----------------+----------------+--------------+

Currently the pipeline do not analyze HiC data, so the user need to
provide the end result - a list of loops, in following format:

::

    chrom_1 <TAB> start_1 <TAB> end_1 <TAB> chrom_2 <TAB> start_2 <TAB> end_2

For example:

::

    chr21 <TAB> 29343 <TAB> 500000 <TAB> chr21 <TAB> 1009340 <TAB> 1023400
    chr1  <TAB> 10321 <TAB> 102100 <TAB> chr1  <TAB> 107150  <TAB> 123400

Data in the internet
---------------------

Taiji can automatically download and analyze data from ENCODE portal and GEO database.

Using data from GEO
^^^^^^^^^^^^^^^^^^^

.. note::
    The ``fastq-dump`` is needed for downloading data from GEO database.
    The ``SRA`` needs to be added to the "format" field.


::

    ATAC-Seq:
        - group: 'ATAC_CD4_day1'
          id: 'CD4_day1'
          replicates:
          - rep: 1
            files:
            - path: SRR891275
              format: SRA
              tags: ['PairedEnd']

Using data from ENCODE
^^^^^^^^^^^^^^^^^^^^^^

::

    ATAC-Seq:
    - group: 'heart_left_ventricle'
      id: heart_left_ventricle_ATAC
      replicates:
      - rep: 1
        files:
          - pair:
            - path: ENCFF766IGD
              tags: ['ENCODE']
            - path: ENCFF075UOA
              tags: ['ENCODE']

    RNA-Seq:
    - group: 'heart_left_ventricle'
      id: heart_left_ventricle_RNA
      replicates:
      - rep: 1
        files:
        - path: ENCFF884JDN
          tags: ['ENCODE', 'GeneQuant']