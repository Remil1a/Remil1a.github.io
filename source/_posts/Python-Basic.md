---
title: Python_Basic
date: 2018-08-09 16:01:55
categories: Python
tags: Python
mathjax: true
---

## Python为何这么火

Python的定位非常明确，它是一种**简单易用**但又**专业严谨**的语言。或者说叫胶水语言。普通人也很容易入门。Python可以把各个基本程序拼接在一起协同运作。任何一个人只要愿意学习，可以在几天的时间内学会Python的基础部分。然后做很多很多事情，这种投入产出比是任何其他语言无法具备的。

<!----more---->



打个比方 我们都知道C语言和C++ Java这种语言比较常用，那么如果我们以造一辆车做比喻，C是从头开始造。先造发动机，再造挡风玻璃，车门，雨刮，轮胎。甚至连一颗螺丝钉都是完全为这辆车打造的。最后造出来的是一辆独一无二的车。这个开发周期可能要一到两年。



而如果使用Python造一辆车，轮子不用自己造，有很多轮子别人已经帮你写好了。这些轮子可能是拿C语言写的。你只需要选一个适合你的。同理，底盘，挡风玻璃等都可以选别人造好的。这样造车，周期会非常短。可能只需要一个星期就能跑起来。当然，这辆车肯定相对来说没有C语言造出来的车要好。毕竟人家是定制的。我这个是拼接的。但是如果不合适我可以把轮子直接换一个。非常灵活。



如果有人说，Python开发的程序没有C开发的程序跑得快，执行效率低。我想说的是，很多Python的模块就是拿C写的。打个比方，Python做的事情就是指挥这些模块去做事。是个指挥者的角色。我们做的很多时候都是逻辑。而不是方法。例如，一个数据来了 交给某个Python模块，这个模块是拿C写的，然后返回给你结果，你再拿到这个结果给另一个模块处理。很少有纯Python的程序。执行效率当然没有纯C写出来的程序高。但是也差不了多少。



## Python内置类型

Python的程序分为**模块** **语句** **表达式** 和**对象**

- 程序由模块组成
- 模块包含语句，条件，循环....等等
- 语句包含表达式
- 表达式创建和处理对象。

  为什么使用内置对象呢，内置对象是程序自带的，本身就支持的东西。比方说数字，字符，列表等等。并且可以基于内建对象创建新对象。

那么内置对象有哪些呢？下面列出了常用的内置对象。

| 对象类型             | 例子                                               |
| -------------------- | -------------------------------------------------- |
| Numbers（数字）      | 1234，3.1415926，0b111，3+4j                       |
| Strings（字符串）    | 'remilia'，‘ipconfig’，'\r\n'                      |
| Lists（列表）        | [1,2,[12,3]],[1,'1','Playstation'],list[range(10)] |
| Dictionaries（字典） | {'food':'Hamburg','PS4':'Playstation4','1':123}    |
| Tuples（元组）       | (1,'test',4,6)                                     |
| Files（文件）        | open('dns.txt'),open(r'D:\Learn\ccna.ppt')         |
| Sets（集合）         | set('abc'),{'a','b','c'}                           |

大部分都是我们熟悉的。接下来看第一个 数字。这个是我们生活中也会用到的。Python中处理的每个东西都是对象。所以这些很关键。



Python的安装就不去说了。Windows版在官网上的都能找到。MacOS和Linux都能在官网上找到。安装过程网上都有教程。

## Python内置类型-数字

首先每个对象我都会用以下这张表来概述特点。这些特点需要牢记。数字类型的特点如下：

| 类型           | 分类            | 可变   |
| -------------- | --------------- | ------ |
| Number（数字） | Numeric（数值） | 不可变 |

那么来解释一下吧

- 类型：就是对象的类型 在内置类型里列出了常用的类型。现在说明的是数字。
- 分类：或者说特征，数字类型的特点就是里面只能放数值。常见的数值有整数，浮点数和虚数等。
- 可变：对象一旦定义就是不可变的。比方说定义了A=999 那么除非重新再赋值A=666 不然是没办法改变其中的值的。这个可以跟下面要讲的字符串做对比。先这么记着。

接下来来看一下数字类型的对象能够接受的值吧。

