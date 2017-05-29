## 文件初探

有时候文件比较大，想简单查看下文件内容，可以用head，tail或者less，部分查看文件。

#### 看前N行`head`

+ `head filename`：查看文件头
+ `head -n filename`：查看文件前n行

#### 看后N行`tail`

+ `tail filename`：查看文件结尾
+ `tail -n filename`：查看文件后n行

#### 交互式查看文件`less`

+ `less filename`：交互式查看文件，比如日志文件等
+ `<Space> `：下一页
+ `b`：上一页
+ `g`：跳到文件开头
+ `G`：跳到文件结尾
+ `q`：退出
+ `/query_string`：前向查找字符串
+ `?query_string`：反向查找字符串

#### 统计行数

Word Count统计文件行数。

+ `wc -l filename`：统计文件有多少行
+ `wc -w filename`：统计文件有多少个单词
+  `wc -c filename`：统计有多少个字符