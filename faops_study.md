# faops初尝试
## faops help

```
faops help
```
输出
```
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
## faops count
count base statistics in FA file(s)_统计序列总长和碱基组成
```dotnetcli
faops count <in.fa> [more_files.fa] 
```
运行测试
```dotnetcli
faops count in.fa
```
输出
```dotnetcli
#seq    len     A       C       G       T       N
hg38_1  64      11      11      26      7       9
hg38_2  67      8       25      26      8       0
hg38_3  71      33      7       13      18      0
total   202     52      43      65      33      9
```
## faops size
count total bases in FA file(s)_统计序列长度  
```dotnetcli
faops size <in.fa> [more_files.fa]
```
运行测试  
```dotnetcli
faops size in.fa
```
输出
```dotnetcli
hg38_1  66
hg38_2  72
hg38_3  71
```
