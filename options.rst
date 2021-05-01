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

::

    input: "data/input.yml"

output_dir
^^^^^^^^^^

Directory containing the results.

::

    output_dir: "output/"

assembly
^^^^^^^^

(Optional.) The name of the genome assembly. This is an optional parameter.
We use this parameter to automatically determine the values of ``genome``, ``annotation``
and ``motif_file``.
If your genome assembly is not supported by the program, you will need to mannually
set the parameters for ``genome``, ``annotation`` and ``motif_file``.

We currently support the following assemblies: "GRCh38", "hg38", "GRCm38" or "mm10".

::

    assembly: "GRCh38"

Advanced options
----------------

genome
^^^^^^

(Optional.)
Complete genome in a SINGLE ungzipped FASTA file.

If not specified, this will be downloaded according to ``assembly``.

::

    genome: "/home/kai/genome/GRCh38/genome.fa"

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

::

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

::

    motif_file: "/home/kai/motif_databases/cisBP_human.meme"

genome_index
^^^^^^^^^^^^

(Optional.)
This is the *FILE* containing customized genome sequence index.
The program will detect whether the file exists.
If the index is not present, it will generate the index at the specified location.
If you leave this parameter unspecified,
the program will generate the index in the ``output_dir``.

To avoid re-generate the index for every project, we recommend you to set
this parameter mannually.

::

    genome_index: "/home/kai/genome/GRCh38/GRCh38.index".


bwa_index
^^^^^^^^^

(Optional.)
BWA genome index prefix.
The program will detect whether the directory contain proper BWA indices.
If the indices are not present, it will generate the indices at the specified
location. If you leave this parameter unspecified,
the program will generate the indices in the ``output_dir``.

To avoid re-generate the indices for every project, we recommend you to set
this parameter mannually.

::

    bwa_index: "/home/kai/genome/GRCh38/BWAIndex/genome.fa"

bwa_seed_length
^^^^^^^^^^^^^^^

The "-k" parameter in BWA -- minimum seed length.

::

    bwa_seed_length: 32

star_index
^^^^^^^^^^

(Optional.)
This is the *DIRECTORY* containing STAR INDICES.
The program will detect whether the directory contain proper STAR indices.
If the indices are not present, it will generate the indices within the specified
directory. If you leave this parameter unspecified,
the program will generate the indices in the ``output_dir``.

To avoid re-generate the indices for every project, we recommend you to set
this parameter mannually.

::

    star_index: "/home/kai/genome/GRCh38/STAR_index/"

rsem_index
^^^^^^^^^^

(Optional.)
RSEM genome index prefix.
The program will detect whether the directory contain proper RSEM indices.
If the indices are not present, it will generate the indices at the specified
location. If you leave this parameter unspecified,
the program will generate the indices in the ``output_dir``.

To avoid re-generate the indices for every project, we recommend you to set
this parameter mannually.

::

    rsem_index: "/home/kai/genome/GRCh38/RSEM_index/genome"


callpeak_fdr
^^^^^^^^^^^^

(Optional.)
FDR threshold for peak calling in MACS2.

::

    callpeak_fdr: 0.01

callpeak_genome_size
^^^^^^^^^^^^^^^^^^^^

(Optional.)
The effective genome size used for MACS2's "-g/--gsize" parameter.
This will be automatically determined based on the assembly or genome file.
For human or mouse assembly, we set this parameter to "hs" or "mm".
For other genome, we set this parameter to ``0.9 * GENOME_SIZE``.
The value of this parameter usually doesn't make big difference.

::

    callpeak_genome_size: "2.7e9"

external_network
^^^^^^^^^^^^^^^^

(Optional.) External network file to be used in PageRank analysis.

::

    external_network: "pathway.tsv" 

tmp_dir
^^^^^^^

(Optional.) The directory for storing temporary files.

::

    tmp_dir: "/tmp"


Single cell ATAC-seq analysis
-----------------------------

Parameters related to scATAC-seq analysis. All parameters in this section
should be specified under `scatac_options`. For example:

::

    scatac_options:


tss_enrichment_cutoff
^^^^^^^^^^^^^^^^^^^^^

(Optional.)
TSS enrichment cutoff for filtering cell in single cell ATAC-seq analysis.

