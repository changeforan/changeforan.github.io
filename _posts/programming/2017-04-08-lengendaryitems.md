---
layout: post
title: "大酋长的脱非入欧之旅"
author: changeforan
category: programming
use_math: true
tags: [algorithm, math]
---

> 题目链接：[#1489 : Legendary Items](http://hihocoder.com/problemset/problem/1489)

## 问题描述

本题介绍了一个很实用的游戏物品获取机制，每当玩家完成一个任务，会有一定的概率获取一件传说装备，虽然题目中是通过完成任务来获取传说装备，但同样适用类似于通过开卡包来获得 SSR 卡的游戏。
- 玩家的初始获取概率为 $P\%$.
- 每失败一次，获取概率会在当前基础上提高 $Q\%$ 但最高不会超过$100\%$.
- 每获得一件传说装备，获取概率变为$ \lfloor {P \over {2^I}} \rfloor \%$ ，其中$I$是玩家当前获得的传说装备总数.

该机制的特点保证了：
- 玩家获取传说装备总是越来越难
- 玩家只要不停的完成任务，总是能获取传说装备

求解，在已知$P, Q$的情况下，一个玩家集齐$N$件传说装备所需要完成的任务数的期望值。也就是问一个非洲大酋长要想集齐 SSR，需要开多少个卡包呢。

## 解题思路

用 $f(x,i,n)$ 表示初始概率为$x$，已经获得了$i$件装备，需要收集$n$件装备所需完成任务数的期望值，那么$f(P\%,0,N)$即为本题答案。
完成当前任务使任务数加 1, 当前任务完成后，有$x$的几率获得一件装备，$1-x$的几率没有获得，故

$$
f(x,i,n)=1+x \cdot f({\lfloor  {P \over {2^{i+1}}} \rfloor\%},i+1,n)+(1-x) \cdot f(x+Q\% > 1 ? 1 : x+Q\%,i, n) 
$$

当$i=n 时，f(x,i,n)=0$，根据以上公式不难写出递归求解程序，注意到 n 在公式两边是不变的，所以函数参数实际上只需要两个。

```java
public class LegendaryItems {
	 static int P,Q,N;
	 public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		P = sc.nextInt();
		Q = sc.nextInt();
		N = sc.nextInt();
		System.out.println(String.format("%.2f", foo(P / 100.,0)));
	 }
	 private static double foo(double x, int i) {
		if (i == N) {
			return 0;
		}
		double tmp1, tmp2;
		if (x > 0){
			tmp1 = x * foo( Math.floor(P / Math.pow(2, i + 1)) / 100., i + 1);
		}else {tmp1 = 0;}
		if (x < 1){
			tmp2 = (1 - x) * foo( x + Q / 100. > 1 ? 1 : x + Q / 100. , i);
		}else {tmp2 = 0;}
		return 1 + tmp1 + tmp2;
	}
}
```

## 算法优化
虽然这个解法答案是正确的，但是严重超时，即使使用了动态规划常见的打表法进行优化。
优化的关键在于，当一个玩家已经获得了 7 件装备后，他下一个任务完成后获得装备的概率一定是 0。且此后玩家每需获得一件装备所需完成的任务数期望都是一样的。因此，当$N > 7$ 时，可以理解成先获取 7 件装备，此后的$N-7$件装备的任务数期望是 $f(0,6,7)$的$N-7$倍。$f(0,6,7)$表示概率为0时，再获取一件装备的任务期望数。

$$
f(x,0,n)=f(x,0,7)+(n-7) \times f(0,6,7)
$$

## AC 代码

```java
public class LegendaryItems {
	static int P,Q,N;
	static Double[][] table = new Double[101][8];
	public static void main(String[] args) {
		Scanner sc = new Scanner(System.in);
		P = sc.nextInt();
		Q = sc.nextInt();
		N = sc.nextInt();
		if (N > 7) {
			int m = N - 7;
			N = 7;
			System.out.println(String.format("%.2f", foo(P / 100.,0) + m * foo(0, 6)));
		}else{
			System.out.println(String.format("%.2f", foo(P / 100.,0)));
		}
	private static double foo(double x, int i) {
		if (i == N) {
			return 0;
		}
		if (table[(int) x * 100][i] != null) {
			return table[(int) x * 100][i];
		}
		double tmp1, tmp2;
		if (x > 0){
			tmp1 = x * foo( Math.floor(P / Math.pow(2, i + 1)) / 100., i + 1);
		}else {tmp1 = 0;}
		if (x < 1){
			tmp2 = (1 - x) * foo( x + Q / 100. > 1 ? 1 : x + Q / 100. , i);
		}else {tmp2 = 0;}
		table[(int) x * 100][i] = 1 + tmp1 + tmp2;
		return 1 + tmp1 + tmp2;
	}
}

```
