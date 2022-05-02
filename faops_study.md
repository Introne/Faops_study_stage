# Faops Instructions
## 一. 导论

随着基因组计划的进行，人类和各种模式生物的基因组序列汇成了潮水般的DNA信息量。对这些基因组资料进行大规模比较，寻找其最大相似性（同源性），或搜索序列上的局部特征，或研究由同一个祖先基因特化而来的对应基因，或利用进化分析方法，从而能够鉴定和预测未知的ORF或非编码区各元件的生物学功能。基于这些巨量序列信息，就可以在分子层面上探索人类健康和疾病，乃至物种进化发展奥秘。从老年痴呆症到癌症等不治之症的攻克，从疾病预防到优生优育到长寿……，几十年，上百年后，生物技术在各个领域的应用，将使人类生存的质量和境况发生革命性的飞跃。  

目前，随着测序通量的上升和成本下沉，使得昔日王榭堂前燕，飞入寻常百姓家，基因组测序已是时下热门。数据解读的地位不言而喻，如何准确高效的读取序列信息也成为了科学研究的关键。

faops就是其中一款简便集成的基因组统计分析软件，基于C语言编写，运行速度极快，能极大的提高工作效率。faops日常序列的处理包括，比如：格式化序列，截取序列，随机抽取序列，序列统计值计算等。faops将帮助科研工作者更好地解密DNA序列。
  
