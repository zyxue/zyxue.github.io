---
layout: post
title:  Comparison of common options between rsync and wget
author: Zhuyi Xue
tags: rsync, wget
---


| | `rsync` | `wget` |
|:-|:-------|:------|
|protocal|ssh| ftp, http|
|inlcude files|`--include`, `--include-from`|`--accept`|
|exclude files|`--exclude`, `--exclude-from`|`--reject`|
|dry run|`-n`/`--dry-run`|`--spider`|
