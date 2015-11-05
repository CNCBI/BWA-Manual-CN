# BWA-Manual-CN
Brief-简介

This is the Chinese translation of BWA's [Introduction](http://bio-bwa.sourceforge.net/) and [Manual](http://bio-bwa.sourceforge.net/bwa.shtml) page. BWA is a very popular aligner for Next Generation Sequencing data. Since most of the Chinese tutorials are incomplete or even with errors, we create this project to put the translation of official manual here. We are tring our best to finish it as good as we can and as soon as possible. Hope more people join us to make it better. Even one sentence sometimes makes a huge difference, like one single base dose.

这是BWA[介绍页](http://bio-bwa.sourceforge.net/)和[使用手册](http://bio-bwa.sourceforge.net/bwa.shtml)的中文翻译。 BWA是常用的第二代高通量测序数据比对软件。由于大多中文教程没能给大家一个完整的说明，有的甚至有错误，我们创建了这个项目来翻译官网的使用说明。我们正在尽最大的努力翻译地更好、更快，希望有更多的朋友加入进来。就算只是一个句子，有时也会产生巨大的影响，就像单个碱基一样。

You can read the Chinese translation of the introduction page of BWA at [here](https://github.com/CNCBI/BWA-Manual-CN/blob/master/Introduction.md).
你可以在[这里](https://github.com/CNCBI/BWA-Manual-CN/blob/master/Introduction.md)查阅BWA介绍页的中文翻译。

#Translation-译文

###NAME-名字
    bwa - Burrows-Wheeler(伯罗斯-惠勒)对比工具

###CONTENTS-目录
    概要
    描述
    命令和选项
    Sam对比格式
    短序列对比的注意事项
        对比准确度
        估计插入大小分布
        内存要求
        速度
    Bwa-0.6的变化
    另请参阅
    作者
    许可证和引用
    历史
  
###SYNOPSIS-概要
```bash
    bwa index ref.fa
    bwa mem ref.fa reads.fq > aln-se.sam
    bwa mem ref.fa read1.fq read2.fq > aln-pe.sam
    bwa aln ref.fa short_read.fq > aln_sa.sai
    bwa samse ref.fa aln_sa.sai short_read.fq > aln-se.sam
    bwa sampe ref.fa aln_sa1.sai aln_sa2.sai read1.fq read2.fq > aln-pe.sam
    bwa bwasw ref.fa long_read.fq > aln.sam
```
###DESCRIPTION-描述
    BWA是一个把低发散序列比对到一个大型参考基因组（比如说人类基因组）上去的软件包。
