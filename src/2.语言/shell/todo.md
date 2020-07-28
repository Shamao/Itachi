```shell
a=1
b=1
echo $a $b
echo `expr $a + $b`
echo $((a + b))
echo $[a+b]

```



#### read 读取单行数据

Linux read命令用于从标准输入读取数值。read 内部命令被用来从标准输入读取单行数据。这个命令可以用来读取键盘输入，当使用重定向的时候，可以读取文件中的一行数据。



#### let: 执行一个或多个表达式

let命令是bash中用于计算的工具，用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量。如果表达式中包含了空格或其他特殊字符，则必须引起来。



#### 关系运算

| 运算符 | 说明                                                  | 举例                       |
| :----- | :---------------------------------------------------- | :------------------------- |
| -eq    | 检测两个数是否相等，相等返回 true。                   | [ $a -eq $b ] 返回 false。 |
| -ne    | 检测两个数是否不相等，不相等返回 true。               | [ $a -ne $b ] 返回 true。  |
| -gt    | 检测左边的数是否大于右边的，如果是，则返回 true。     | [ $a -gt $b ] 返回 false。 |
| -lt    | 检测左边的数是否小于右边的，如果是，则返回 true。     | [ $a -lt $b ] 返回 true。  |
| -ge    | 检测左边的数是否大于等于右边的，如果是，则返回 true。 | [ $a -ge $b ] 返回 false。 |
| -le    | 检测左边的数是否小于等于右边的，如果是，则返回 true。 | [ $a -le $b ] 返回 true。  |



#### shell将输出结果赋值到变量

```shell
var=`command`

var=$(command)
```





在shell中引号分为三种：单引号，双引号和反引号。

\* 单引号 ‘

由单引号括起来的字符都作为普通字符出现。特殊字符用单引号括起来以后，也会失去原有意义，而只作为普通字符解释。例如：

```
$ string=’$PATH’
$ echo $string
$PATH
```

可见$保持了其本身的含义，作为普通字符出现。

```
howard@0[script]$ grep Susan phonebook 
Susan Goldberg 403-212-4921 
Susan Topple    212-234-2343
```

如果我们想查找的是Susan Goldberg，不能直接使用grep Susan Goldberg phonebook命令，grep会把Goldberg和phonebook当作需要搜索的文件

```
howard@0[script]$ grep 'Susan Gold' phonebook 
Susan Goldberg 403-212-4921
```

当shell碰到第一个单引号时，它忽略掉其后直到右引号的所有特殊字符。

\* 双引号 “

由双引号括起来的字符，除$、倒引号(`)和反斜线（\）仍保留其特殊功能外，其余字符均作为普通字符对待。“$”表示变量替换，即用其后指定的变 量的值来代替$和变量；倒引号表示命令替换；仅当“\”后面的字符是下述字符之一时，“\”才是转义字符，这些字符是：“$”、“`”、“””、“\”或 换行符。转义字符告诉Shell不要对其后面的那个字符进行特殊处理，只是当作普通字符。

```
$ echo "My current dir is `pwd` and logname is $LOGNAME"
My current dir is /home/mengqc and logname is mengqc
```

\* 反引号 `

反引号（`）这个字符所对应的键一般位于键盘的左上角，不要将其同单引号（’）混淆。反引号括起来的字符串被shell解释为命令行，在执行时，shell首先执行该命令行，并以它的标准输出结果取代整个反引号（包括两个反引号）部分。例如：

```shell
$ pwd
/home/xyz
 
$ string=”current directory is `pwd`”
$ echo $string
current directour is /home/xyz
```

shell执行echo命令时，首先执行`pwd`中的命令pwd，并将输出结果/home/xyz取代`pwd`这部分，最后输出替换后的整个结果。

利用反引号的这种功能可以进行命令置换，即把反引号括起来的执行结果赋值给指定变量。例如：

```shell
$ today=`date`
$ echo Today is $today
Today is Mon Apr 15 16:20:13 CST 1999
```

对于bash的来说，命令的解释是从左到右的，首先遇到单引号和双引号，所作的解释是不一样，看这个例子：

```shell
$ a=1
$ echo "$a"
1               #$被bash解释到
$ echo "'$a'"
'1'             #单引号失效
$ echo '$a'
$a            
$ echo '"$a"'
"$a"            #双引号失效
```