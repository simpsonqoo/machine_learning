---
title: 第一章
tags: 機器學習基石
---

# 符號
- Input data： ${\bf{x}} \in X$
- Output data： $y \in Y$
- $f:X \to Y$：input為$X$，經由一定的行為(通常是未知的)後得到輸出  (e.g. 如何得知一個人是否要發卡)
- Training example： $D:\;\{ ({{\bf{x}}_1},{y_1}),({{\bf{x}}_2},{y_2}),...,({{\bf{x}}_n},{y_n})\}$，目前已知的input及ouput data對應的結果(e.g. 目前銀行手中客戶及是否有欠錢的紀錄)
- Hypothesis(假說)$g:X \to Y$：預測$X \to Y$的對應關係

# 圖示過程

![](https://i.imgur.com/63hrqaG.png)

$f$是未知的$X \to Y$對應的關係，也是我們想要去找到的，而input data經由$f$後會得到$Y$，構成training example，電腦只會看到training example，試圖用學習的演算法$A$得到一個近似於$f$的$X \to Y$對應關係$g$，我們會希望$g$和$f$越像越好。

而實際上$g$可能是由一個hypothesis set$H$而來，裡面有很多可能的hypothesis，learning algorithm就是希望找出$H$裡面最好的那個$g$。
![](https://i.imgur.com/ZUJbqmr.png =300x)
