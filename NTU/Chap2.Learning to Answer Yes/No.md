---
tags: 機器學習基石
title: 第二章
---

# 例子
![](https://i.imgur.com/Lrr7aYr.png)

假設以銀行是否要發卡給一個人為例，我們可能會給電腦一個人可能會影響是否會準時還錢相關的特徵，例如年紀、年薪...，並告訴電腦這個人是否能準時還錢，成為我們的trainging examples。希望電腦看完這些training examples後能夠用一個演算法$A$從$H$中找出一個和$f$最相近的$g$出來。

而第一個問題就是：我們要怎麼產生$H$呢？有很多種方法，這裡先介紹一種比較容易理解的percetron algorithm。

# Perceptron Algorithm
上圖中有許多的特徵，但是每個特徵對於是否會準時還錢的影響力都不同，如果我們只是將所有的特徵的值平等看待是不公平的，對於影響力比較大的特徵，我們應該給予比較大的權重，反之亦然，流程如下
1. 我們會在每個特徵的值乘上權重
2. 加總
3. 如果該加總值>theshold，則判定能準時還錢=>發卡，反之則不發卡

![](https://i.imgur.com/EuqNUlu.png)

$y = \sum\limits_i {{w_i}{x_i}}  - threshold$</br>
則
- ${y > 0}$： 發卡
- ${y < 0}$： 不發卡
