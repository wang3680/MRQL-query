MRQL-query
==========

mrql初级教程-概念、使用（一）

以下是本人原创，如若转载和使用请注明转载地址。本博客信息切勿用于商业，可以个人使用，若喜欢我的博客，请关注我，谢谢！博客地址

感谢您支持我的博客，我的动力是您的支持和关注！如若转载和使用请注明转载地址，并且请尊重劳动成果，谢谢！

MRQL简介

MRQL (发音 miracle) 是一个查询处理和优化系统，适用于大规模分布式的数据分析。MRQL  (MapReduce Query Language) 是一个在计算机集群中对大规模数据的类 SQL 查询语言。MRQL 查询处理系统可使用如下三种模式评估 MRQL 查询：

使用 Hadoop 的 Map-Reduce 模式
使用 Apache Hama 的 BSP 模式 (Bulk Synchronous Parallel mode)
基于 Apache Spark 的 Spark 模式

MRQL一般的使用语法

Evaluating MRQL Queries Using Map-Reduce

Before deploying your MRQL queries on a Hadoop cluster, you can run these queries in memory on a small amount of data using the command:

mrql    
//在Hadoop集群部署MRQL查询之前,您可以运行这些查询对少量的数据在内存中使用命令:
which evaluates MRQL top-level commands and queries from the input until you type quit. To run MRQL in Hadoop's standalone mode (single node on local files), use:

mrql -local
//该评估MRQL顶级命令和查询从输入类型,直到你放弃。MRQL运行Hadoop的独立模式(单一节点本地文件)
To run MRQL in Hadoop's fully distributed mode (cluster mode), use:

mrql -dist
//MRQL运行Hadoop的完全分布式模式(集群模式)
Accessing the Data Sources

The MRQL expression that makes a directory of raw files accessible to a query is:

source(parser,path,...args)
where path is the URI of the directory that contains the source files (a string), parser is the name of the parser to parse the files, and args are various parameters specific to the parsing method. It returns a !bag(t), for some t, that is, it returns a map-reduce type. Currently, there are four supported parsers: line, xml, json, and binary, but it is easy to define and embed your own parser (explained later).

Parsing XML Documents

The MRQL expression used for parsing an XML document is:

source( xml, path, tags, xpath )

For example, the following expression:

XMark = source(xml,"xmark.xml",{"person"});
binds the variable XMark to the result of parsing the document "xmark.xml" and returns a list of person elements. A more complex example is:

DBLP = source( xml, "dblp.xml", {"article","incollection","book","inproceedings"},
               xpath(.[year=2009]/title) )
下面是我自己做的例子：
说明：MRQL是在hadoop运行的情况下，完成工作的，而且我采用的是MRQL的第一种模式执行的，下面就MapReduce模式进行说明。

我定义一个xml文件叫1.xml
1
2
3
4
5
6
7
8
<person>
    <name>
        张三
    </name>
    <age>
        20
    </age>
</person>
将1.xml文件上传到hdfs目录下
hadoop fs -put ~/1.xml /user/hadoop/jl

查看jl目录

 

启动MRQL -dist 及执行source
source(xml,'hdfs://183.175.12.220:9010/user/hadoop/jl/1.xml',{"person"});
 
执行成功！
定义变量v用于存储
store v := source(xml,'hdfs://183.175.12.220:9010/user/hadoop/jl/1.xml',{"person"});
 
 
将得出的v进行dump输出，输出至一个txt文件中
dump 'hdfs://183.175.12.220:9010/user/hadoop/jl/a.txt' from v;


执行成功，现在可以去hdfs中查看此文件了






