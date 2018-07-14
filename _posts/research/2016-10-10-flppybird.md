---
layout: post
title: "如何评价 Flappy bird 玩家的游戏水平"
author: changeforan
category: research
tags: [algorithm, game]
---

![enter image description here]({{ site.url }}/img/flappybird.jpg)


##问题来源：
[Flappy bird](http://baike.baidu.com/link?url=p_HLU-ZwYNxyAqslOrKmfhwWjfHoObiAmHQ8y18a0TQ6vF-3NF-cpSdW3n_cYg54Ta-QBfrnxzc45TakpCfzQKpY5h7JvXGViR9Qi_2-7De)，[IMPOSSIBLE ROAD](http://www.appgame.com/archives/158145.html)之类的游戏有一个共同的特点，及玩家的目标是闯过尽量多的关卡，而每个关卡的难度几乎是一样的，不会随着游戏进行显著提高，姑且将此类游戏称之为“连续生存模型”。
那么如何评价一个玩家的水平呢？此类游戏几乎都使用记录历史最佳成绩的方式，这种方式有很强的刺激作用，不同的玩家之间会比较各自的最佳成绩，而玩家自己则会不断试图突破自己的最佳成绩。
但是，最佳成绩的偶然性很大，某次的“超神”发挥之后可能很久都不会达到这样的成绩。不难想到另一个方法是取玩家游戏成绩的平均值，但取平均值会带来评价标准无上限的问题，平均成绩1000的玩家和平均成绩2000的玩家，水平究竟相差多少呢。
那么一个玩家该如何在此类游戏中判断自己的实际水平，又或者说，玩家在开始一局新游戏前应该对此次游戏成绩抱有怎样的期望呢？

##问题提出：

**若一闯关游戏有无限关，玩家每关的存活概率为P，求通关数的期望。**
显然$0<P<1 $ ，期望值为 $1 \times P + 2 \times P^2 + 3 \times P^3 + \dots + n \times P^n $  ，即

$$ E =  P + 2 \times P^2 + 3 \times P^3 + \dots + n \times P^n  (n \to \infty) $$

将上式两边同时乘以 P 并与原式相减，得

$$ E(1-P) = P + P^2 + P^3+ \dots +P^n - n\times P^{n+1} $$

由等比数列求和公式得

$$ E={ P \over {1-P}^2} - {(1+n)P^{n+1} \over {1-P}^2} $$

其中 $ (1+n)P^{n+1} 在 n \to \infty $ 时为0，所以

$$ E =  { P \over {(1-P)}^2} $$

由此可见，若玩家每关的存活几率是80%，那么其通关数的期望值为20。
反之，如果一个玩家在多次游戏后，其成绩平均值为100，那么其每关的存活几率约为90.49%。

因此在“连续生存模型”式的游戏中，与其使用平均成绩 E 来判断玩家水平，不如使用反解以上方程得到的百分制（ $ P \times 100 $ ）评价标准。在这个标准中，玩家的水平被限定在0-100分之间，能得到95分以上的玩家都可谓是大神。