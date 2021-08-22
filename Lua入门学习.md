 Lua入门学习

##### 一、前言

lua 是一种轻量小巧的脚本语言，用标准的C语言编写并以源代码形式开放，其设计目的为了嵌入应用程序中，从而为应用程序提供灵活的扩展和定制功能。

lua可以应用在游戏开发，独立应用脚本，web应用脚本，扩展和数据库插件、安全系统等场景。可以在Web应用（nginx, redis）中使用lua脚本

##### 二、安装

注意：请确保安装 Lua 之前系统已安装 readline 和 readline-devel。如果没有则键入 yum install -y readline readline-devel 进行安装。

```
curl -R -O http://www.lua.org/ftp/lua-5.3.4.tar.gz
tar zxf lua-5.3.4.tar.gz
cd lua-5.3.4
make linux test
```

测试，命令行中键入 lua -v：

```
Lua 5.1.4  Copyright (C) 1994-2008 Lua.org, PUC-Rio
```

##### 三、运行方式

与其他脚本语言一样，我们需要将 Lua 代码编写在文件中，后缀名为 .lua。

运行该文件的代码时，只需在命令行键入 lua xx.lua 即可。

##### 四、语法

###### 4.1 数据类型

Lua 中包含 8 种基本数据类型，即：nil、boolean、number、string、userdata、function、thread 和 table。

| 数据类型 | 说明                                           |
| :------- | ---------------------------------------------- |
| nil      | 表示无效值，在条件表达式中表示 false。         |
| boolean  | 布尔值，包含 true 和 false 两个值。            |
| number   | 表示双精度类型的实浮点数。                     |
| string   | 表示字符串，通过双引号或单引号括住。           |
| userdata | 表示任意存储在变量中的 C 数据结构              |
| function | 表示 C 或 Lua 编写的函数                       |
| thread   | 表示执行的独立线程，用于执行协同程序。         |
| table    | 表示一个关联数组，数组索引可以是数字或字符串。 |

例如：

```
a=10
str="hello world"
```

不需要声明变量类型，我们可以通过 type() 判断变量类型。

**注意：**

Lua 变量有三种类型：全局变量、局部变量和表中的域；

默认情况下，不管在哪声明的变量都是全局变量。通过 local 修饰的变量为局部变量；

变量默认值为 nil。

###### 4.2 运算符

1. 赋值运算符

   ```
   str="hello".."world" -- 通过 .. 连接字符串
   
   a,b=10,15 -- 执行后a=10,b=15
   
   c,d,e=1,2 -- 执行后c=1,b=2,e=nil
   ```

2. 算术运算符

   与其他程序设计语言类似

   ```
   a,b=10,15
   
   c=a+b -- 加号
   
   d=a-b -- 减号
   
   e=a*b -- 乘号
   
   f=a/b -- 除号
   
   g=a%b -- 求余
   
   h=a^2 -- 求乘方
   
   i=-a  -- 负号
   ```

3. 关系运算符

   与其他程序设计语言类似。

```
a,b=10,15

print(a>b) -- 大于

print(a<b) -- 小于

print(a==b) -- 等于

print(a~=b) -- 不等于
```

4.逻辑运算符

```
print(true and false) -- 与

print(true or false)  -- 或

print(not true)       -- 非
```

 5.其他运算符

..    连接两个字符串

 #返回

字符串或表的长度，如#”lua“ ;

###### 4.3流程控制

​    1.条件判断，有3种写法

```
-- if 语句
if(true)
then
    print("hello")
end

-- if .. else 语句
a,b=10,15
if(a>b)
then
    print(a)
else
    print(b)
end

-- if 嵌套
c=20
if(c>10)
then
    if(c<30)
    then
        print(c)
    end
end
```

2.循环，有四种写法

| 类型          | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| while循环     | 在条件为 true 时，让程序重复执行语句。                       |
| for循环       | 重复执行指定语句，重复次数在 for 中控制。可以遍历数字和泛型。 |
| repeat..until | 重复执行循环，直到指定条件为真为止。                         |

```
-- while 循环
a=10
while(a>0)
do
    print(a)
    a=a-1
end

-- for 循环，遍历数字
for a=1,10,1 do
    print(a)
end

-- repeat .. until 循环
a=10
repeat
    print(a)
    a=a-1
until(a<1)
```

###### 4.4数组

Lua 数组大小不固定且下标从 1 开始。

```
arr={"h","e","l","l","o"}

-- 此处使用遍历数字方式
for index=1,#arr do
    print(arr[index])
end

-- 此处使用遍历泛型方式
for i,v in ipairs(arr) do
    print(i,v)
end
```

###### 4.5函数

```
-- 案例 1
function calc(a,b,c)
    return a+b+c
end

result=calc(1,2,3)

print(result)

-- 案例 2 允许返回多个值
function getCalc(a,b,c)
    return a,b,c
end

r1,r2,r3=getCalc(1,2,3)

print(r1,r2,r3)
```

###### 4.6 table

table 是 Lua 中的一个数据结构，类似于 Java 中的 Map 类型或 Javascript 中的 JSON 对象。

Lua table使用关联数组，我们可以使用任意类型值作为数组的索引，但不能是nil

lua table 大小不固定

```
person={}
person.name="jack"
person.age=20

print(person[1])
print(person.name)
print(person["age"])
```

###### 4.7 模块和包

模块类似于一个封装库。从 Lua 5.1 开始，Lua 加入标准的模块管理机制，可以将一些公用的代码放在一个文件中，以 API 接口的形式在其他地方调用，有利于代码的重用和降低代码耦合。

Lua 的模块由变量、函数等已知元素组成的 table 。有点像类的概念

例如 ： 

创建名为 module.lua的文件

```
module={}

module.index=1

function module.sum(a,b)
    return a+b
end
```

另一个文件引入

```
-- 此处 module 是文件名
require "module"

-- 此处 module 是引入模块中定义的名称
print(module.index)
print(module.sum(1,2))
```

