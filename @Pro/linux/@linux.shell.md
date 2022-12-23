Shell
[toc]

# 简介
Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。
Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。
shell脚本默认拓展名.sh
shell脚本运行需要解释器，常见如下：
- Bourne Shell（/usr/bin/sh或/bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）

以下实例使用 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。
在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 #!/bin/sh，它同样也可以改为 #!/bin/bash。
[#!] 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

# 运行方法
- 作为可执行程序
#!/bin/bash 脚本第一行设置解释器
chmod +x ./my.sh 设置脚本具有执行权限
./my.sh 执行脚本
- 作为解释器参数
/bin/sh my.sh
直接运行解释器，其参数是shell脚本文件名，不需要在脚本第一行指定解释器，指定也无效

# 输入输出
- read a 读取输入变量 a
- echo $a 输出变量 a
- echo "this is echo"
- echo "\"this is echo\"" 显示转义字符
- echo -e "first line \n second line" 开启转义
- echo `date` 显示linux命令执行结果

# shell 变量
fn="my.sh" 变量名和等号之间不能有空格
类型有数字、字符串、数组
字符串可以用双引号、单引号、或不用引号，推荐使用双引号

## 使用变量 变量前加 $
echo ${fn} 推荐使用 方便解释器识别变量边界
echo $fn
变量赋值不需要加 $

## 只读变量 readonly + 变量
readonyly fn="my.sh"
或者分开
fn="my.sh"
readonly fn
只读变量的值不能被改变

## 删除变量 unset + 变量
fn="my.sh"
unset fn
echo ${fn}
将不会输入任何字符

## shell字符串
- 双引号 推荐使用
  fn="my.sh"
  fn1="文件名为：\"${fn}\"\n123"
  双引号中可以有变量
  双引号中可以有转义字符
  echo -e 输出转义字符 
  不加 \"会输出"，\n \t等其他会原样输出
- 单引号
  fn='my.sh'
  fn='123''就是'
  单引号中的任何字符都会原样输出，其中的变量无效
  单引号中不能出现单独的一个单引号，对单引号使用转义字符也是不行的，但可以成对出现，作为拼接字符串使用

### 获取字符串长度
fn="my.sh"
echo ${#fn} 输出5
当变量为数组时 ${#fn} 等价于 ${#fn[0]}

### 提取子字符串
fn="my.sh"
echo ${fn:1:2} 从第2个字符开始截取2个 输出 y.
第一个索引为：0

### 查找子字符串
fn="my.sh"
echo `expr index "${fn}" ys' 
查找 y 或 s 的位置(哪个先出现就计算哪个)  输出 2, 输出的索引从 1 开始

## shell数组
数组下标从 0 开始
fns=(1 2 3 4 5) #定义数组
echo ${fns[4]} #读取某个位置的值
fns[1]=100  #赋值某个位置的值
echo ${fns[@]} 或 echo ${fns[*]}  #读取数组所有元素
echo ${#fns[*]} #获取数组元素个数

# shell运算符
a=10
b=4
echo "a + b = `expr $a + $b`"
echo "a - b = `expr $a - $b`"
echo "a * b = `expr $a \* $b`"
echo "a / b = `expr $a / $b`"
echo "a % b = `expr $a % $b`"

# shell流程控制
## 条件判断
- [ 条件 ]
  前后必须有空格
  [ $a -gt $b ]  a>b
  不可以直接使用 > = < >= <=
  - -gt >
  - -lt <
  - -eq ==
  - ge >=
  - le <=
- (( 条件 ))
  前后必须有空格
  (( $a>$b )) 可以直接使用 > = < >= <=

## if else
a=10
b=4
if [ $a -gt $b ] #大于
then
    echo "a > b"
elif [ $a -lt $b ] #小于
then
    echo "a < b"
elif [ $a -eq $b ]
then
    echo "a = b"
else
    echo "null"
fi

## for循环
for item in 1 2 4 10 3
do
  echo $item
  if [ $item -ge 10 ]
  then
      echo ">="
      break
  else
      echo "<"
      continue
  fi
done

strs=("this" "is" "my" "shell")
for item in ${strs[*]}
do
  echo $item
done

## while循环 不能使用[]
a=10
while(( $a >= 5 ))
do
  echo $a
  a=`expr $a - 1`
done

## until循环 直到条件=true时停止 与while相反
a=10
until(( $a <= 5 ))
do
  echo $a
  a=`expr $a - 1`
done

## case 语句
read a
case $a in
 1) echo "李白";;
 2) echo "苏轼";;
 *) echo "输错了";;
esac

# shell函数
[注意]：shell 语言中 0 代表 true，0 以外的值代表 false。
fun1(){
  echo "无返回值"
}
fun2(){
  echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
  return $1
}

fun1

fun2 1 2 3 4 5 6 7 8 9 10 11
echo $?

- $10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。
- $#	传递到脚本或函数的参数个数
- $*	以一个单字符串显示所有向脚本传递的参数
- $$	脚本运行的当前进程ID号
- $!	后台运行的最后一个进程的ID号
- $@	与$*相同，但是使用时加引号，并在引号中返回每个参数。
- $-	显示Shell使用的当前选项，与set命令功能相同。
- $?	函数返回值

