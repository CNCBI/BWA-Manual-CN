# BWA-Manual-CN
Brief-简介

This is the Chinese translation of BWA's [Introduction](http://bio-bwa.sourceforge.net/) and [Manual](http://bio-bwa.sourceforge.net/bwa.shtml) page. BWA is a very popular aligner for Next Generation Sequencing data. Since most of the Chinese tutorials are incomplete or even with errors, we create this project to put the translation of official manual here. We are tring our best to finish it as good as we can and as soon as possible. Hope more people join us to make it better. Even one sentence sometimes makes a huge difference, like one single base dose.

这是BWA[介绍页](http://bio-bwa.sourceforge.net/)和[使用手册](http://bio-bwa.sourceforge.net/bwa.shtml)的中文翻译。 BWA是常用的第二代高通量测序数据比对软件。由于大多中文教程没能给大家一个完整的说明，有的甚至有错误，我们创建了这个项目来翻译官网的使用说明。我们正在尽最大的努力翻译地更好、更快，希望有更多的朋友加入进来。就算只是一个句子，有时也会产生巨大的影响，就像单个碱基一样。

#Translation-译文

You can read the Chinese translation of the introduction page of BWA at [here](https://github.com/CNCBI/BWA-Manual-CN/blob/master/Introduction.md).

你可以在[这里](https://github.com/CNCBI/BWA-Manual-CN/blob/master/Introduction.md)查阅BWA介绍页的中文翻译。

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
    BWA是一个把低发散序列比对到一个大型参考基因组（比如说人类基因组）上去的软件包。它由三个算法组成：BWA-backtrack, BWA-SW和BWA-MEM。第一个算法是为小于等于100bp的illumina测序reads设计的，另外两个是为更长的序列，从70bp到1Mbp，而设计的。BWA-MEM和BWA-SW有一些相同的特性，比如支持长reads和剪切比对，但是BWA-MEM，也是最新的算法，是通常被推荐用来做高质量查询的，因为它更快，更准确。在70-100bp的illumina reads上，BWA-MEM也比BWA-backtrack有更好的性能。
    
    对于所有的算法，BWA首先需要（用**index**命令）建立参考基因组的FM-索引。比对算法要通过不同的子命令来调用：***aln/samse/sampe***对应BWA-backtrack，**bwasw**对应BWA-SW，**mem**对应BWA-MEM算法。
    
###COMMANDS AND OPTIONS-命令和选项
**index**<space><space><space><space>```bwa index [-p prefix] [-a algoType] <in.db.fasta>```
对FASTA格式的database(可译为数据库，原意为要建立索引的参考基因组文件。译者注)序列建立索引。<br/>
选项：<br/>
**-p** *STR* 输出数据库的前缀。\[和db文件名相同\]([ ]中的内容表示不指定该参数时，命令自动添加的默认值。译者注)<br/>
**-a** *STR* 建立BWT索引的算法，可选的选项有：<br/>
<space><space><space><space>**is**
IS线性时间算法来创建数组后缀。这需要5.37N的内存，N是database的大小。IS稍微快一点，但是当databse大于2GB时，它就不能工作了。由于它的简便性，IS是默认的算法。现在IS算法的代码是由Yuta Mori重新实现的。<br/>

<space><space><space><space>**bwtsw**
BWT-SW中实现的算法，这个方法可以对人类全基因组建立索引。

**mem**<space><space><space><space>```bwa mem [-aCHMpP] [-t nThreads] [-k minSeedLen] [-w bandWidth] [-d zDropoff] [-r seedSplitRatio] [-c maxOcc] [-A matchScore] [-B mmPenalty] [-O gapOpenPen] [-E gapExtPen] [-L clipPen] [-U unpairPen] [-R RGline] [-v verboseLevel] db.prefix reads.fq [mates.fq]```
