---
date: 2021-03-03T13:50:08+08:00  # 创建日期
author: "Rustle Karl"  # 作者

# 文章
title: "Ffmpeg Doc 05 Eval"  # 文章标题
# description: "文章描述"
url:  "posts/ffmpeg/docs/ch05_eval"  # 设置网页永久链接
tags: [ "ffmpeg"]  # 标签
series: [ "FFmpeg 从入门到放弃"]  # 文章主题/文章系列
categories: [ "学习笔记"]  # 文章分类

# 章节
weight: 20 # 排序优先级
chapter: false  # 设置为章节

index: true  # 是否可以被索引
toc: true  # 是否自动生成目录
draft: false  # 草稿
---

## 表达式计算/求值
在计算表达式时，ffmpeg通过`libavutil/eval.h`接口调用内部计算器进行计算。

表达式可以包含一元运算符、运算符、常数和函数

两个表达式`expr1`和`expr2`可以组合起来成为"expr1;expr2" ，两个表达式都会被计算，但是新表达式（组合起来的）值实为表达式`expr2`的值。

表达式支持的二元运算符有:`+`，`-`，`*`,`/`,`^`

一元运算符:`+`,`-`

以及下面的函数：

- abs(x) 
	
	返回x的绝对值.
- acos(x)

    计算x反余弦 .
- asin(x)

    计算x的反正弦.
- atan(x)

    计算x反正切.
- between(x, min, max)

    判断min<=x<=max是否成立，成立返回1，否则返回0.
- bitand(x, y)
- bitor(x, y)

	返回x和y按位与/或的值

	**注意**计算是作为整数的，转换为整数和转换回浮点数都会损伤精度。这可能造成意外结果(通常2^53和更大的数)
- ceil(expr)

    返回大于expr的最接近整数，例如"ceil(1.5)" 返回"2.0".
- clip(x, min, max)

    Return the value of x clipped between min and max.
- cos(x)

    计算x的余弦.
- cosh(x)

    计算x的双余弦.
- eq(x, y)

    如果x==y返回1，否则为0 otherwise.
- exp(x)

    计算指数x，(底数为e,即计算欧拉数（Euler’s number）)
- floor(expr)

    返回不大于expr的整数，例如 "floor(-1.5)" 为 "-2.0".
- gauss(x)

    计算x的高斯（Gauss ）函数值,即计算(-x*x/2) / sqrt(2*PI).
- gcd(x, y)

    返回x和y的最大公约数，如果x和y为0或者任意数小于0则行为未定义。
- gt(x, y)

    返回判断x>y的结果，符合则为1，否则为0
- gte(x, y)

    返回判断x>=y的结果，符合则为1，否则为0
- hypot(x, y)

    这个和C语言中的函数有相同名字和功能，相当于计算"sqrt(x*x + y*y)",是求长为x，宽为y的斜边长度（勾股定理）
- if(x, y)

    判断x值，如果x值为非0，则返回y，否则返回0
- if(x, y, z)

    判断x值，如果x值为非0，则返回y，否则返回z.
- ifnot(x, y)

    判断x值，如果x值为0，则返回y，否则返回0
- ifnot(x, y, z)

    判断x值，如果x值为0，则返回y，否则返回z.
- isinf(x)

    如果x值是正负无穷则返回1.0.否则返回0.0
- isnan(x)

    如果x是`NAN`则返回1.0，否则返回0.0
- ld(var)

    加载预订的内部变量var对应值，其中值是利用st(var, expr)存储的
- log(x)

    计算x的自然对数值
- lt(x, y)

    返回x<y判断式值，符合为1，否则为0
- lte(x, y)

    返回x<=y判断式值，符合为1，否则为0
- max(x, y)

    返回x和y中的更大的值
- min(x, y)

    返回x和y中的更小的值
- mod(x, y)

    计算x%y
- not(expr)

    如果expr==0则返回1，否则返回0
- pow(x, y)

    计算"(x)^(y)".
- print(t)
- print(t, l)

    以日志层次l打印t，如果l没有定义则采用当前默认日志层次，返回打印内容。
- random(x)

    返回一个0.0-1.0间的随机数，x是一个随机数种子。
- root(expr, max)

    对于不同的输入计算表达式expr的值，直到max输入值。即依次取ld(x)，x的值为0..max，把ld(x)值作为参数计算expr值

    表达式expr必须是一个连续函数，否则结果不定。

    ld(0)被用作expr表达式的参数，所以表达式可以依据不同的值计算多次。
- sin(x)

    计算x的正弦
- sinh(x)

    计算x的双曲正弦
- sqrt(expr)

    计算x的平方根。相当于 "(expr)^.5".
- squish(x)

    计算 1/(1 + exp(4*x)).
- st(var, expr)

    对var变量在内部存储一个expr值，供以后使用，var范围为0-9.**注意**这些变量当前不能在表达式间共享
- tan(x)

    返回x的正切.
- tanh(x)

    计算x的双曲正切
- taylor(expr, x)
- taylor(expr, x, id)

    计算泰勒（Taylor）级数值。给出表达式（ld(id)）在0阶的导数函数,即taylor(expr,x)=taylor(expr,x,0)

    如果级数不收敛，则结果是不确定的。

    ld(id)用来表示expr的导数阶，这意味着对给定的表达式，输入不同的值可以通过ld(id)进行多次计算。这里我们假定不是预设的0阶。

    **注意**当你用一个Y值替代默认的0时，相当于计算 taylor(expr, x-y) 
- time(0)

    返回当前时间，单位为秒
- trunc(expr)

    返回expr最接近的（向0）整数，如"trunc(-1.5)" 值为 "-1.0".
- while(cond, expr)

    当cond不为0时循环执行expr,直至cond为0 

有如下一些常量:
- PI

    单位圆周长与直径比，约3.14 
- E

    exp(1)计算值 (Euler’s 欧拉数),约2.718 
- PHI

    黄金分割比，(1+sqrt(5))/2计算值，约1.618 

以及布尔运算，其中非0值表示"true"(真),以及运算符:
- `*` 表示 `AND` 与操作
- `+` 表示 `OR` 或操作

例如：
	要表示 if (A AND B) then C


	等效于if(A*B,C)

如果你了解C语言代码，其所有的一元和二元以及定义的常数均可用于表达式。

表达式也支持国际标准的单位前/后缀（定义），例如`i`附加在数值后，表示这个数值是基于1024而不是1000计算幂的，"B"表示"Byte"，并可以附加一个单位前缀或者当地使用，例如允许`KB`，`MiB`，`G`和`B`作为单位后缀。

下面的列表就是当前遵循的国际体系前缀列表，并给出了对应2的整10次方值：

y

    10^-24 / 2^-80 
z

    10^-21 / 2^-70 
a

    10^-18 / 2^-60 
f

    10^-15 / 2^-50 
p

    10^-12 / 2^-40 
n

    10^-9 / 2^-30 
u

    10^-6 / 2^-20 
m

    10^-3 / 2^-10 
c

    10^-2 
d

    10^-1 
h

    10^2 
k

    10^3 / 2^10 
K

    10^3 / 2^10 
M

    10^6 / 2^20 
G

    10^9 / 2^30 
T

    10^12 / 2^40 
P

    10^15 / 2^40 
E

    10^18 / 2^50 
Z

    10^21 / 2^60 
Y

    10^24 / 2^70 