- 1234，-24，0，999999   这些都是**整数**。
- 1.23，1.5666，3.1415926   4E210 这些都是**浮点数**。
- 0o177，0x12a，ob1010101011   这些分别是**八进制**，**十六进制**和**二进制数**。
- 3+4j，3.0+4j 这些都是**复数**
- set('test')，{1,2,3,4} 这些是集合
- Decimal('1.0')，Fraction(1,3) 小数和分数 右边那个是$\frac{1}{3}$
- bool(X),True,False 布尔和常量 布尔值就像 真，假这种。

那么下面列出一些数字对象的基本操作

- 表达式操作符

+(加)，-(减)，\*(乘)，/(除)，\*\*(乘方)，>> 移位



这里要说明一下移位。移位是指将数字往前或者往后移动一位。例如算IP地址的时候经常会记下面的一串数字



64 32 16 8 4 2 1



那么例如8往后移动2位应该是2



下面来实际操作一下

```python
>>> 3+2
5
>>> 3**2
9
>>> 6*5
30
>>> 8-2
6
>>> 8-9
-1
>>> 8 >> 2
2
>>> 8 << 2
32
```



### 内建数学函数

内建数学函数可以理解为Python自带的数学函数例如

pow()乘方，abs()求绝对值，round()四舍五入，int, hex,oct,bin等等

那么我们也通过实际操作感受一下

```python
>>> pow(4,2) \\四的二次方
16
>>> abs(-100) \\求100的绝对值
100
>>> round(4.6) \\四舍五入4.6
5
>>> round(4.4) \\四舍五入4.4
4
>>> round(4.5) \\四舍五入4.5
4
>>> int(0b01101) \\将0b01101(二进制01101)转换成int(十进制数)
13
>>> hex(15) \\将十进制的15转换成16进制数
'0xf'
>>> oct(0x0fab) \\将十六进制数的0x0fab转换成八进制数
'0o7653'
>>> 
```

### 工具模块

之前也有说过Python有很多别人写好的模块。这里直接导入别人写好的模块 随机数模块random。



然后我们来看看用随机数模块如何快速生成随机数。这个如果有学过C语言应该知道 让计算机生成随机数可以调用rand()函数，但是每次生成的序列是一样的。而且还要有种子值之类的。写起来还比较麻烦。下面看看Python怎么做的。

```python
>>> import random
>>> random.randint(1,100)
20
>>> random.randint(1,100)
80
>>> random.randint(1,100)
75
>>> random.randint(1,100)
14
>>> random.randint(1,100)
92
>>> 
```



接下来我们来对比一下C语言生成随机数怎么写

```c
include <stdio.h> 
include <stdlib.h> 
include <time.h> 

int main() 
{ 
    float s; 
    srand((int)time(0)); 
    s = (float)rand() / RAND_MAX; 
    s = 3 + (5-3) * s; 
    printf("%f\n", s); 
} 
```



相比之下C语言就复杂的多了。这是我们第一次接触模块，后面还会经常用到。



再来看一组实例吧，我刚开始学C语言的时候，老师会出一个题，要求给定几个数，让程序返回最大值。这个看似简单的任务真的当时做的头都是大的。写的很复杂。先来看用C怎么写的。

```c
include<stdio.h>
int maxfun(int a,int b);
int main()
{
 int i,j,N;
 int sum = 0,max = 0; 
 scanf("%d",&N);
 int a[N];
 for(i=0; i<N; i++){
  scanf("%d", &a[i]);
  if(a[i]>max){
   max = a[i];
   sum = i;
  }
 }
 printf("%d %d",N,sum);
}
```



再来看，这么复杂的程序，在python里，只需要简单的几行

```python
>>> max([3,6,7,9,1,4,])
9
```



其中用[]框起来的是列表 后面我们会提到。总之python实现排序是如此简单，别人已经跟你写好了。你只需要告诉他需要干嘛就行了。非常好用。不需要了解实现方法。python有很多很多模块，都是已经写好了的。你只需要关心 把什么东西传进去，把得到的东西再给什么模块处理就行了，不需要关心他到底是怎么找的。这就是python在写起来很快的原因。当然，前提条件是你要知道有这么个东西。比方说数学模块其实有很多东西的。

![1](Python-Basic\1.png)

前提是你要很懂数学 才能很好的去利用这些工具和模块



## Python内置类型-字符串

字符串的特点如下：