## 二. What’s faops
### 2.1 faops开发背景
faops可以被视为[UCSC Jim Kent's utilities](http://hgdownload.cse.ucsc.edu/admin/exe/)旗下的序列分析工具：faCount, faSize, faFrag, faRc, fasomerecord, faFilter和faSplit的集合。  

faops的开发参考了[seqtk](https://github.com/lh3/seqtk)和[ufasta](https://genome.umd.edu/masurca.html)的某些功能。
### 2.2 适用对象 
faops是一个轻量级工具，可以处理FASTA或FASTQ格式（忽略质量值）的序列，这些文件也可以被gzip压缩。  
### 2.3 工作环境
faops的编写语言为C语言，可在Windows环境和Linux环境运行。  


 ## 2.4 How to obtain a faops
 本软件可以使用Linuxbrew直接安装，命令如下：
 ```bash
 brew install wang-q/tap/faops
 ```
也可下载软件包后，在Linux，macOS（gcc or clang）和Windows（MinGW）系统环境自行编译：
```bash
git clone https://github.com/wang-q/faops
cd faops
make
```
注：本教程里的faops命令例子都是在ubuntu中演示的。

## 三. How to use faops
### 3.1 获取使用帮助：faops help
在操作系统上直接敲faops help回车后，即返回faops的所有功能选项。

##### 输入
```
faops help
```
##### 输出 
```bash
Usage:     faops <command> [options] <arguments>
Version:   0.8.21

Commands:
    help           print this message
    count          count base statistics in FA file(s)
    size           count total bases in FA file(s)
    masked         masked (or gaps) regions in FA file(s)
    frag           extract sub-sequences from a FA file
    rc             reverse complement a FA file
    one            extract one fa record
    some           extract some fa records
    order          extract some fa records by the given order
    replace        replace headers from a FA file
    filter         filter fa records
    split-name     splitting by sequence names
    split-about    splitting to chunks about specified size
    n50            compute N50 and other statistics
    dazz           rename records for dazz_db
    interleave     interleave two PE files
    region         extract regions from a FA file

Options:
    There're no global options.
    Type "faops command-name" for detailed options of each command.
    Options *MUST* be placed just after command.
```
## 3.2 统计序列总长和碱基数量：faops count
命令count用于统计一个或多个序列文件中各序列的总长和各序列的碱基数量分情况。 

#### 用法

```bash
faops count <in.fa> [more_files.fa] 
```
#### 运行测试
##### 输入
```bash
faops count in.fa
```
其中，in.fa文件内容为：  
```bash
>test0
tCGTTTAACCCAAatcAAGGCaatANNNCAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAnnCcGagataAG
>test1
taGGCGcgCggtggGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatc
GCaTaTC
>test4
>test7
gTTTTcttaGNNgCgtccCGAAgcAtCtCTagCCgggGgTAatctccAggtgTgTttGTTaCCtcCTCGtgACCC
```
##### 输出
```bash
#seq    len     A       C       G       T       N
test0   134     40      30      34      25      5
test1   103     24      25      30      24      0
test4   0       0       0       0       0       0
test7   75      10      21      19      23      2
total   312     74      76      83      72      7
```
## 3.3 统计序列长度：faops size
命令faops size可以统计一个或多个序列文件中序列长度信息。

#### 用法
```bash
faops size <in.fa> [more_files.fa]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops size in.fa
```
##### 输出
```bash
test0   136
test1   103
test4   0
test7   75
```
## 3.4 定位冗余碱基：faops masked 
命令faops masked可以快速找到一个或多个序列文件中冗余碱基位置，比如N碱基和gap等。

#### 用法
```bash
faops masked [options] <in.fa> [more_files.fa]

"options"
-g         only record regions of N/n
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops masked -g in.fa
```
##### 输出
```bash
test0:26-28
test0:103-104
test0:125-126
test7:11-12
```

## 3.5 提取指定位置的序列片段：faops frag
命令frag可以从序列文件中准确提取指定位置的片段。值得注意的是：如果一个序列文件中包含多个序列，则默认对其中第一个序列进行提取操作。此外，可以通过手动添加参数-l来指定提出片段的分行宽度，序列分行的宽度默认为80。
#### 用法
```bash
faops frag [options] <in.fa> <start> <end> <out.fa>

"options"
-l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops frag -l 60 in.fa 18 88 out.fa
```
##### 输出
返回提示  
```bash
More than one sequence in in.fa, just using first
```
out.fa文件内容
```bash
>test0:18-88
AGGCaatANNNCAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGcaGCAtCtGc
```

## 3.6 获得反向“和”“或”互补序列：faops rc
命令rc可以获得一个或多个序列的反向或互补序列。本功能可以根据使用者实际需求，使用不同参数生成反向、互补、反向且互补这三种形式的序列。其中，参数-f 列表里的序列名称以回车换行进行分隔。
#### 用法
```bash
faops rc [options] <in.fa> <out.fa>

"options"
-n         keep name identical (don't prepend RC_) 
-r         just Reverse, prepends R
-c         just Complement, prepends C 
-f STR     only RC sequences in this list.file 
-l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops rc -l 60 -n -f list.file in.fa out.fa
```
list.file文件内容 
```bash
test0
test7
```
##### 输出
out.fa文件内容
```bash
>test0
CTtatctCgGnnTtCGatacCATtttccGcTagagAATGGGGCtcagCaGaTGCtgCTtc
AcTcCAtTtcCcaCTCatcgagTTTgTgAcatGggCGGccCaccTGNNNTattGCCTTga
tTTGGGTTAAACGa
>test1
taGGCGcgCggtggGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggG
AAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC
>test4

>test7
GGGTcaCGAGgaGGtAACaaAcAcaccTggagatTAcCcccGGctAGaGaTgcTTCGgga
cGcNNCtaagAAAAc
```

## 3.7 提取特定list中的序列：faops some 
命令faops some可以根据列表文件里的序列名称提取特定序列，其中，列表里的序列名称以回车换行进行分隔。本功能也可通过添加参数-i来反向提取名称不在列表中的序列。
#### 用法
```bash
faops some [options] <in.fa> <list.file> <out.fa>

"options"
-i         Invert, output sequences not in the list
-l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops some -i -l 60 in.fa list.file out.fa
```
list.file文件内容与3.6一致
##### 输出
out.fa文件内容
```bash
>test1
taGGCGcgCggtggGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggG
AAcTtcgaccGgtCTCgGccCtatAtgaTtCcGatcGCaTaTC
>test4
```

## 3.8 按照指定规则重排序列：faops order
命令faops order可以将序列文件数据全部读入内存，然后按照提供的顺序输出到新文件中。本功能类似some，但内存调度策略存在差别：order会一次性占用更高的内存。
#### 用法
```bash
faops order [options] <in.fa> <list.file> <out.fa>

"options"
-l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入

注：此命令行实现了序列从长到短的排列
```bash
  faops order in.fa \
      <(faops size in.fa | sort -n -r -k2,2 | cut -f 1) \
      out.fa
```
##### 输出
out.fa文件内容
```bash
>test0
tCGTTTAACCCAAatcAAGGCaatANNNCAggtGggCCGccCatgTcAcAAActcgatGAGtgGgaAaTGgAgTgaAGca
GCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCGaAnnCcGagataAG
>test1
taGGCGcgCggtggGATTAaggCAGaggtTgCGCGCtTgaTAaAACTacgtaACatcggGAAcTtcgaccGgtCTCgGcc
CtatAtgaTtCcGatcGCaTaTC
>test7
gTTTTcttaGNNgCgtccCGAAgcAtCtCTagCCgggGgTAatctccAggtgTgTttGTTaCCtcCTCGtgACCC
>test4

```

## 3.9 替换序列名: faops replace
命令faops replace能够实现对特定序列名的替换，也可以仅对指定序列的提取、改名并输出。其中，replace.tsv文件里名称的变换可以用制表符tab分隔开。
#### 用法
```bash
faops replace [options] <in.fa> <replace.tsv> <out.fa>

"options"
  -s         only output sequences in the list, like faops some
  -l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops replace -s -l 60 in.fa replace.tsv out.fa
```
repalce.tsv文件内容
```bash
test0	read0
test4	read4
```
##### 输出
out.fa文件内容
```bash
>read0
tCGTTTAACCCAAatcAAGGCaatANNNCAggtGggCCGccCatgTcAcAAActcgatGA
GtgGgaAaTGgAgTgaAGcaGCAtCtGctgaGCCCCATTctctAgCggaaaATGgtatCG
aAnnCcGagataAG
>read4

```

## 3.10 过滤特定长度区间的序列： faops filter
命令filter可以进行长度条件筛选。设置指定的最长和最短通过量，即可完成指定长度区间序列的筛选，并还可以使用参数实现其他一些功能：比如去除序列名重复的序列，简化序列名，过滤掉N含量较高的序列，将IUPAC模糊碱基替换为N，统一转大写，同一序列置于一行等等。
#### 用法
```bash
faops filter [options] <in.fa> <out.fa>

"options"
-a INT     pass sequences at least this big ('a'-smallest)
-z INT     pass sequences this size or smaller 
-n INT     pass sequences with fewer than this number of Ns
-u         Unique, removes duplicated ids, keeping the first
-U         Upper case, converts all sequences to upper cases
-b         pretend to be a blocked fasta file
-N         convert IUPAC ambiguous codes to 'N'
-d         remove dashes '-'
-s         simplify sequence names
-l INT     sequence line length [%d]
```
#### 运行测试（in.fa文件内容与3.2中一致）  
##### 输入
```bash
faops filter -l 60 -a 5 -z 88 -n 4 in.fa out.fa
```
##### 输出
out.fa文件内容
```bash
>test7
gTTTTcttaGNNgCgtccCGAAgcAtCtCTagCCgggGgTAatctccAggtgTgTttGTT
aCCtcCTCGtgACCC
```
