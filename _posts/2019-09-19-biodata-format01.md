---
layout: post
title: '常见生物数据格式01-FASTA/FASTQ'
date: 2019-09-19
author: 柠檬菌
tags: 生物数据格式
---

> 我们不生产数据，只是数据的搬运工。

在进行生物数据分析之前，有必要先了解一下常见的生物数据格式，不然可能会犯一些意料不到的错误。

常见的生物数据格式有：

- FASTA
- FASTQ
- GFF/GTF/BED/... 
- VCF
- SAM/BAM

<img src="https://ae01.alicdn.com/kf/H7e8b085c89fa49f1b33602844981ce21D.png" alt="ngs_map_read_file_formats.png" style="zoom:36%;" />

### FASTA

记录DNA/Protein序列的文本格式，其由两部分组成:
- 描述行，以`>`起始，记录序列ID，名称或注释信息
- 序列数据，一般为大写格式
```{bash,eval=FALSE,echo=TRUE}
>gi|31563518|ref|NP_852610.1| microtubule-associated proteins 1A/1B light chain 3A isoform b [Homo sapiens]
MKMRFFSSPCGKAAVDPADRCKEVQQIRDQHPSKIPVIIERYKGEKQLPVLDKTKFLVPDHVNMSELVKI
IRRRLQLNPTQAFFLLVNQHSMVSVSTPIADIYEQEKDEDGFLYMVYASQETFGFIRENE
```
有些从Ensembl下载的基因组数据中，序列中包含小写字母或`N`，这是因为基因组文件中包含大量的重复序列，`soft-mask`将重复序列从大写字母表示转化为小写字母表示；`hard-mask`将重复序列转化为`N`s。

### FASTQ

记录测序序列及其质量得分的文本格式，序列与质量得分用单个ASCII字符表示。

Phred64编码格式，碱基质量值为字符的十进制ASCII码减去64

Phred32编码格式，碱基质量值为字符的十进制ASCII码减去32

通常一个序列由4行组成：

1. 以`@`开头，之后为序列的ID及描述信息(与fasta格式的描述行类似);
2. 序列信息;
3. 以`+`开头，之后可以再加上序列的ID及描述信息(可选);
4. 质量得分，与第二行长度相同。

```{bash,eval=FALSE,echo=TRUE}
@SEQ_ID
GATTTGGGGTTCAAAGCAGTATCGATCAAATAGTAAATCCATTTGTTCAACTCACAGTTT
+
!''*((((***+))%%%++)(%%%%).1***-+*''))**55CCF>>>>>>CCCCCCC65
```

#### Illumina序列标识符

```{bash,eval=FALSE,echo=TRUE}
@HWUSI-EAS100R:6:73:941:1973#0/1
```

| HWUSI-EAS100R | the unique instrument name                                   |
| ------------- | ------------------------------------------------------------ |
| 6             | flowcell lane                                                |
| 73            | tile number within the flowcell lane                         |
| 941           | 'x'-coordinate of the cluster within the tile                |
| 1973          | 'y'-coordinate of the cluster within the tile                |
| #0            | index number for a multiplexed sample (0 for no indexing)    |
| /1            | the member of a pair, /1 or /2 *(paired-end or mate-pair reads only)* |

Illumina 1.4之后multiplex ID使用`#NNNNNN`替代`#0`，`#NNNNNN`为sequence of the multiplex tag。
```{bash,eval=FALSE,echo=TRUE}
@EAS139:136:FC706VJ:2:2104:15343:197393 1:Y:18:ATCACG
```

| EAS139  | the unique instrument name                                   |
| ------- | ------------------------------------------------------------ |
| 136     | the run id                                                   |
| FC706VJ | the flowcell id                                              |
| 2       | flowcell lane                                                |
| 2104    | tile number within the flowcell lane                         |
| 15343   | 'x'-coordinate of the cluster within the tile                |
| 197393  | 'y'-coordinate of the cluster within the tile                |
| 1       | the member of a pair, 1 or 2 *(paired-end or mate-pair reads only)* |
| Y       | Y if the read is filtered, N otherwise                       |
| 18      | 0 when none of the control bits are on, otherwise it is an even number |
| ATCACG  | index sequence                                               |

最近版本的Illumina输出格式为：
```{bash,eval=FALSE,echo=TRUE}
@EAS139:136:FC706VJ:2:2104:15343:197393 1:N:18:1
```

#### 质量Score计算

测序仪测序是根据荧光信号强弱给出参考测序错误概率(probability that the corresponding base call is incorrect,P)。

Sanger使用：$Q_{sanger}=-10*\log_{10}P$，其也称为Phred质量得分

Solexa(Illumina Genome Analyzer)使用：$Q_{solexa-prioor to v.1.3}=-10*\log_{10}\frac{P}{1-P}$

二者区别如下图所示：![Probability_metrics.png](https://ae01.alicdn.com/kf/Hd03b095bd3af48e89488dd3cb9b22bcfp.png)

#### 质量得分编码

<img src="https://ae01.alicdn.com/kf/H471cbf9fc82a44e686bd043effab413dA.png" alt="score_encoding.png" style="zoom:75%;" />

#### 检测编码类型

通过上图，可以做类似这种决策树：

包含字符`0`：

- 包含字符`J`：Illumina 1.8+ Phred33
- 不包含字符`J`：Sanger Phred33

不包含字符`0`:

- 包含字符`=`：Solexa Phred64
- 不包含字符`=`，包含字符`A`：Illumina1.3 Phred64
- 不包含字符`=`，不包含字符`A`：Illumina1.5 Phred64

可以使用Github的[guess-encoding.py](https://github.com/brentp/bio-playground/blob/master/reads-utils/guess-encoding.py)来进行判断

```bash
gunzip -c file.fastq.gz | awk 'NR % 4 == 0' | head -n 1000000 | python guess-encoding.py
```