| 分类           | 特点 | 是否可变 |
| -------------- | ---- | -------- |
| String（字符） | 序列 | 不可变   |

字符串应该是用的比较多的。待会儿解释一下什么叫序列类型。以及不可变是什么概念。



字符串是可以单引号，双引号和三引号框起来。反过来说，用这些符号框起来的就代表里面的是字符串。先体验一下吧。



```python
>>> 'Playstation3'
'Playstation3'
>>> "Playstation3"
'Playstation3'
>>> """Playstation4"""
'Playstation4'

```



那么除了我们常用的英文字母，数字，中文等字符串，还有一些特殊的东西我们要了解

### 转意符号\

我们都知道单引号有特殊的意义 用于把字符串引起来，但是如果我想要输出正常的单引号又该怎么办呢？这个时候就需要使用转意符号，告诉程序这里的单引号没有特殊意义。就是一个普通的字符。

```python
>>> 'Python'Cisco'
  File "<stdin>", line 1
    'Python'Cisco'
                ^
SyntaxError: invalid syntax 
```

如果这么去做 会直接报错。那么现在在Python和Cisco之间的单引号前面加一个转意符号\

```python
>>> 'Python\'Cisco'
"Python'Cisco"
>>> 
```



除了上述这种情况，其实很多时候我们会碰到\t \r\n这种特殊符号。这个\t代表一个tab \r代表回车\n代表换行。

```python
>>> print('Cisco\tHuawei')
Cisco	Huawei
>>> 
```

你会发现中间空了一块出来，这就是制表符（table）的效果。



```python
>>> print('Cisco\nHuawei')
Cisco
Huawei
```

\n就是回车。



```python
>>> print('Cisco\rHuawei')
Huawei
```

\r为换行。



> 换行和回车的区别：
>
> 在计算机还没有出现之前，有一种叫做电传打字机（Teletype Model 33）的机械打字机，每秒钟可以打10个字符。但是它有一个问题，就是打完一行换行的时候，要用去0.2秒，正好可以打两个字符。要是在这0.2秒里面，又有新的字符传过来，那么这个字符将丢失。
>
> 于是，研制人员想了个办法解决这个问题，就是在每行后面加两个表示结束的字符。一个叫做“回车”，告诉打字机把打印头定位在左边界，不卷动滚筒；另一个叫做“换行”，告诉打字机把滚筒卷一格，不改变水平位置。
>
> 这就是“换行”和“回车”的由来。
>
> 后来，计算机发明了，这两个概念也就被般到了计算机上。那时，存储器很贵，一些科学家认为在每行结尾加两个字符太浪费了，加一个就可以。于是，就出现了分歧。
>
> 回车 \r 本义是光标重新回到本行开头，r的英文return，控制字符可以写成CR，即Carriage Return
>
> 换行 \n 本义是光标往下一行（不一定到下一行行首），n的英文newline，控制字符可以写成LF，即Line Feed
>
> 符号    ASCII码      意义
>
> \n        10        换行NL
>
> \r        13        回车CR
>
> 在不同的操作系统这几个字符表现不同，比如在WIN系统下，这两个字符就是表现的本义，在UNIX类系统，换行\n就表现为光标下一行并回到行首，在MAC上，\r就表现为回到本行开头并往下一行，至于ENTER键的定义是与操作系统有关的。通常用的Enter是两个加起来。
>
> 不同操作系统下的含义：
>
> \n:  UNIX 系统行末结束符
>
> \r\n: window 系统行末结束符
>
> \r:  MAC OS 系统行末结束符
>
>  

### 原始字符串r

如果我们想输出一个Windows里的路径例如D:\World of Warcraft\test\WoW64.exe 这里面会有多个斜杠。

```python
>>> print('D:\World of Warcraft\test\WoW64.exe')
D:\World of Warcraft	est\WoW64.exe
>>> 
```

你会发现这边\test由于出现了\t，程序会认为这是一个制表符。我们也不能保证Windows里的路径一定不出现这种特殊的东西。有两种办法解决。第一种是在每个斜杠前面加上\转意，来表示后面的斜杠没有特殊意义。这样比较麻烦。另一种办法就是使用r 原始字符串，代表后面的所有内容均是普通字符串，没有任何其他意义。

```python
>>> print(r'D:\World of Warcraft\test\WoW64.exe')
D:\World of Warcraft\test\WoW64.exe
```



