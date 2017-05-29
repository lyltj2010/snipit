## `sed`

#### 简介

sed表示stream editor，流式编辑，把文件按行读进来，做处理。做替换处理text replacement
，或者删除操作的时候特别有用。  

基本的命令模式`sed [options] commands [file-to-edit]`。  

其中`commands`是传给sed的命令，也是最核心的；  

`commands`的模式为`[addr]X[options]`，其中`addr`指定是对哪些行做操作，比如第1行，或者3-100行，也可以通过正则表达式确定；其中`X`是一个字符的sed命令，常见的有`p`打印，`d`删除，`s`替换等；`[options]表示不同命令所需要的参数`，比如替换操作时`g`表示全局替换；  

`[file-to-edit]`是需要处理的文件，当然sed也可以接受`stdin`作为输入。

#### Cookbook

sed涉及的参数太多了，直接用一个个case比较好解释。  


**打印行**的操作：  

sed默认会对匹配到的行做echo操作，所以默认是有print操作的，可以用参数`-n`抑制默认的打印操作，一般`-n`和`p`放在一起使用。

+ `sed '' filename`：和`cat`一个效果；
+ `sed -n '1p' filename`：打印第一行；
+ `sed -n '10,20p' filename`：打印10-20行；
+ `sed -n '10,+10p' filename`：打印从第10行开始的10行，注意有的版本的sed不支持；

**删除行**的操作： 

+ `sed '1d' filename`：删除第一行，当我们不需要CSV的header时候很实用；
+ `sed -i '1d' filename`：删除文件第一行，in-place模式，也就是直接修改文件，比较危险；
+ `sed -i.bak '1d' filename`：删除文件第一行，in-place模式，但会先创建一个`filename.bak`文件；
+ `sed '2,10d' filename`：删除第2-10行，`2,10`指定一个区间range；
+ `sed /^$/d filename`：删除空行，这里是用正则表达式锁定操作的区间的，也就是匹配到空行才执行操作；
+ `sed /^foo/d filename`：删除以`foo`开头的行；
+ `sed /ERROR/!d filename`：删除**不**包含`ERROR`的行，其中`!`作用是negate the range，对不包含在指定range里的行操作；

**替换行**的操作：

`s`表示substitute，也是sed最强大的命令。基本模式就是`sed 's/regex/replacement/' filename`，其中`s`表示替换，注意`/`需要三个，一个都不能少哦，也可以用其他字符统一替换，比如`:`，`sed 's:regex:replacement:' filename`同样有效；`s`前面也可以指定range，限定要替换的范围，不指定的话对所有行操作。

+ `sed 's/this/This/' filename`：把this替换为This，只替换第一个匹配的this；
+ `sed 's/this/This/g' filename`：global模式，把所有的this替换为This；
+ `sed 's/this/This/2 filename`：替换第二个this为This；注意这里指的是当前行匹配到的第二个this；

`echo "thisthisthis" | sed 's/this/This/2'`会输出`thisThisthis`。

+ `sed -n 's/this/This/2p' filename`：会打印发生替换的行；
+ `sed 's/this/This/i filename'`：查找的时候忽略大小写；
+ `sed -e 's/this/This/' -e 's/that/That/' filename`：整合多条sed命令；

#### Reference

+ [The Basics of Using the Sed Stream Editor to Manipulate Text in Linux](https://www.digitalocean.com/community/tutorials/the-basics-of-using-the-sed-stream-editor-to-manipulate-text-in-linux)
+ [sed, a stream editor](https://www.gnu.org/software/sed/manual/sed.html#sed-script-overview)