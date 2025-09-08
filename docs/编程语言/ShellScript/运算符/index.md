
# 02- Shell脚本学习—运算符

## Shell运算符

Bash 支持很多运算符，包括算数运算符、关系运算符、布尔运算符、字符串运算符和文件测试运算符。

### 算术运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

`expr` 是一款表达式计算工具，使用它能完成表达式的求值操作。

```shell
# 命令行直接计算expr 2 + 2   
#4expr 3 - 2   
#1expr 3 / 2  
#1expr 3 \* 2   
#6
# 使用表达式
a=10
b=20
val=`expr $a + $b`
echo "a + b : $val"
```

注意：

- **表达式和运算符之间要有空格**，例如 `2+2` 是不对的，必须写成 `2 + 2`，这与我们熟悉的大多数编程语言不一样。
- 乘号(*)前边必须加反斜杠(\\)才能实现乘法运算
- 完整的表达式要被 ` ` 包含，注意这个字符不是常用的单引号，在 Esc 键下边。

###### **算术运算符列表**

| 运算符 | 说明                                          | 举例                        |
| ------ | --------------------------------------------- | --------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30    |
| -      | 减法                                          | `expr $a - $b` 结果为 10    |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200 |
| /      | 除法                                          | `expr $b / $a` 结果为 2     |
| %      | 取余                                          | `expr $b % $a` 结果为 0     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a  |
| ==     | 相等      用于比较两个数字，相同则返回   true | [ $a == $b ] 返回 false     |
| !=     | 不相等  用于比较两个数字，不相同则返回 true   | [ $a != $b ] 返回 true      |

### 关系运算符

关系运算符只支持数字，不支持字符串，除非字符串的值是数字。

```shell
#!/bin/sh
a=10
b=20
if [ $a -eq $b ]then   
	echo "$a -eq $b : a is equal to b"
else   
	echo "$a -eq $b: a is not equal to b"fi
```

缩成一行可以这样：

```shell
a=10;b=20;if [ $a -eq $b ];then echo "$a -eq $b : a is equal to b"; else echo "$a -eq $b: a is not equal to b"; fi
```

这里缩写，主要是为了让大家注意：

- if后面直到then前面的分号结束，都是有空格的: `if [ $a -eq $b ]`

**关系运算符列表**

```shell
运算符              说        明
-eq    检测两个数是否相等，相等返回 true。同算数运算符 `==`
-ne    检测两个数是否相等，不相等返回 true
-gt    检测左边的数是否大于右边的，如果是，则返回 true。
-lt    检测左边的数是否小于右边的，如果是，则返回 true。
-ge    检测左边的数是否大等于右边的，如果是，则返回 true。
-le    检测左边的数是否小于等于右边的，如果是，则返回 true。
```

## 布尔运算符布尔运算符列表**

| 运算符 | 说明                                              |      |
| ------ | ------------------------------------------------- | ---- |
| !      | 非运算，表达式为 true 则返回 false，否则返回 true |      |
| -o     | 或运算(or),有一个表达式为 true 则返回 true        |      |
| -a     | 与运算(and),两个表达式都为 true 才返回 true       |      |

示例1：

```shell
[ ! -f redis.log ] && echo "hello world" || echo "没有此文件"
```

输出：

```shell
没有此文件
```

示例2:或者

```shell
[root@test-script ~]# [  -f redis.log -o ! -f redis.log ]   && echo "hello world"
```

示例3: 与运算

```shell
[root@test-script ~]# [  -f redis.log -a ! -f redis.log ]   && echo "hello world"
```

## 字符串运算符

**字符串运算符列表**

| 运算符 | 说明                                        | 举例                     |
| ------ | ------------------------------------------- | ------------------------ |
| =      | 检测两个字符串是否相等，相等返回 true。     | [ $a = $b ] 返回 false。 |
| !=     | 检测两个字符串是否不相等，不相等返回 true。 | [ $a != $b ] 返回 true   |
| -z     | 检测字符串长度是否为0，为0返回 true。       | [ -z $a ] 返回 false     |
| -n     | 检测字符串长度是否不为0，不为0返回 true。   | [ -n $a ] 返回 true      |
| str    | 检测字符串是否为空，不为空返回 true。       | [ $a ] 返回 true         |

示例1:检测两个字符串是否相等