### 什么叫有序或者说序列类型

之前提到了字符串是一种有序的类型，什么叫有序呢？就是字符串里的每个字符都有对应的位置。可以通过索引的办法提取出来。

```python
>>> String = 'Playstation4' \\将Playstation4的值赋予给变量String
>>> String[0]  \\提取变量String的第0号位置的值
'P'
>>> String[3] \\提取变量String的第3号位置的值
'y'
>>> 
```



计算机数数都是从0开始的。所以Playstation4对应的位置就是

![2](Python-Basic\2.png)

### 什么叫可变类型

刚才提到了序列，接下来谈谈可变。字符串的类型是不可变，也就是说你不能更改字符串里的某一个字的值。要想更改，只能重新整个赋值。

```python
>>> String = 'Playstation4' \\将Playstation4赋值给变量String
>>> String[3] = 'O' \\将String变量的第三位更改成字符串O
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object does not support item assignment
```

这边报错了 。代表字符串确实是不可变的，后面在讲列表的时候这种操作就是可以被执行的。



### 多态

python的多态说法非常多。这边直接拿实际操作举例吧

```python
>>> print('-'*10)
----------
```

上面代码的意思是 打印- 后面*10代表 -乘10。



```python
>>> 'Cisco'*4
'CiscoCiscoCiscoCisco'
>>> 3*2
6
```

如果是C语言这种 直接拿字符串乘一个数字是肯定会报错的。但是python这种语言的多态特性就可以做到这种事情

### 循环迭代

只要是序列类型 都可以用For循环进行迭代处理。

```python
>>> str = 'Cisco'
>>> for x in str:
...     print(x)
... 
C
i
s
c
o
```

第一次循环会迭代str里的第一个值C 然后打印出来 回车，第二次会迭代第二个值 i 打印出来 然后回车。以此类推。 每次循环迭代出的内容就会赋值给x。for循环可以对**序列类型**对象进行迭代。把每次迭代的结果进行输出。如果输出的时候不想要自动换行。可以这样

```python
>>> for x in str:
...     print(x,end='')
... 
Cisco>>> 
```

其中end=''代表每次迭代以空为结束。

### 索引和切片

**所有的序列类型都可以索引和切片**



拿字符串Playstation4来说。



![2](Python-Basic\2.png)





```python
>>> String = 'Playstation4'
>>> String[3]
'y'
>>> 
```

这是提取String字符串中3号位置的示例。[3]代表3号位置 这种方式叫索引。



也可以倒过来提取



```python
>>> String[-1]
'4'
>>> 
```



除了索引，我们还可以进行切片



```python
>>> String[1:7]
'laysta'
>>> 
```

 对比上面的位置 我们会发现处于7号位置的't'并没有出现。1号位的'l'出现了。



这种切片方式，左边的叫下边缘，右边的叫上边缘。默认上边缘是不要的。也就是输出了不包含上边缘



其他的切片方式如下：

```python
>>> String[:7] \\从0位开始到6
'Playsta'
>>> String[-2:] \\从-2开始一直到后面的所有位
'n4'
>>> String[:-2] \\从头开始到-2位（-2位不要）
'Playstatio'
>>> String[:] \\所有都要
'Playstation4'
>>> 
```



### 字符串的拼接格式问题

例如我们想把字符串拼一起



```python
>>> 'Play' + 'Station'
'PlayStation'
```

字符串之间的拼接是没问题的。



如果再来一个4呢

```python
>>> 'Play' + 'Station' + 4
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: can only concatenate str (not "int") to str
>>> 
```

你会发现报错 并且提示字符串不能和数字相加。这个时候需要做转换



```python
>>> 'Play' + 'Station' + str(4)
'PlayStation4'
>>> 
```



`str(4)`代表将括号里的内容转换成字符串。这样就可以拼接了。



## 字符串格式化表达式和方法

在输出字符串的时候我们希望能够把字符串做格式化，显示出来干净整齐，格式统一。这就是字符串格式表达式的目的。



字符串格式化有两种方法。第一种是传统表达式方法，第二种是新方法。来体验一下



### 传统方法（表达式）

String formatting expressions: '...%s...'(values)

%左边是一个格式化的字符串，包含一个或多个嵌套的转换目标，每个目标用%开始。

%右边提供一个或者多个对象(多个对象需要放在元组中)，这些对象用来替换%左边的转换对象

