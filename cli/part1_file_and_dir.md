## 文件和目录操作

#### 改变目录

+ `cd path/to/dir`：到指定目录
+ `cd ..`：到父目录
+ `cd -`：到上次所在目录
+ `cd`：到home目录
+ `cd ~/path/to/dir`：到home目录下指定文件夹
+ `cd /path/to/dir`：到root目录下指定文件夹

#### 文件操作

+ `touch test.txt`：新建文件`test.txt`
+ `rm test.txt`：删除文件`text.txt`
+ `cp /path/to/original /path/to/copy`：复制文件
+ `cp -r /path/to/original /path/to/copy`：递归地复制文件夹
+ `mv /path/to/source /path/to/target`：移动文件或文件夹

#### 目录操作

+ `mkdir tmp`：新建目录`tmp`
+ `rmdir tmp`：删除空目录`tmp`
+ `mkdir -p tmp/nested/dir`：递归式新建目录，创建嵌套目录时特别有用
+ `rm -r tmp`：以递归形式删除非空目录`tmp`
+ `rm -rf tmp`：强制删除非空目录`tmp`，无须确认

#### 查看目录

+ `ls`：列出当前目录下文件、目录
+ `pwd`：查看当前所在工作目录
+ `tree`：以树形结构显示当前目录
+ `ls -1`：list files，每个一行（是1，不是小写的L）
+ `ls -l`：long format list，显示权限等信息
+ `ls -a`：同时显示隐藏文件
+ `ls -lh`：同时显示文件大小，以human readable的单位（KB，MB，GB等）
+ `ls -lS`：按文件大小降序排列
+ `ls -ltr`：按修改时间逆序排列
+ `tree -L num`：限制显示到指定层级（当前层级是1）
+ `tree -d`：只显示目录
+ `tree -a`：隐藏文件也显示