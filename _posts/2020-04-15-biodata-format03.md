---
layout: post
title: '常见生物数据格式03-VCF'
date: 2020-04-15
author: 柠檬菌
tags: 生物数据格式
---

>The **Variant Call Format** (**VCF**) specifies the format of a text file used in bioinformatics for storing gene sequence variations.

### VCF文件

Variant Call Format(VCF)文件是存储序列变异信息的文本文件，可以表示SNP，插入/缺失，CNV和结构变异。

VCF文件可分为两部分：

- 头文件注释信息
- 变异位点信息

![vcf-format.png](https://ae01.alicdn.com/kf/Hc0c88fb1ba5b4bb780c21236532faf02o.png)

#### 头文件注释信息

以`##`开头，以key-value键值对的形式记录版本，参考基因组，标签意义等信息

**变异位点信息**


列 | 含义
---|---
#CHROM | 参考基因组染色体
POS | 变异位置，1-based
ID | 变异ID，如dbSNP中的rs number
REF | 参考序列
ALT | 变异序列
QUAL | 变异质量，Phread-scaled，-10 * log prob (ALT is wrong)
FILTER | 过滤状态
INFO | 变异的详细信息，其TAG在头注释文件有说明
FORMAT | GT:DP

**INFO**

- AA : ancestral allele
- AC : allele count in genotypes, for each ALT allele, in the same order as listed
- AF : allele frequency for each ALT allele in the same order as listed: use this when estimated from primarydata, not called genotypes
- AN : total number of alleles in called genotypes
- BQ : RMS base quality at this position
- CIGAR : cigar string describing how to align an alternate allele to the reference allele
- DB : dbSNP membership
- DP : combined depth across samples, e.g. DP=154
- END : end position of the variant described in this record (for use with symbolic alleles)
- H2 :  membership in hapmap2
- H3 :  membership in hapmap3
- MQ : RMS mapping quality, e.g. MQ=52
- MQ0 :  Number of MAPQ == 0 reads covering this record
- NS : Number of samples with data
- SB : strand bias at this position
- SOMATIC : indicates that the record is a somatic mutation, for cancer genomics
- VALIDATED : validated by follow-up experiment
- 1000G : membership in 1000 Genomes

**FORMAT**

- GT, genotype, 0 for the reference allele (what is in the REF field),  1 for the first allele listed in ALT, 2 for the second allele list in ALT and so on. 
  - / : genotype unphased
  - \| : genotype phased
- DP : read depth at this position for this sample
- FT : Filter flag
- GL : Genotype likelihoods 
- GLE: Genotype likelihoods of heterogeneous ploidy
- PL : phred-scaled genotype likelihoods  
- GP : phred-scaled genotype posterior probabilities
- HQ : haplotype qualities
- PS : phase set
- PQ : phasing quality
- EC : expected alternate allele counts
- MQ : RMS mapping quality

#### Reference

1. [https://samtools.github.io/hts-specs/VCFv4.2.pdf](https://samtools.github.io/hts-specs/VCFv4.2.pdf)
2. [https://vip.biotrainee.com/d/164-vcf](https://vip.biotrainee.com/d/164-vcf)
3. [https://zhuanlan.zhihu.com/p/36289359](https://zhuanlan.zhihu.com/p/36289359)