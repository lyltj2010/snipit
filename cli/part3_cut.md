## 列操作

面对较大CSV文件的时候，可以用列工具做简单操作。

以如下的一个`student.csv`为例子：

| name    | gender | score | grade |
| ------- | ------ | ----- | ----- |
| David   | male   |    85 | B     |
| Michael | female |    90 | A     |
| Cammy   | male   |    88 | A     |
| Tom     | female |    59 | C     |

#### 甄选列`cut`

CSV有很多列，可以用cut挑选出指定列。这里有几个有用的参数：

+ `-d`：field delimiter，字段分隔符；
+ `-f`：fields，指定字段；

常用操作：

+ `cut -d',' -f1 filename`：提取第一列，当`,`为字段分隔符时
+ `cut -d',' -f1,3`：提取第一个和第三个字段，当`,`为字段分割符时
+ `cut -d':' -f2-4`：提取第二到第四个字段，当`:`为字段分割符时
+ `cut -d',' -f3 --complement student.csv`：提取除第三列的其他列，当`,`为分隔符时

比如`cut -d"," -f1,3 student.csv`产生如下结果：

| name    | score |
| ------- | ----- |
| David   |    85 |
| Michael |    90 |
| Cammy   |    88 |
| Tom     |    59 |


