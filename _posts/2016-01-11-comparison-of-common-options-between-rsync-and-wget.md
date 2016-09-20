---
layout: post
title:  Comparison of Common Options between rsync and wget
author: Zhuyi Xue
tags: rsync, wget
---


| | `rsync` | `wget` |
|:-|:-------|:------|
|protocal|ssh| ftp, http|
|inlcude files|`--include`, `--include-from`|`--accept`|
|exclude files|`--exclude`, `--exclude-from`|`--reject`|
|dry run|`-n`/`--dry-run`|`--spider`|
|skip if files exist|won't redownload by default|`--nc`/`--no-clobber`|
