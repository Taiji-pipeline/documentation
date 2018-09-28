Tutorial
========

Introduction
------------

The Taiji pipeline aims at integrating different kinds of high throughput profiling techniques to construct TF regulatory networks and identify key regulators through network analysis.
That being said, one only needs ATAC-seq or DNase-seq data to run the analysis, though the result will be better given other information, especially RNA-seq data.

The data input to the Taiji pipeline are fastq or bam files.
For gene expression data, you have the option to provide an external gene expression table instead of having the pipeline analyze the raw RNA-seq data for you.

How to use
----------

To run the Taiji pipeline, you would need 2 configuration files.

The first configuration file is used to specify the options used by the pipeline.
Please look at :download:`this example configuration file <data/example_config.yml>` or :doc:`options` for details.

The second configuration file contains the information about the input data sets.
Take a look at :download:`this example file <data/example_input.yml>` or :doc:`input`.

To run the pipeline, supply the ``taiji`` with the first configuration file: ``taiji run --config example_config.yml`` or ``taiji run --config example_config.yml --remote`` if the program is compiled with ``sge`` flag.

Parallelism
-----------

Taiji supports two levels of parallelism -- node level and workflow level. Node
level parallelism is automatically turned on when compiling with the ``drmaa`` flag.
The workflow level parallelism can be turned on using `-N <num_of_process>`.
However, this is only recommended for users with a super computer, as it will
consume a lot of memory.

Auto-recovery
-------------

The pipeline supports auto-recovery, which means you can stop the program at any time and it will resume from the last checkpoint.
The checkpoints are saved in a file called "sciflow.db".
Delete this file if you want a fresh run.

Results
-------

The structure of the results in the output directory looks like this:

::

    output_directory
    +-- ATACSeq
    |   +-- Bam
    |   |   +-- *.bam              (raw alignment files)
    |   |   +-- *_filt.bam         (filtered alignment files)
    |   |   +-- *_filt_dedup.bam   (filtered and deduplicated alignment files)
    |   +-- Bed
    |   |   +-- *.bed.gz           (BED files converted from Bam files)
    |   |   +-- *.merged.bed.gz    (merged BED file from all replicates)
    |   +-- Download               (any files that are downloaded from the web)
    |   +-- Peaks
    |   |   +-- *.narrowPeak       (narrow peaks called by MACS2 using loose cutoff)
    |   +-- TFBS
    |   |   +-- *.bed              (TF binding sites found in open chromatin)
    |   +-- openChromatin.bed      (the union of accessible chromatin from all samples)
    |   +-- QC.tsv                 (some quality control metrics)
    +-- Network                    (network related files)
    +-- Promoters
        +-- *_promoters.bed        (active promoters)
    +-- RNASeq
        +-- *_gene_quant.tsv       (gene quantification)
        +-- *.bam                  (alignment files)
        +-- expression_profile.tsv (expression profiles from all samples in one file)
    +-- GeneRanks_PValues.tsv      (p-values for the PageRank scores)
    +-- GeneRanks.tsv              (PageRank scores)


Visualization
-------------

Download the software from `here <https://github.com/Taiji-pipeline/Taiji-view>`_. Use ``stack install`` to install the software.

To visualize the result, use:

::

    taiji-view rank GeneRanks.tsv --expression RNASeq/expression_profile.tsv output.svg

``GeneRanks.tsv`` is the file containing the PageRank result; ``RNASeq/expression_profile.tsv`` is the file containing the gene expressions; ``output.svg`` is the output file.

Filtering the result
^^^^^^^^^^^^^^^^^^^^

``--cv`` can be used to filter the result based on the coefficient of variance (CV) of PageRank scores. For example, ``--cv 0.5`` will exclude any gene with CV less than 0.5. The default value is 1, which keeps the highly variable genes.

The file type of the output is determined by the suffix of file name. Supported file extensions include ".png", ".pdf", ".jpg", ".svg".