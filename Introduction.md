#BWA-Introduction-page-CN
Brief-简介

This is the Chinese translation of BWA's [Introduction](http://bio-bwa.sourceforge.net/) page.

这是BWA[介绍页](http://bio-bwa.sourceforge.net/)的中文翻译。 在[这里](https://github.com/CNCBI/BWA-Manual-CN)可以查看其使用手册的中文翻译。

#Translation-译文

###Introduction-简介
BWA是一个把低发散序列比对到一个大型参考基因组（比如说人类基因组）上去的软件包。它由三个算法组成：*BWA-backtrack, BWA-SW*和*BWA-MEM*。第一个算法是为小于等于100bp的illumina测序reads设计的，另外两个是为更长的序列，从70bp到1Mbp，而设计的。BWA-MEM和BWA-SW有一些相同的特性，比如支持长reads和剪切比对，但是BWA-MEM，也是最新的算法，是通常被推荐用来做高质量查询的，因为它更快，更准确。在70-100bp的illumina reads上，BWA-MEM也比BWA-backtrack有更好的性能。

###FAQ-常见问题
#####我如何引用BWA？
    短序列对比工具（bwa-short）已经发表了：
      Li H. and Durbin R. (2009) Fast and accurate short read alignment with Burrows-Wheeler Transform. Bioinformatics, 25:1754-60. [PMID: 19451168](http://www.ncbi.nlm.nih.gov/pubmed/19451168)
    如果你使用了BWA-SW，请引用：
      Li H. and Durbin R. (2010) Fast and accurate long-read alignment with Burrows-Wheeler Transform. Bioinformatics, Epub. [PMID: 20080505](http://www.ncbi.nlm.nih.gov/pubmed/20080505)
    （在下面的勘误中，对这些文章中的公式做了一个小的校正，请查看。）

#####这里有三个算法，我该选择哪一个呢？
    对70bp或更长的，由*illumina，454，Ion Torrent*和*Sanger*测序的reads，组装的contigs（中文可译为重叠群，为基因组组装中常用术语，译后可能让人误解，故不翻译。译者注）和BAC序列，通常首选BWA-MEM算法。对于短序列，BWA-backtrack可能会更好。当比对中的gaps（中文可译为缺口，为基因变异分析中常用术语，译后可能让人误解，故不翻译。译者注）频繁出现时，BWA-SW可能会有更高的敏感性。
    
#####当使用BWA-MEM/BWA-SW时，我的工具在警告有多个首选比对（multiple primary alignments），这是一个漏洞吗？
    这不是的。由于结构变异，基因融合或基因组的错误组装，多个首选比对是有可能的。然而，在SAM中如何表示这种情况还没有被最终确定。要让BWA和你的工具能协同工作，请使用‘-M’选项把多余的hits（中文可译为命中位点）标记为次要的。

#####对测序错误的容忍度是怎样的？
    Bwa-back主要是为测序错误率低于2%时而设计的。虽然用户可以通过调节命令行的选项来要求它容忍更多的错误，但是它的性能会快速下降。注意，对于illumina的reads，在比对之前，bwa-backtrack可以选择性地从3’端修剪低质量的碱基，从而比对更多的reads，尽管它们尾部的错误率很高，这一点在illumina数据中是很典型的。
    
    考虑到是更长的比对，因此BWA-SW和BWA-MEM都可以容忍更多的错误。模拟实验表明，它们在下列情况的比对中工作地很好：100bp-2%的错误率，200bp-3%的错误率，500bp-5%的错误率，1000bp或更长-10%的错误率。
    
#####BWA会找到chimeric reads（嵌合reads，[请查阅](https://www.broadinstitute.org/crd/wiki/index.php/Chimerism)。译者注）吗？
    会，BWA-SW和BWA-MEM都能够找到chimera。对一个read来说，BWA通常报告一个比对；但是如果这个read/contig是一个chimera，可能会输出两个或以上的比对。
    
#####BWA可以像MAQ那样识别SNPs吗？
    不能，BWA只做比对。尽管如此，它输出的比对是SAM格式的，常用的几个识别SNP的软件比如像samtools和GATK都是可以读取这个格式的。
    
#####我注意到在双端测序中的某一个read有很高的比对质量(mapping quality)，另外一个read的却是零。这是正确的吗？
    这是正确的。比对质量是对每个read独立赋值的，而不是对一个read pair（测序对，双端测序中的两条reads。译者注）一起赋值的。某一个read能被非常准确的比对，然而它的另一半却落在了串联重复序列（tandom repeat）里面，这种情况是有可能的。因此它准确的位置也就不能被确定了。
    
#####我看到一个read落在了一条染色体末端，但它却被标记成unmapped（没比对上。译者注）（flag列的标记是0x4）。这是怎么回事？
    BWA内部处理时，会把所有的参考序列连接成一条长序列，一个read可能会被比对到两个相邻参考序列的连接处。这时，BWA会把这个read标记成unmapped，但是你会看到位置信息，CIGAR和其他所有的标签。一个更好的解决方案是报告一个备用的位置或者把超出末端的序列比对修剪掉。但是这在编程上是相当复杂的，目前还没有被实现。
    
#####当总参考序列大于4GB的时候，BWA还能工作吗？
    可以。从0.6.x版开始，所有的BWA算法都可以处理总参考序列大于4GB的情况。但是，单条染色体不能超过2GB。（原文内容是不长于2GB，另一个可能的解读是2G Base，但目前的存储技术，人类3G base的参考基因组，在硬盘上就是3GB大小，所以目前两种理解问题都不大。确切含义有待和作者确认。译者注）
    
###勘误
一个空字符串数组的后缀区间应该是[0,n-1]，其中n是数据库字符串的长度，而不是Li and Durbin (2009 and 2010)在文章里面所说的 [1,n-1]。相应的，我们需要定义O(a,-1)=0并且修改Li and Durbin (2009)文章中图3里的伪代码。BWA软件的实现实际上是对的，只是文章里面出现了这个错误。我们对为此带来的困惑深感歉意，同时感谢Nils Homer和Abel Antonio Carrion Collado指出了这个问题。
