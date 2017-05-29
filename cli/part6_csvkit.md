## csvkit

[csvkit](https://csvkit.readthedocs.io/en/833/)是一个处理CSV文件的一个小工具，基于python，可以直接通过`pip install csvkit`安装。集成了`csvlook`，`csvcut`和`csvsql`等好用的小工具，非常好用。可以以表格形式显示CSV文件，可以轻松选取CSV指定列，可以在其上执行SQL操作

以如下的`student.csv`为例，做简单演示：

| name    | gender | score | grade |
| ------- | ------ | ----- | ----- |
| David   | male   |    85 | B     |
| Michael | female |    90 | A     |
| Cammy   | male   |    88 | A     |
| Tom     | female |    59 | C     |

#### csvlook

csvlook和其他命令行一样，可以直接指定文件作为输入，也可以接受stdin作为输入。对比下效果，直接`cat student.csv`的输出：

<pre>
name,gender,score,grade
David,male,85,B
Michael,female,90,A
Cammy,male,88,A
Tom,female,59,C
</pre>

用csvlook后`cat student.csv | csvlook`的输出：

<pre>
| name    | gender | score | grade |
| ------- | ------ | ----- | ----- |
| David   | male   |    85 | B     |
| Michael | female |    90 | A     |
| Cammy   | male   |    88 | A     |
| Tom     | female |    59 | C     |
</pre>

熟悉Markdown语法的会发现这个就是Markdown里面的表格啊！

#### csvcut

和`cut`作用相似，但对CSV支持更好。比如查看列名，选取列，剔除列等。

+ `csvcut -n student.csv`：输出列名，当列比较多的时候特别有用，结果如下；
+ `head -1 student.csv | tr ',' '\n' | cat -n`：可以达到类似的效果;

<pre>
  1: name
  2: gender
  3: score
  4: grade
</pre>

+ `csvcut -c 1 student.csv`：提取第一列；
+ `csvcut -c name student.csv`：按名字提取列；
+ `csvcut -C 2 student.csv`：提取除去第二列的其他列，如下：


| name    | score | grade |
| ------- | ----- | ----- |
| David   |    85 | B     |
| Michael |    90 | A     |
| Cammy   |    88 | A     |
| Tom     |    59 | C     |


#### csvstat

可以对指定列做简单地统计分析，比如想简单看一下score的统计数据，可以直接提取该列，用pipeline传递给该命令即可：

+ `csvcut -c score student.csv| csvstat`：产生如下结果

<pre>
	Type of data:          Number
	Contains null values:  False
	Unique values:         4
	Smallest value:        59
	Largest value:         90
	Sum:                   322
	Mean:                  80.5
	Median:                86.5
	StDev:                 14.47987108598231501073829222
	Most common values:    85 (1x)
	                       90 (1x)
	                       88 (1x)
	                       59 (1x)

Row count: 4
</pre>

#### csvsql

这是一个比较狠的工具，直接在CSV文件上执行SQL查询，直接传入SQL语句即可：

+ `csvsql --query "select avg(score) from student;" student.csv`：计算score的均值
+ `csvsql --query "select * from student where score > 85;" student.csv`：选出score大于85行，如下：

| name    | gender | score | grade |
| ------- | ------ | ----- | ----- |
| Michael | female |    90 | A     |
| Cammy   | male   |    88 | A     |

