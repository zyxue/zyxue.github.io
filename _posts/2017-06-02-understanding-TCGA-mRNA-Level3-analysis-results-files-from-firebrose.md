---
layout: post
title: Understanding TCGA mRNA Level3 analysis results files from FireBrowse
author: Zhuyi Xue
tags: mRNA-Seq, FireBrowse, FireHose, RSEM
---

I am trying to figure out which mRNA analysis results file from Firehose I
should use for differential gene expression (e.g. with DESeq2). However, I find
the documentation relatively poor. As I have spent quite some time figuring out
all the details, I decided to share what I've found here so future users don't
need to.

I am currently looking at KICH. If you click KICH at http://firebrowse.org/, and
then click mRNA, you see the following pop up menu.

<img src="/assets/KICH-FireBrowse-mRNA-analysis-results-files.png" width="600"/>

I tried to understand all the difference, and found the following pages somewhat helpful.

1. [confluence.broadinstitute.org](https://confluence.broadinstitute.org/download/attachments/29790363/DESCRIPTION.txt?version=1&modificationDate=1363806109000)
2. [wiki.nci.nih.gov](https://wiki.nci.nih.gov/display/tcga/rnaseq+version+2) (This is about the RNASeqV2 pipeline in general)

Based on the description of the second link, I thought I would be interested in
the `illuminahiseq_rnaseqv2-RSEM_genes` file as DESeq2 require expression values
as close to [raw count][2] as possible.

However, when I download it and look inside, it contains headers as such with
very long lines.

```
Hybridization  REF        TCGA-KL-8323-01A-21R-2315-07  TCGA-KL-8323-01A-21R-2315-07  TCGA-KL-8323-01A-21R-2315-07  TCGA-KL-8324-11A-01R-2315-07  TCGA-KL-8324-11A-01R-2315-07  TCGA-KL-8324-11A-01R-2315-07  TCGA-KL-8324-01A-11R-2315-07...
gene_id        raw_count  scaled_estimate               transcript_id                 raw_count                     scaled_estimate               transcript_id                 raw_count                     scaled_estimate...
?|100130426    0.00       0                             uc011lsn.1                    0.00                          0                             uc011lsn.1                    0.00                          0...
```

I find 

1. it confusing why transcript_ids are involved when it's a file about genes
2. the file format is not very intuitive, and creates inconvenience for future
parsing. Such nested structure would've been much better handled by
XML/JSON/YAML, etc instead.

The normalization method is briefly described in the first link

> RSEM expression estimates are normalized to set the upper quartile count at 1000 for gene level
and 300 for isoform level estimates.  These files have two columns, feature name and normalized
count

I also looked into the `mRNAseq_Preprocess` link, which was not described in the
above links. I found an overly short description
[here](https://confluence.broadinstitute.org/display/GDAC/Documentation):

> mRNAseq_preprocessor: Pick the "normalized_count"(quantile normalized RSEM)
> value from illumina hiseq/ga2 mRNAseq level_3 (v2) data set and make the
> mRNAseq matrix with log2 transformed for the downstream analysis. To maximize
> sample counts we include both HiSeq and GA2 aliquots in each cohort dataset,
> but if a given patient has both HiSeq and GA2 aliquots the HiSeq aliquot will
> take precedence (to avoid double-counting a patient during analysis). The
> pipeline also will create the matrix with RPKM and log2 transform from
> hiseq/ag2 mRNAseq level 3 (v1) data set.

> Z score calculation of RSEM/RPKM data:

> Z = (expression in single tumor sample) - (mean expression in all tumor
> samples ) / (standard deviation of expression in all tumor samples)

The Preprocess files after download look like this

 1. `KICH.uncv2.mRNAseq_RSEM_Z_Score.txt`
 2. `KICH.uncv2.mRNAseq_RSEM_normalized_log2.txt`
 3. `KICH.uncv2.mRNAseq_RSEM_normalized_log2_PARADIGM.txt`
 4. `KICH.uncv2.mRNAseq_RSEM_all.txt `
 5. `KICH.uncv2.mRNAseq_raw_counts.txt`
 6. `KICH.uncv2.mRNAseq_scaled_estimate.txt`
 7. `KICH.uncv2.mRNAseq_geneid_transcriptid_mapping.txt`
 8. `MANIFEST.txt`

File 1 and 2 are briefly explained in the above quote. I don't quite understand
why that is called a Z score, the formula is equal to [standardization][3] (i.e.
removing mean, and scaled to unit variance). Also, it does not say if how
normal/metastatic samples' Z scores are calculated. File 3 is not described, but
PARADIGM seems to be related to pathway analysis (keyword search within that
linked page). File 4 and 5 are likely extracted from the `raw_count` and
`scaled_estimate` columns of the `RSEM_genes` (unnormalized) file mentioned
before. I checked a few genes and File 7 seems to the same as the
`RSEM_genes_normalized` file (but slightly different file formats), consistent
with the description of **mRNAseq_preprocessor** part under **RNAseq Pipelines**
section.

I think the documentation could've been much improved so that no one needs to
hack around file formats, which bioinformaticians have already spent too much
time doing so in general.

Back to my own problem, I think I will try the `raw_count` file (e.g.
`KICH.uncv2.mRNAseq_raw_counts.txt`) first for differential expression analysis
with DESeq2.

  [1]: https://ibb.co/iXrmoF
  [2]: https://bioconductor.org/packages/release/bioc/vignettes/DESeq2/inst/doc/DESeq2.html#why-un-normalized-counts
  [3]: https://en.wikipedia.org/wiki/Feature_scaling#Standardization
