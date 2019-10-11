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

assembly
^^^^^^^^

(Optional.) The name of genome assembly (supports "GRCh38", "hg38", "GRCm38" or "mm10").
The ``assembly`` is used to download ``genome``, ``annotation`` and ``motif_file``
when the values of these fields are not provided.

Advanced options
----------------

genome
^^^^^^

(Optional.)
Complete genome in a SINGLE ungzipped FASTA file.

If not specified, this will be downloaded according to ``assembly``.

Example: "/home/kai/genome/GRCh38/genome.fa".

annotation
^^^^^^^^^^

(Optional.)
Genome annotation in *GTF* format. For human and mouse, Gencode annotations
are available at http://www.gencodegenes.org/.
*Very important*: chromosome names in the annotations GTF file have to match
chromosome names in the FASTA genome sequence file. For example, one can use
ENSEMBL FASTA files with ENSEMBL GTF files, and UCSC FASTA files with UCSC
FASTA files. However, since UCSC uses chr1, chr2,... naming convention,
and ENSEMBL uses 1, 2, ... naming, the ENSEMBL and UCSC FASTA and GTF files
cannot be mixed together.

If not specified, this will be downloaded according to ``assembly``.

.. topic:: Example

    annotation: "/home/kai/genome/GRCh38/gencode.v25.annotation.gtf"

motif_file
^^^^^^^^^^

(Optional.)
A plain text file containing PWMs in MEME format.
The naming convention for motifs is ``TF_NAME+OTHER_STRING``, where
the ``TF_NAME`` should match the gene names in your annotation file.
You may include multipe PWMs for same TFs and use ``OTHER_STRING`` to distinguish
them. For example, ``SP1+ID1``, ``SP1+ID2``.
Taiji will combine sites found from different PWMs.

We provide human and mouse motifs here (downloaded from cisBP database):
:download:`Human <data/motifs/cisBP_human.meme>`
and :download:`mouse <data/motifs/cisBP_mouse.meme>` motif files.
When ``motif_file`` is not specified, these will be used according to
the value of the ``assembly`` field.

.. topic:: Example

    motif_file: "/home/kai/motif_databases/cisBP_human.meme"

callpeak_fdr
^^^^^^^^^^^^

(Optional.)
FDR threshold for peak calling in MACS2.

.. topic:: Example

    callpeak_fdr: 0.01

tss_enrichment_cutoff
^^^^^^^^^^^^^^^^^^^^^

(Optional.)
TSS enrichment cutoff for filtering cell in single cell ATAC-seq analysis.

.. topic:: Example

    tss_enrichment_cutoff: 7

external_network
^^^^^^^^^^^^^^^^

(Optional.) External network file to be used in PageRank analysis.

.. topic:: Example

    external_network: "pathway.tsv" 

.. note::
    You don't have to physically provide the following files. But you do need to
    specify the locations where these files will be *GENERATED AUTOMATICALLY WHEN
    FILES/DIRECTORIES DOES NOT EXIST*. If the specified directories or files
    already exist, the program will do nothing.
    If this is the first time you run the program, make sure delete existing
    files/directories first so indices can be generated properly.
    You only need to generate the indices once, *THEY CAN BE REUSED*.

seq_index
^^^^^^^^^

(Optional.)
This is the *FILE* containing GENOME SEQUENCE INDEX.

Example: "/home/kai/genome/GRCh38/GRCh38.index".

bwa_index
^^^^^^^^^

(Optional.)
This is the *DIRECTORY* containing BWA INDICES.

Example: "/home/kai/genome/GRCh38/BWAIndex/".

star_index
^^^^^^^^^^

(Optional.)
This is the *DIRECTORY* containing STAR INDICES.

Example: "/home/kai/genome/GRCh38/STAR_index/".

rsem_index
^^^^^^^^^^

(Optional.)
This is the *DIRECTORY* containing RSEM INDICES.

Example: "/home/kai/genome/GRCh38/RSEM_index/".

.. _distributed_computing:

Distributed computing
---------------------

The following settings are used in the cloud computing mode.

submit_command
^^^^^^^^^^^^^^

The command for submitting jobs.

.. topic:: Example

    submit_command: "qsub"

submit_cpu_format
^^^^^^^^^^^^^^^^^

The command line options for requesting cpu cores.

.. topic:: Example

    submit_cpu_format: "-l nodes=1:ppn=%d"

submit_memory_format
^^^^^^^^^^^^^^^^^^^^

The command line options for requesting memory.

.. topic:: Example

    submit_memory_format: "-l mem=%dG"

submit_params
^^^^^^^^^^^^^

Additional job submission parameters.

.. topic:: Example

    submit_params: "-q glean"


resource
^^^^^^^^

(Optional.)
Specify the computational resources for each step.

.. topic:: Example

    ::
        resource:
            SCATAC_Remove_Duplicates:
                parameter: "-q home -l walltime=24:00:00"

            SCATAC_Merged_Reduce_Dims:
                parameter: "-q home -l walltime=24:00:00"
                cpu: 4
                memory: 80