::

    tss_enrichment_cutoff: 7


fragment_cutoff
^^^^^^^^^^^^^^^

(Optional.) Used to remove cells that do not have enough fragments/reads.

::

    fragment_cutoff: 1000

doublet_score_cutoff
^^^^^^^^^^^^^^^^^^^^

::
    doublet_score_cutoff: 0.5

cell_barcode_length
^^^^^^^^^^^^^^^^^^^

(Optional.)

::
    cell_barcode_length: 10

cluster_optimizer
^^^^^^^^^^^^^^^^^

(Optional.) Quality function used in graph clustering. Available options are `RBConfiguration` and `CPM`.
`RBConfiguration` optimizes modularity and has resolution limit while
`CPM` is resolution-limit free.

::

    cluster_optimizer: CPM

cluster_resolution
^^^^^^^^^^^^^^^^^^

Mannually specify the clustering resolution. Otherwise it will be automatically determined.

::

    cluster_resolution: 1

cluster_resolution_list
^^^^^^^^^^^^^^^^^^^^^^^

::

    cluster_resolution_list: [0.005, 0.01, 0.02, 0.04, 0.08, 0.16, 0.32, 0.64, 0.8, 1]

cluster_by_window
^^^^^^^^^^^^^^^^^

Whether to use bin/window based clustering. Default is to use peak based clustering.

::
    cluster_by_window: False

window_size
^^^^^^^^^^^

Window/bin size.

::
    window_size: 5000


do_subclustering
^^^^^^^^^^^^^^^^

Whether to perform iterative clustering to identify subclusters.

::
    do_subclustering: False

subcluster_resolution    
^^^^^^^^^^^^^^^^^^^^^


cluster_exclude
^^^^^^^^^^^^^^^


Single cell RNA-seq analysis
-----------------------------

scrna_cell_barcode_length
^^^^^^^^^^^^^^^^^^^^^^^^^

The length of the cell barcode used in demultiplexing.

::

    scrna_cell_barcode_length: 12

scrna_umi_length
^^^^^^^^^^^^^^^^

The length of the UMI used in demultiplexing.

::

    scrna_umi_length: 8

scrna_doublet_score_cutoff
^^^^^^^^^^^^^^^^^^^^^^^^^^

(Optional.) Cutoff for doublet detection, a value between 0 and 1 reflecting
how likely a "cell" is a doublet. (default is 0.5)

::

    scrna_doublet_score_cutoff: 0.5

scrna_cluster_resolutions
^^^^^^^^^^^^^^^^^^^^^^^^^


.. _distributed_computing:

Distributed computing
---------------------

The following settings are used in the cloud computing mode.

submit_command
^^^^^^^^^^^^^^

The command for submitting jobs.

::

    submit_command: "qsub"

submit_cpu_format
^^^^^^^^^^^^^^^^^

The template for specifying the number of cpu cores in the job submission script.
This is system/environment dependent. 
For example, if your system uses ``-l nodes=1:ppn=XX`` to allocate CPU resource,
you should put ``-l nodes=1:ppn=%d`` here.
The ``%d`` will be replaced by the actual numbers when submitting the jobs.
The CPU cores for individual steps can be modified in the :ref:`job-resource` section below.

::

    submit_cpu_format: "-l nodes=1:ppn=%d"

submit_memory_format
^^^^^^^^^^^^^^^^^^^^

The template for specifying the amount of memory in the job submission script.
This is system/environment dependent. 
For example, if your system uses ``-l mem=XXG`` to allocate memory resource,
you should put ``-l mem=%dG`` here.
The ``%d`` will be replaced by the actual numbers when submitting the jobs.
The memory for individual steps can be modified in the :ref:`job-resource` section below.

::

    submit_memory_format: "-l mem=%dG"

submit_params
^^^^^^^^^^^^^

Additional parameters to be included in the submission command.

::

    submit_params: "-q glean"


.. _job-resource:

resource
^^^^^^^^

(Optional.)
Specify the computational resources for each step.

::

    resource:
        SCATAC_Remove_Duplicates:
            parameter: "-q home -l walltime=24:00:00"

        SCATAC_Merged_Reduce_Dims:
            parameter: "-q home -l walltime=24:00:00"
            cpu: 4
            memory: 80