```python
'Playstation%d is %s'%(4,'belong to sony')
'Playstation4 is belong to sony'
```

能够看到。整个语句用%分隔开，分成左边和右边。左边是已经格式化了的字符串，右边是用来替换%d和%s的内容。

其中%d这个地方要替换的是整数

%s代表这个地方要替换的是字符串。

要替换的值放在元组里，元组在后面会介绍。其实就是用小括号括起来的东西。



一般%后面就会用到三个

%d---整数

%s---字符串

%f---浮点

#### 完整表达式语法

%[(keyname)]\[flags][width]\[.precision]typecode

- keyname代表%后面其实还可以放一个字典的键值，由于字典还没有涉及到，这边先放着
- flags可以指明字符串是左对齐(-)还是右对齐还是用0补齐,默认右对齐。
- width代表整体宽度
- 如果是小数还可以用.后面跟数字来表示小数点位数 例如.2f表示保留小数点后2位的浮点数

还是来实操感受一下

```python
>>> example = '...%d....%-10d....%10d...%010d'%(int,int,int,int)
>>> print(example)
...98765....98765     ....     98765...0000098765
```



第一个%d代表直接输出

第二个%-10d代表左对齐输出，宽度10

第三个%10d代表右对齐输出，宽度10

%010d代表右对齐输出，宽度10，不足的位置用0补齐

得出的结果就如上所示了。



接下来看看关于浮点数的输出

```python
>>> float = 6.123151253464567
>>> example = '%f|%.2f|%010.3f|%5.2f'%(float,float,float,float)
>>> print(example)
6.123151|6.12|000006.123| 6.12
```

第一个%f啥也不做

第二个%.2f代表保留小数点后2位

第三个%010.3f代表右对齐，长度10位，不够的用0补齐

第四个%5.2f代表右对齐，保留小数点后2位并且占5位长度



### 新方法

String formatting method calls:'...{}...'.format(values)

刚才讲的是传统的方法。现在来看一下新方法。

#### 完整语法

{fieldname component !conversionflag :formatspec}

- Filename是指定参数的关键字或数字，后面跟可选的'.name'(属性)或者'[index]'(键值)成分引用
- conversionflag可以是r,s或者a分别是该值上对repr，str，ascii内置函数的一次调用
- formatspec指定如何表示该值，包括宽度，对齐方式，补零，小数点精确度等。

还是来实操一下吧

```python
'{0},{1},and {2}'.format('Playstation','Nintendo','Mircosoft')
'Playstation,Nintendo,and Mircosoft'
```

这是基于位置的方法，意思是0号位置是playstation，1号位置是nintendo，2号位置是microsoft。还有基于名字的。

```python
str = '{Playstation4},{Switch},{XboxOne}'
str.format(Playstation4=Sony,Switch=Nintendo,XboxOne=Microsoft)
str.format(Playstation4='Sony',Switch='Nintendo',XboxOne='Microsoft')
'Sony,Nintendo,Microsoft'
```

这就相当于有Playstation位，Switch位和XboxOne位。

当然 这两种方法可以混合起来用。

刚才提到了老方法可以控制宽度，小数点位数等。新方法当然也可以。

```python
str='{Sony:10},{Microsoft:10},{Nintendo:10}'
str.format(Sony='Playstation4',Microsoft='XboxOne',Nintendo='Switch')
'Playstation4,XboxOne   ,Switch    '
```

跟老方法一样，:10代表宽度。

```python
str='{Sony:10},{Microsoft:^10},{Nintendo:<10}'
str.format(Sony='Playstation4',Microsoft='XboxOne',Nintendo='Switch')
'Playstation4, XboxOne  ,Switch    '
```

^代表居中。

<代表左对齐

\>代表右对齐

关于小数的操作方法和老方法一样，就不再赘述了。



## 正则表达式

正则表达式较为复杂。我参考了

