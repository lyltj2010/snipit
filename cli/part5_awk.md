## awk

一个强大的工具，可以同时处理行和列，好多C语言内置函数可以集成进来，非常灵活。基本模式是`awk 'BEGIN{print "start"} pattern {commands} END {print "end"} file'`，其中BEGIN和END可选，就是开始执行真正的循环之前和之后执行的操作。  

#### 简介

有几个特殊的变量：

+ `NR`：number of current row，当前行号；
+ `NF`：number of fields，总共有多少个字段，默认是按空格分字段的；
+ `$0`：当前行段内容；
+ `$1`：第一个字段的内容

执行逻辑是：

+ 执行BEGIN块里命令
+ 读取一行内容（文件或stdin），匹配模式，若匹配成功，执行commands；匹配不成功，不执行；如果没有模式，默认都执行；重复这一步
+ 执行END块里命令

下面还是以student.csv为例:

| name    | gender | score | grade |
| ------- | ------ | ----- | ----- |
| David   | male   |    85 | B     |
| Michael | female |    90 | A     |
| Cammy   | male   |    88 | A     |
| Tom     | female |    59 | C     |


#### Cookbook

简单常见操作：

+ `awk '{print $1}' student.csv`：打印第一个字段，默认空格分割
+ `awk '/Tom/ {print $2}' student.csv`：若该行包含Tom，打印第二列，默认空格分割
+ `awk -F ',' '{print $NF}' student.csv`：打印最后一列，指定是按逗号分隔
+ `awk '{s+=$3} END {print s}' student.csv`：计算第三列的和，如果没有表头的话
+ `awk 'BEGIN {getline; print $0} {s+=$3} END {print s}' student.csv`：getline跳过第一行，尤其是CSV文件
+ `awk 'END{print NR}' student.csv`：统计有几行

##### 计算一列和

`awk -F"," 'BEGIN {getline} {s+=$3} END {print s}' student.csv`结果算出score列和为322。  

其中`-F","`告诉awk用逗号分隔；BEGIN里的get line告诉awk跳过第一行；后面每次循环加上第三列的值，结果就是求个sum。

#### 计算某列最大值

`awk -F"," 'BEGIN{getline} max < $3 {max = $3} END{print max}' student.csv`得出结果90。  

同样开始的时候，跳过第一行；`max < $3`是一个条件判断，如果遇到更大的值，将其赋给max，如果没有，继续；最后打印最大值。

`awk -F"," 'BEGIN{getline} max < $3 {max = $3; maxline=$0} END{print maxline }' student.csv`可以打印最大值这一行。

##### 交换两列值

`awk -F"," 'BEGIN{OFS=","} {tmp=$3; $3=$4; $4=tmp; print $0}' student.csv`结果如下：

| name    | gender | grade | score |
| ------- | ------ | ----- | ----- |
| David   | male   | B     |    85 |
| Michael | female | A     |    90 |
| Cammy   | male   | A     |    88 |
| Tom     | female | C     |    59 |

其中BEGIN模块里先指定Output Field Separator，默认的是空格，可以重新指定为逗号；后面建立一个临时变量，然后交换第三四列；打印交换后的行。

##### 给加一个列id

`awk 'BEGIN {getline; print "id," $0} {print NR-1 "," $0}' student.csv`

| id | name    | gender | score | grade |
| --- | ------- | ------ | ----- | ----- |
|  1 | David   | male   |    85 | B     |
|  2 | Michael | female |    90 | A     |
|  3 | Cammy   | male   |    88 | A     |
|  4 | Tom     | female |    59 | C     |

第一行的时候，直接加id即可；其他行，利用`NR`变量自动加，同时用变量`$0`保留原来行数据。