```shell
[root@test-script ~]# a=user
[root@test-script ~]# b=user
[root@test-script ~]# [ $a = $b ] && echo "相等"
相等
```

示例2:检测两个字符串是否相等，不相等返回 true。

```shell
[root@test-script ~]# [ $a != $b ] && echo "不相等" || echo "相等"
```

示例3:检测字符串长度是否为0

```shell
[root@test-script ~]# [ -z  $a ] && echo "零" || echo "非零"
非零
[root@test-script ~]# [ -z  $a ] && echo "零" || echo "非零"
零
```

示例4:

```shell
[root@test-script ~]# [ $b ] && echo "true" || echo "false"
true
[root@test-script ~]# [ $a ] && echo "true" || echo "false"
false
```

## 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

```shell
#!/bin/sh
file="/tmp/test.sh"
if [ -e $file ]
then   
	echo "File exists"
else   
	echo "File does not exist"
fi
```

文件测试运算符列表

| 操作符 | 说    明                                             | 举       例                                   |
| ------ | ---------------------------------------------------- | --------------------------------------------- |
| -b     | 检测文件是否是块设备文件                             | [ -b $file ] 如果是，返回 true，则返回 false  |
| -c     | 检测文件是否是字符设备文件                           | [ -c $file ] 如果是，返回 true，则返回 false  |
| -d     | 检测文件是否是目录                                   | [ -d $file ] 如果是，返回 true，则返回 false  |
| -f     | 检测文件是否是普通文件（既不是目录，也不是设备文件） | [ -f $file ]如果是，返回 true，则返回 false   |
| -g     | 检测文件是否设置了 SGID 位                           | [ -g $file ]如果是，返回 true，则返回 false   |
| -k     | 检测文件是否设置了粘着位(Sticky Bit)                 | [ -k $file ]如果是，返回 true，则返回 false   |
| -p     | 检测文件是否是具名管道                               | [ -p $file ] 如果是，返回 true，则返回 false  |
| -u     | 检测文件是否设置了 SUID 位                           | [ -u $file ] 如果是，返回 true，则返回 false  |
| -r     | 检测文件是否可读                                     | [ -r $file ]如果是，返回 true，则返回 false   |
| -w     | 检测文件是否可写                                     | [ -w $file ]如果是，返回 true，则返回 false   |
| -x     | 检测文件是否可执行                                   | [ -x $file ]  如果是，返回 true，则返回 false |
| -s     | 检测文件是否为空（文件大小是否大于0）                | [ -s $file ] 如果是，返回 true，则返回 false  |
| -e     | 检测文件（包括目录）是否存在                         | [ -e $file ]如果是，返回 true，则返回 false   |

示例1: 检查文件是否存在

```shell
[root@test-script ~]# [ -f /etc/yum.repos.d/epel.repo ]  &&  echo "true" || echo "flase"
true
[root@test-script ~]# [ ! -f /etc/yum.repos.d/epel.repo ]  &&  echo "true" || echo "flase"
flase
```

# 03- Shell脚本学习—字符串和数组

## 字符串

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。单双引号的区别跟PHP类似：

单双引号的区别：

- 双引号里可以有变量，单引号则原样输出；
- 双引号里可以出现转义字符，单引号则原样输出；
- 单引号字串中不能出现单引号。

### 拼接字符串

```shell
str1='i'
str2='love'
str3='you'
echo $str1 $str2 $str3
echo $str1$str2$str3
echo $str1,$str2,$str3
```

输出：

```shell
i love you
iloveyou
i,love,you
```

### 获取字符串长度

```shell
#!/bin/bash/
str='i love you'
echo ${#str}

# 输出：10
```

### 截取字符串

```shell
#!/bin/bash/
str='i love you'
echo ${str:1} # 从第1个截取到末尾。注意从0开始。
echo ${str:2:2} # 从第2个截取2个。
echo ${str:0} # 全部截取。
echo ${str:-3} # 负数无效，视为0。
```

输出：

```
love youlo
i love you
i love you
```

### 查找字符串

```shell
#!/bin/bash/
str="i love you"
echo `expr index "$str" l`
echo `expr index "$str" love` #最后一个参数是字符，字符串只保留首字母
echo `expr index "$str" o`
echo `expr length "$str"` #字符串长度
echo `expr substr "$str" 1 6` #从字符串中位置1开始截取6个字符。索引是从0开始的。
```

输出:

```
33410i love
```

