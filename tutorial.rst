Tutorial
========

Introduction
------------

The Taiji software is a versatile genomics data analysis pipeline.
It can be used to analyze ATAC-seq, RNA-seq, single cell ATAC-seq and
Drop-seq data. However, the uniqueness and the power of Taiji really lie in 
its ability to integrate diverse datasets and use these information in a clever
way to construct regulatory network and identify candidate driver genes.

Taiji accepts many data formats. It can start with raw data like fastq or
post-processed files like bam or bed files.

How to use
----------

Preparing input
^^^^^^^^^^^^^^^

To run the Taiji pipeline, you would need 2 configuration files.

The first configuration file is used to specify the options used by the pipeline.
Please look at :download:`this example configuration file <data/example_config.yml>` or :doc:`options` for details.

The second configuration file contains the information about the input data sets.
Take a look at :download:`this example file <data/example_input.yml>` or :doc:`input`.

Visualizing the workflow
^^^^^^^^^^^^^^^^^^^^^^^^

You can use ``taiji view taiji.html`` to generate a HTML output of the workflow.
Use a web browser to open the file and see what components are included in the
Taiji pipeline.

Running the workflow
^^^^^^^^^^^^^^^^^^^^

To run the entire workflow, simply supply the ``taiji`` with the first configuration file:
``taiji run --config example_config.yml``.

Oftentimes you just need to run certain steps. For example, if you only want to 
get some QC metrics, type ``taiji run --config example_config.yml --select SCATAC_QC``.
You can run multiple steps together using
``taiji run --config example_config.yml --select STEP1,STEP2,STEP3``.

Parallelism and distributed computing
-------------------------------------

Taiji can use multiple cores. For example, to use 5 cores, simply type:
``taiji run --config config.yml -n 5 +RTS -N5``.
``-n 5 +RTS -N5`` tells the taiji to use 5 cores/threads.

Taiji also supports distributed computing. This feature can be turned on by adding
the ``--cloud`` flag. To use this feature, you need to have a job scheduling system like
SGE or slurm. See :doc:`advance` for more details.

Auto-recovery
-------------

The pipeline supports auto-recovery, which means you can stop the program at
any time and it will resume from the last checkpoint.
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