[这里]( http://www.regexlab.com/zh/regref.htm )和[这里](https://zh.wikipedia.org/wiki/正则表达式 )来写。

正则表达式可以用来匹配一些特定的字符串。其实在BGP就有用到正则表达式。BGP的AS-PATH就是使用正则表达式来匹配AS号。

### 如何匹配各种字符

在Python中想使用正则表达式需要引入re模块。使用`import re`来引入。

```python
>>> import re
>>> re.match('explorer.exe','explorereexe')
<re.Match object; span=(0, 12), match='explorereexe'>
>>> re.match('explorer.exe','fjdlskajlf')
>>> 
```

最简单的使用方法就像上面这样。

re.match代表使用re模块里面的match动作，括号中左边的就是正则表达式，用逗号隔开之后右边就是要匹配的东西。如果有匹配上，程序就会返回值。这边能看到确实匹配上了，匹配上的内容是0到12位。如果匹配不上就不会返回任何值。

这边能匹配上可能会有问题。我匹配的条件是**explorer.exe**给他匹配的东西是**explorereexe**。但是还是匹配上了。明明有个点不一样，为什么还是能匹配上？这边就需要了解正则表达式中符号的特殊意义。



| 表达式 | 能匹配的东西                                 |
| ------ | -------------------------------------------- |
| \d     | 任意数字，即0～9                             |
| \w     | 任意字母，下划线或者数字就是A~Z,a-z,0~9以及_ |
| \s     | 包括空格、制表符、回车换行等空字符           |
| .      | 除了换行符号\n以外的所有字符                 |

所以难怪上面能匹配上了。点可以匹配除了换行以外的任意字符。



那如果只想匹配explorer.exe怎么办？这边就需要用到转意符。

```python
>>> re.match('explorer\.exe','explorer.exe')
<re.Match object; span=(0, 12), match='explorer.exe'>
>>> re.match('explorer\.exe','exploreraexe')
```



### 匹配多种字符的表达式

使用方括号\[]扩起来一些字符，能够匹配其中的任意一个字符；用\[^]扩起来一些字符，能够匹配除了其中这些字符之外的任意字符。只能匹配其中的一个或者除开其中的一个，不能是多个。下面看一些例子。

| 表达式    | 可匹配                      |
| --------- | --------------------------- |
| [abc5@]   | a,b,c,5,@                   |
| [^abc]    | 除了a，b，c之外的任意字符   |
| [a-k]     | a-k之间的任意字母           |
| [^A-F0-3] | 除了A到F和0-3之间的任意字符 |

下面来看几个实例

```python
>>> re.match('[bcd][bcd]','bc123')
<re.Match object; span=(0, 2), match='bc'> \\匹配上的是bc
>>> re.match('[^abc]','123')
<re.Match object; span=(0, 1), match='1'> \\匹配上的是1
```



### 修饰匹配次数

 前面章节中讲到的表达式，无论是只能匹配一种字符的表达式，还是可以匹配多种字符其中任意一个的表达式，都只能匹配一次。如果使用表达式再加上修饰匹配次数的特殊符号，那么不用重复书写表达式就可以重复匹配。



使用方法是："次数修饰"放在"被修饰的表达式"后边。比如："\[bcd]\[bcd]" 可以写成 "[bcd]{2}"。



| 表达式 | 作用                           |
| ------ | ------------------------------ |
| {n}    | 表达式重复n次                  |
| {m,n}  | 表达式至少重复m次，最多重复n次 |
| {m,}   | 表达式至少重复m次              |
| ?      | 匹配表达式至少出现0或者1次     |
| +      | 表达式至少出现1次              |
| *      | 表达式不出现或者出现任意次     |

下面来看一组例子

```python
>>> re.match('[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}','192.168.1.100')
<re.Match object; span=(0, 13), match='192.168.1.100'>
```

可以用上述的表达式匹配IPv4地址。

IPv4的地址每8位都用点来分隔，数字的范围是0到9则用\[0-9]匹配。 出现至少一次，最多三次，就用{1,3}匹配。小数点本身需要有\\.来匹配。当然 这样写非常长。也不太好看。有没有简单方法呢。

### 表示抽象意义的符号

| 表达式 | 作用                                                     |
| ------ | -------------------------------------------------------- |
| ^      | 于字符串开始的地方匹配，本身无意义                       |
| $      | 于字符串结束的地方匹配，本身无意义                       |
| ()     | 在被修饰匹配次数的时候，括号中的表达式可以作为整体被修饰 |
| \|     | 左右表达式之间是或的关系，匹配左边或者右边。             |

来看两组例子：

```python
re.match('([0-9]{1,3}\.){3}[0-9]{1,3}','192.168.1.1')
<re.Match object; span=(0, 11), match='192.168.1.1'>

>>> re.match('^aaa','aaafdafadfawfa')
<re.Match object; span=(0, 3), match='aaa'>
```



第一个例子是将上面的匹配IPv4地址进行了简化。用括号将\[0-9]{1,3}\\.扩起来。然后后面跟上{3}代表出现3次。后面再单独写\[0-9]{1,3} 代表不带点 在单独出现一次。这样一来就能匹配到完整的IPv4地址



后面的则是^aaa,代表字符串必须以aaa开头。至于后面是什么不管。



## 列表和字典

| 对象               | 类型           | 可变 |
| ------------------ | -------------- | ---- |
| List(列表)         | Sequence(序列) | 可变 |
| Dictionaries(字典) | Mapping(映射)  | 可变 |

### 列表

一看到序列类型，我们就知道，一定可以根据索引提取内容，可以切片。可以改的。字符串我们体会过，根据某个位置去改值 是不行的。但是列表就可以。

列表的主要属性有以下几个：

- **任意**对象的**有序集合**
- 通过**偏移**读取
- 可变长度，异构以及任意嵌套
- 属于**可变序列**的分类
- 对象引用数组

什么叫任意对象？就是说列表里放字符，数字，甚至再放列表都是可以的。有序就是可以通过索引的方式提取。通过操作来感受一下：

```python
>>> list = ['cisco',123,['H3C','BSD','Linux']]
>>> list
['cisco', 123, ['H3C', 'BSD', 'Linux']]
>>> list [2]
['H3C', 'BSD', 'Linux']
```





又或者把0号位置的cisco改成大写的CISCO

```python
>>> list[0]='CISCO'
>>> list
['CISCO', 123, ['H3C', 'BSD', 'Linux'], 'Playstation']
```



如果想要提取列表中的列表，则可以这样

```python
>>> list[2][0]
'H3C'
>>> list[2][1]
'BSD'
```



列表也可以直接用+号连接在一起。



#### 列表的常见操作方法

##### len

使用len()可以提取列表的长度

```python
>>> list
['CISCO', 123, ['H3C', 'BSD', 'Linux'], 'Playstation']
>>> len(list)
4
>>> 
```

这里显示list这个列表有4个元素。分别是CISCO 123 一个列表和 Playstation。计算长度是从1开始算的。

##### in 

也可以用 in 来判断

```python
>>> 'CISCO' in list
True
>>> 'Cisco' in list
False
```



以及所有序列类型都可以用for循环。这里不再赘述。



##### append

我们可以通过append方法来往列表后面追加



```python
>>> list.append('Playstation')
>>> list
['cisco', 123, ['H3C', 'BSD', 'Linux'], 'Playstation']
```



##### insert

插入操作。可以在列表中指定位置去插入。例如上面的list。在1位置插入。

```python
>>> list = [1,'Cisco','H3C','Python'] \\创建列表
>>> list \\显示列表
[1, 'Cisco', 'H3C', 'Python']
>>> list.insert(1,'Playstation') \\在1位置插入Playstation
>>> list  \\显示列表
[1, 'Playstation', 'Cisco', 'H3C', 'Python'] \\1位置变成了Playstation，其他元素往后顺移。
```



##### remove

remove 可以移除列表中的某个元素。只能填具体的值。

```python
>>> list
[1, 'Playstation', 'Cisco', 'H3C', 'Python']
>>> list.remove('Cisco')
>>> list
[1, 'Playstation', 'H3C', 'Python']
```





### 字典

字典是映射类型，是通过“键”来提取“值”。所以不能通过索引提取值。由于不是序列，所以也不能切片了。



创建字典的方法很简单 如下：

```python
a = {'name':'leexu','age':23}
 a.get('name')
'leexu'
```

可以通过get()来提取键值.



也可以用dict()来创建

```python
b = dict(name="leexu",age=23)
b
{'name': 'leexu', 'age': 23}
```



#### 字典的增加和修改

给字典增加键值对，如果键已经存在，则会覆盖，如果不存在，则增加新的键值对

```python
b = dict(name="leexu",age=23)
b
{'name': 'leexu', 'age': 23}
b['name']='lixu'
b
{'name': 'lixu', 'age': 23}
b['job']='teacher'
b
{'name': 'lixu', 'age': 23, 'job': 'teacher'}
```