注意字符串变量需要加双引号。

* 拓展：`expr`更多关于字符串用法：

```shell
STRING : REGEXP   anchored pattern match of REGEXP in STRINGmatch STRING REGEXP        
same as STRING : REGEXPsubstr STRING POS LENGTH   #从STRING中POS位置开始截取LENGTH个字符。POS索引是从1开始的。
index STRING CHARS         #在STRING中查找字符CHARS首次出现的位置，没有找到返回0
length STRING              #字符串长度
```

## 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。类似与C语言，数组元素的下标由0开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于0。

在Shell中，用括号来表示数组，数组元素用`空格`符号分割开。定义数组的一般形式为：

```
array_name=(value1 value2 ... valuen)
```

例如：

```
array_name=(value0 value1 value2 value3)
```

或者

```
array_name=(    value0    value1    value2    value3)
```

还可以单独定义数组的各个分量：

```
array_name[0]=value0
array_name[1]=value1
array_name[2]=value2
```

可以不使用连续的下标，而且下标的范围没有限制。

下面来读取数组：

```shell
echo ${array_name[2]} #读取下标为2的元素
echo ${array_name[*]} #读取所有元素
echo ${array_name[@]} #读取所有元素
echo ${#array_name[*]} #获取数组长度
echo ${#array_name[@]} #获取数组长度
echo ${#array_name[1]} #获取数组中单个元素的长度
```

输出：

```shell
value2
value0 
value1 
value2 
value3
value0 
value1 
value2 
value3446
```

## 总结

对比shell里字符串和数组，我们发现：

字符串

```shell
str="hello"
echo ${#str} # 读取字符串长度
echo ${str} # 读取字符串全部
echo ${str:1} # 截取字符串
```

数组：

```
arr=(a1,a2,a3)
${#str[*]} # 读取数组长度
${#str[1]} # 读取数组某个元素长度

echo ${str[*]} # 读取数组全部
echo ${str[1]} # 读取数组某个元素
`${#ele*}`用来读取ele元素长度属性
`${ele*}`用来读取或操作ele元素
```

# 04- Shell脚本学习—条件控制

标签： Shell

## 条件判断：if语句

语法格式：

```
if [ expression ]
then  
Statement(s) to be executed 

if expression is true

fi
```

注意：`expression` 和方括号([ ])之间必须有空格，否则会有语法错误。

if 语句通过关系运算符判断表达式的真假来决定执行哪个分支。Shell 有三种 if … else 语句：

```
if [ 条件 ];then
		代码块...
fi

if [ 条件 ]；then
			代码块....
else 
		代码块...
fi 


if [ 条件 ];then
elif ... 
else ... 
fi 语句
```

示例：

```
#!/bin/bash/
a=10
b=20
if [ $a == $b ]
then     
	echo "a is equal to b"
elif [ $a -gt $b ]then    
	echo "a is greater to b"
else    
	echo "a is less to b"
fi
```

`if ... else` 语句也可以写成一行，以命令的方式来运行:

```
a=10;b=20;if [ $a == $b ];then echo "a is equal to b";else echo "a is not equal to b";fi;
```

`if ... else` 语句也经常与 `test` 命令结合使用，作用与上面一样：

```
#!/bin/bash/
a=10
b=20
if test $a == $b 
then     
	echo "a is equal to b"
else    
	echo "a is not equal to b"
fi
```

## 分支控制：case语句

您可以使用多个`if...elif `语句来执行多向分支。不过，这并不是最佳解决方案，尤其是当所有分支都取决于一个变量的值时。

Unix shell 支持`case ... esac` 语句，它能准确处理这种情况，而且比重复`if...elif`语句更有效。

> `case ... esac` 与其他语言中的 `switch ... case` 语句类似，是一种多分支选择结构。

语法：

> `case ... esac` 语句的基本语法是给出一个要评估的表达式，并根据表达式的值执行不同的语句。
>
> 解释器会根据不同表达式的值检查每个case，直到找到匹配为止。如果没有匹配项，将使用默认条件。

```shell
case word in
  pattern1)
  语句代码块
  ;;
  pattern1)
  语句代码块
  ;;
  pattern1)
  语句代码块
  ;;
  *)
  默认输出代码块
  ;;
