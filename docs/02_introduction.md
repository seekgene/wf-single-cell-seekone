This workflow extracts cell barcodes and UMIs from [SeekGene](https://www.seekgene.com/) SeekOne®-generated single-cell libraries.
It was initially created as a Nextflow port of [Sockeye](https://github.com/nanoporetech/sockeye).

> **Note:** This workflow is a fork of [epi2me-labs/wf-single-cell](https://github.com/epi2me-labs/wf-single-cell) (by Oxford Nanopore Technologies), modified to add support for SeekGene SeekOne® single-cell libraries.

In brief, the workflow does the following:

+ Adapter identification, fused read splitting and stranding.
+ Mapping of reads to genomic reference.
+ Gene and transcript read assignment.
+ Cell barcode and UMI extraction and correction.
+ Generation of gene and transcript count matrices for unique UMIs.
+ Tagging BAM files with cell barcodes and UMIs.
+ Calculation of library saturation.

<figure>
<img src="docs/images/seekone_workflow.svg" alt="wf-single-cell workflow schematic."/>
<figcaption>Schematic depicting the wf-single-cell (SeekOne) workflow.</figcaption>
</figure>

This workflow supports the following SeekOne (SeekGene) kits:
+ SeekOne DD 3' single-cell gene expression (3'-library): `3seekone:DDv2`
+ SeekOne DD 5' single-cell gene expression (5'-library): `5seekone:DDv2`
+ scFAST single-cell full-length transcriptome: `flseekone:DDv1`

The [BLAZE](https://github.com/shimlab/BLAZE) preprint provided useful benchmarking of the original sockeye implementation. 
This assisted in the selection of appropriate thresholds for cell cut-off and for defining the limits of the gene x cell matrix.

The isoform selection procedure used in this workflow was adapted from that found in the [FLAMES](https://github.com/LuyiTian/FLAMES) 
package.
