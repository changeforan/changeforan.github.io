---
layout: post
title: "随机生成合法字符"
author: changeforan
category: programming
use_math: true
tags: [algorithm, java]
---

将大小写字母与阿拉伯数字称为“合法字符”，如何随机生成这些字符呢?  
显然，合法字符包含 0 ~ 9, A ~ Z, a ~ z 共计 62 个字符，但这些字符在 ASCII 码表中是不连续的，若用$x,x\in [0,61]$表示随机生成数，$f(x)$表示合法字符的 ASCII 编码值，则其函数关系为以下分段函数：

$$
f(x) =
\begin{cases}
x+48,& \text{$0 \le x < 10$} \\\
x+48+7,& \text{$10 \le x < 36$} \\\
x+48+7+6,& \text{$36 \le x \le 61$}
\end{cases}
$$

<!--more-->

实现这个分段函数最朴素的方法是使用分支语句

```java
Random random = new Random(); 
int x = random.nextInt(62);
System.out.print((char)( x< 10 ? x + 48 : x < 36 ? x + 55 : x + 61));
```  

另一种思路是寻找分段函数的统一表达式

令$x' = x + 48$,则原函数变为:

$$
 f(x') =
\begin{cases}
x',& \text{$48 \le x' < 58$} \\\
x'+7,& \text{$58 \le x' < 84$} \\\
x'+7+6,& \text{$84 \le x' \le 109$}
\end{cases}
$$

设$P = \lfloor {x' \over 58} \rfloor , Q = \lfloor {x' \over 84} \rfloor $,那么有

$$
\begin{cases}
P=0,Q=0,& \text{$48 \le x' < 58$} \\\
P=1,Q=0,& \text{$58 \le x' < 84$} \\\
P=1,Q=1,& \text{$84 \le x' \le 109$}
\end{cases}
$$

得

$$
f(x')=x'+\lfloor {x' \over 58} \rfloor \times 7 + \lfloor {x' \over 84} \rfloor  \times 6
$$

实现代码如下

```java
Random random = new Random();
int x = 48 + random.nextInt(62);
System.out.print((char)( x + ( x / 58 ) * 7 + ( x / 84) * 6 ));
```