esac
```

示例：

```shell
#!/bin/bash/
grade="B"
case $grade in     
  "A") echo "Very Good!";;    
  "B") echo "Good!";;    
  "C") echo "Come On!";;    
  *)         
  echo "You Must Try!"        
  echo "Sorry!";;
esac
```

转换成C语言是：

```c
#include <stdio.h>
int main(){    
	char grade = 'B';    
	switch(grade){       
    case 'A': printf("Very Good!");
      break;        
    case 'B': printf("Very Good!");
      break;        
    case 'C': printf("Very Good!");
      break;        
    default:             
      printf("You Must Try!");            
      printf("Sorry!");            
      break;    
  }    
  return 0;
}
```

对比看就很容易理解了。很相似，只是格式不一样。

需要注意的是：
**取值后面必须为关键字 in，每一模式必须以右括号结束。取值可以为变量或常数。匹配发现取值符合某一模式后，其间所有命令开始执行直至 `;;`。**`;;` 与其他语言中的 `break` 类似，意思是跳到整个 `case` 语句的最后。

取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。如果无一匹配模式，使用星号 `*` 捕获该值，再执行后面的命令。

再举一个例子：

```shell
#!/bin/bash
option="${1}"
case ${option} in   
	"-f") FILE="${2}"      
			echo "File name is $FILE";;   
	"-d") DIR="${2}"      
			echo "Dir name is $DIR";;   
	*)       
			echo "`basename ${0}`:usage: [-f file] | [-d directory]"      
			exit 1;; # Command to come out of the program with status 1      
esac
```

运行结果：

```shell
$./test.shtest.sh: 
usage: [ -f filename ] | [ -d directory ]
./test.sh -f index.htmlFile name is index.html
```

这里用到了特殊变量`${1}`,指的是获取命令行的第一个参数。

## 循环语句

### 循环语句:  for循环

shell的for循环与c、php等语言不同，同Python很类似。下面是语法格式：

```shell
for 变量 in 列表
do    
  command1    
  command2    
  ...    
  commandN
done
```

示例：

```shell
#!/bin/bash/
for value in 1 2 3 4 5
do    
	echo "The value is $value"
done
```

输出：

```
The value is 1The value is 2The value is 3The value is 4The value is 5
```

顺序输出字符串中的字符：

```shell
for str in 'This is a string'
do    
	echo $str
done
```

运行结果：

```
This is a string
```

遍历目录下的文件：

```shell
#!/bin/bash
for FILE in *
do   
	echo $FILE
done
```

上面的代码将遍历当前目录下所有的文件。在Linux下，可以改为其他目录试试。

遍历文件内容：
city.txt

```shell
beijingtianjinshanghai
#!/bin/bash
citys=`cat city.txt`
for city in $citys 
do
	echo $city
done
```

输出：

```
beijingtianjinshanghai
```

### 循环语句: while循环

只要while后面的条件满足，就一直执行do里面的代码块。

其格式为：

```shell
while command
do   
	Statement(s) to be executed 
if command is true
done
```

命令执行完毕，控制返回循环顶部，从头开始直至测试条件为假。

示例：

```shell
#!/bin/bash
c=0;
while [ $c -lt 3 ]
do    
  echo "Value c is $c"    
  c=`expr $c + 1`
done
```

输出：

```
Value c is 0Value c is 1Value c is 2
```

这里由于shell本身不支持算数运算，所以使用`expr`命令进行自增。

### 循环语句: until循环

until 循环执行一系列命令直至条件为 true 时停止。until 循环与 while 循环在处理方式上刚好相反。一般while循环优于until循环，但在某些时候，也只是极少数情况下，until 循环更加有用。

将上面while循环的例子改改，就能达到一样的效果：

```shell
#!/bin/bash
c=0;
until [ $c -eq 3 ]
do    
  echo "Value c is $c"    
  c=`expr $c + 1`
done
```

首先do里面的语句块一直在运行，直到满足了until的条件就停止。

输出：

```
Value c is 0Value c is 1Value c is 2
```

## 循环控制语句

在循环过程中，有时候需要在未达到循环结束条件时强制跳出循环，像大多数编程语言一样，Shell也使用 break 和 continue 来跳出循环。

### 循环控制：break

break命令允许跳出所有循环（终止执行后面的所有循环）。

```shell
#!/bin/bash
i=0
while [ $i -lt 5 ]
do   
	i=`expr $i + 1`    
	if [ $i == 3 ];then            
  break    
  fi    
	echo -e $i
