---
layout: post
title:  An annotated SAM format example
author: Zhuyi Xue
tags: genomics, alignment, SAM format
---

SAM (Sequence Alignment/Map) format is one of the most popular formats used for
storing aligment information of high-throughput sequencing data. I've used it
extensively, but still sometimes I find it hard to recognize the columns at
first glance. So I annotated an example from the [SAM format
specification](https://samtools.github.io/hts-specs/SAMv1.pdf) (Version: 1 Jun
2017) for quick reference.

Other related specification can be found at http://samtools.github.io/hts-specs./

<figure>
    <img src="/assets/sam_format_example.jpg" alt="SAM format example "/>
    <figcaption>
    An annotated example of SAM format. (<strong>A</strong>) An example alignment result.
    (<strong>B</strong>) The alignment result represented in SAM format. 
    Black color represents the SAM information, and colorful text is the annotation.
</figcaption>
</figure>