done
```

运行结果：

```
1
2
```

在嵌套循环中，break 命令后面还可以跟一个整数，表示跳出第几层循环。例如：

```
break n
```

表示跳出第 n 层循环。

### 循环控制：continue

continue命令与break命令类似，只有一点差别，它不会跳出所有循环，仅仅跳出当前循环。

```shell
 复制代码
 #!/bin/bash
 i=0
 while [ $i -lt 5 ]
 do    
 		i=`expr $i + 1`    
 		if [ $i == 3 ];then            
 				continue    
 		fi    
 			echo -e $i
 done
```

运行结果：

```
1
2
4
5
```

# 05- Shell脚本学习— IO重定向

> Shell 输入/输出重定向。大多数 Unix 系统命令从终端接收输入，并将输出结果发送回终端。命令通常从标准输入（默认情况下就是终端）读取输入。同样，命令通常会将输出写入标准输出，而标准输出默认也是你的终端。

## 5.1 输出重定向

<p>命令的输出通常会被重定向到文件中，而不是标准输出。这种能力被称为输出重定向。
如果将 `>` 文件 的符号附加到任何通常将其输出写入标准输出的命令后面，该命令的输出将被写入文件而不是终端。</p>
<strong>示例</strong>

<p>检查以下的 who 命令，它将命令的完整输出重定向到 users 文件中。</p>

```shell
#  who > users
```

<p>请注意，终端上不会显示任何输出。这是因为输出已被重定向从默认标准输出设备（终端）到指定的文件。您可以检查用户文件以获取完整内容。</p>

```shell
```



# 06- Shell脚本学习—函数

函数可以让我们将一个复杂功能划分成若干模块，让程序结构更加清晰，代码重复利用率更高。像其他编程语言一样，Shell 也支持函数。Shell 函数必须先定义后使用。

## 函数定义

Shell 函数的定义格式如下：

```shell
function(关键字) function_name（函数的名称） () {    
list of commands    [ return value ]
函数体
}
```

其中`function`关键字是可选的。

```shell
#!/bin/bash
hello(){    
	echo 'hello';
}

hello
```

运行结果：

```
hello
```

调用函数只需要给出函数名，不需要加括号。

函数返回值，可以显式增加return语句；如果不加，会将最后一条命令运行结果作为返回值。

Shell 函数返回值只能是整数，一般用来表示函数执行成功与否，0表示成功，其他值表示失败。如果 return 其他数据，比如一个字符串，往往会得到错误提示：`numeric argument required`。

```shell
#!/bin/bash
function hello(){    
			return 'hello';
}

hello
```

运行结果：

```shell
line 4: return: hello: numeric argument required
```

如果一定要让函数返回字符串，那么可以先定义一个变量，用来接收函数的计算结果，脚本在需要的时候访问这个变量来获得函数返回值。

```shell
#!/bin/bash
function hello(){    
return 'hello';
}
str=hello
echo $str
```

运行结果：

```
hello
```

像删除变量一样，删除函数也可以使用 `unset` 命令，不过要加上 `.f` 选项，如下所示：

```shell
$unset .f function_name
```

如果你希望直接从终端调用函数，可以将函数定义在主目录下的 .profile 文件，这样每次登录后，在命令提示符后面输入函数名字就可以立即调用。

## 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 `$n` 的形式来获取参数的值，例如，`$1`表示第一个参数，`$2`表示第二个参数…这就是前面讲的特殊变量。

```shell
#!/bin/bash
function sum(){
		case $# in         
				 0) echo "no param";;        
				 1) echo $1;;        
				 2) echo `expr $1 + $2`;;        
				 3) echo `expr $1 + $2 + $3`;;        
				 *) echo "$# params! It's too much!";;    
	 esac
}
sum 1 3 5 6
```

运行结果：

```shell
4 params! It's too much!
```

注意，`$10` 不能获取第十个参数，获取第十个参数需要`${10}`。当`n>=10`时，需要使用`${n}`来获取参数。

另外，还有几个特殊变量用来处理参数，前面已经提到：

| 特殊变量 | 说明                                        |
| -------- | ------------------------------------------- |
| $#       | 传递给函数的参数个数                        |
| $*       | 显示所有传递给函数的参数。                  |
| $@       | 与$*相同，但是略有区别，请查看Shell特殊变量 |
| $?       | 函数的返回值。                              |

