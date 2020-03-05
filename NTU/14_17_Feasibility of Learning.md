---
tags: 機器學習基石
title: 看data學到東西是可信的嗎？
---

[TOC]

以下內容都來自於林軒田老師-[機器學習基石-Feasibility of Learning](https://www.youtube.com/watch?v=tOgbh5_747w&list=PLXVfgk9fNX2I7tB6oIINGBmW50rrmFTqf&index=14)
# 無論怎麼學習都是無效的？
## 例子1
![](https://i.imgur.com/SMsq3Ek.png)
如上圖，如果給了上方六個例子，要你因此得出一個規律($g(x)$)後，下方的狀況輸出為何？

而無論給1或是-1，都是有道理的
![](https://i.imgur.com/sHBv2gC.png)

如果是以對稱性來說，那應該是1，如果是以左上角的顏色來區分，那應該是-1，換句話說，無論我們推導出來答案是甚麼都有可能是錯的。
所以假設我們經由演算法後得知$g(x)$，我們怎麼能夠相信我們得到的推論是正確的呢？如果連推導出來的答案都沒有信心，那還能學習到甚麼？

## 例子2
![](https://i.imgur.com/tXIUJKL.png)

假設輸入有三個feature，每個feature都只有兩種可能(0 or 1)，輸出只有一個維度，結果也是只有兩種可能(0 or 1)，我們能經由以上五種可能推得hypothesis，而有信心剩下的case都能推論正確嗎？

答案是否定的，我們只能從$2^{2^3}=256$種hypothesis中去除掉不合理的$2^{2^3}-2^3$種，剩餘的$2^3$種都是有可能的。
![](https://i.imgur.com/hh7zzWZ.png)

我們只能保證在已看到的data是正確的，剩下的case就落入上例一樣的狀況中，怎樣的結果都是有可能的。

所以如果data沒辦法看到所有的case的話，沒看過的case都有可能推導($g(x)$)是錯的，但如果所有的case都看的到，那也不需要學習了。

# Hoeffding's inequality
## 例子
![](https://i.imgur.com/e7uoEzD.png)

假設我們想知道罐子內橘色和綠色球的比例，假設我們無法一次看完全部的球，我們能藉由一次看部份的球的狀況得到罐子內各顏色球的比例嗎？

實際上雖然不能"篤定"的說比例為多少，但是我們可以說我們"多有信心"覺得比例為多少

這就是Hoeffding's inequalliy公式想要告訴我們的訊息，Hoeffding's inequalliy公式為
$$P[|\nu  - \mu | > \varepsilon ] \le 2{e^{ - 2{\varepsilon ^2}N}}$$

![](https://i.imgur.com/uXjpqg3.png)

- $\mu$為實際罐子內的色球比例
- $\nu$為sample中的色球比例
- $N$為sample中總球數

整句話可以這樣解讀，不等式左方為sample色球比例和實際冠中色球比例差為$\varepsilon$的情況下的機率

所以只要右方算出來的值夠小，我們大致上可以說，$\nu$和$\mu$是差不多的(probably approximately correct(PAC))。

而不等式右方不需要得知$\mu$的資訊，所以一般情況下(不知道實際狀況)我們是可以得知這個值的，就可以得知我們有多少信心有這個結論。

而由式子我們也可以觀察到，當$N$夠大時，右方數值就會很小，信心就會比較強，比較有信心得到$\nu = \mu$的結論。

## 由Hoeffding ineqally得到hypothesis的信心
![](https://i.imgur.com/TNqcIsw.png)

有前面罐子的例子後，我們可以用這個例子和之前提到的hypothesis的信心做對應。
hypothesis in data $\boldsymbol x$: $h(\boldsymbol x)$
實際上input $\boldsymbol x$的輸出: $f(\boldsymbol x)$
- $\mu$ -> $h(\boldsymbol x ) = f(\boldsymbol x)$的機率: 也就是所有case中有多少推論和實際結果是相同的
- 橘球 -> 實際和hypothesis結果不同的case
- 綠球 -> 實際和hypothesis結果相同的case
- 取N個sample -> 取N個$\boldsymbol x$

我們就可以得到對應的關係，我們雖然無法知道所有的$x$，但是可以藉由目前已知的sample $x$，看看這些sample在hypothesis中的結果，希望這些sample能夠代表這些已知sample外在hypothesis的結果。如果sample內和sample外結果都差不多，而且sample內的預測誤差很小，這樣就能hypothesis和target function不會差很多。

>
>舉個例來說，假設想知道全台灣抽菸人口，如果我們隨機取10萬人，在10萬人中抽菸人口比例為10%，此時hypothesis為10%，這個hypothesis在這10萬人中預測誤差=0，如果能夠證明這10萬人能夠代表權台灣人，也就是在這10萬人之外的台灣人抽菸比例差不多，就可以證明實際台灣抽菸人口比例大約為10%。

# 新的learning流程
而得到學習的流程如下
![](https://i.imgur.com/0i5JkVq.png)

橘色部分就是此章加上的流程，由於我們無法得到所有的sample $\boldsymbol X$，只能得到已知的部分sampe(${x_1},{x_2},...,{x_N}$)，而sample的數量可以推得某一個hypothesis $h$和target function $f$是否接近。

- $E_{out}$: 未知的sample在hypothesis中預測失敗的比例(e.g. 2300萬-10萬台灣人抽菸人口比例和1/3的差距)
- $E_{in}$: 已知的sample在hypothesis中的表現(e.g. 10萬台灣人抽菸人口和1/3的差距)

若${E_{in}} \approx {E_{out}}$，則稱為PAC(probably approximately correct)

由於Hoeffding's inequality不需要知道所有case的結果，僅需要知道$\varepsilon$及$N$就能知道hypothesis和target function的關係。

# N很大且$E_in$很小真的保證能找到最佳的hypothesis？
但是，如果在眾多hypothesis中拿了$E_in$最小的hypothesis，真的會是最好的嗎？

答案可能是否定的，通常我們會希望有很多的hypothesis set可以讓learning algorithm找出最佳的那個，但是如果hypothesis set中有越多hypothesis，就越有可能有某個hypothesis $E_{in}$和$E_{out}$差很多，如果因為該hypothesis在一個data set中表現很好就選這個hypothesis，很有可能就只是因為這個data set很合這個hypothesis的胃口而已，而且在hypothesis set很大的情況下，這種狀況幾乎是必然的。而當有某個data使得$E_{in}$和$E_{out}$差很遠時，這個data就會稱為bad data。

e.g.
![](https://i.imgur.com/OM7luls.png)

一樣以拿球的例子來說，不同球比例的瓶(同一個data set對不同hypothesis而言，會有不同的表現情況)中每種瓶子一次各拿十顆球，綠球代表hypothesis和target輸出一樣的狀況，假設瓶子數量很多，是很有可能至少有一個瓶子拿出來的球全都是綠球，一旦發生一個，因為其${E_{in}}$很低所以一定會被挑中，但其實其$E_{out}$很高並不是個好hypothesis

e.g.
假設一個hypothesis ${E_{in}}$和${E_{out}}$差距很小的機率是1%，有M個hypothesis，則至少有一個hypothesis ${E_{in}}$和${E_{out}}$差距很大的機率是$1-0.99^M$，當M越大，就越可能有${E_{in}}$和${E_{out}}$差很多的情況。

# 修正問題
所以，Hoeffding equality僅能對一筆data對於"一個"hypothesis做${E_{in}}$和${E_{out}}$的評估，但是當有多個hypothesis做評估時，若該筆data不是那麼貼近現實(瓶內的球比例，就很有可能被某個hypothesis剛好猜中，產生${E_{in}}$和${E_{out}}$差很多的狀況。

所以現在的目標就改為"找出好的data"，不能所有的hypothesis不能隨便拿一筆data就做${E_{in}}$和${E_{out}}$的評估。

e.g.
![](https://i.imgur.com/nrWDFgG.png)

以上圖為例，有M個hypothesis，5678個data，雖然每個hypothesis中發生bad data機率都很低(Hoeffding)，但是只要有任何一個hypothesis對於data剛好命中機率很低的時候，就會使得${E_{in}}$和${E_{out}}$差很多。所以只要該data對於任何一個hypothesis是bad data，因為很容易選到該hypothesis，對於hypothesis來說就是bad data，唯有對所有hypothesis都是good data，這個data對於hypothesis set來說才是good data。

所以就可以再擴展Hoeffding equality在hypothesis set情況
![](https://i.imgur.com/JTmDPtL.png)

最終公式中M就是hypothesis的數量。

# 修正後流程
![](https://i.imgur.com/7gqeaAt.png)

所以當Hoeffing ineqality很低時，就可以保證${E_{in}}$和${E_{out}}$很接近。

進入接下去的內容前，先看一題對之後內容有幫助的問題
![](https://i.imgur.com/E9F1iyF.png)

因為若一筆data對h1是bad data，代表其data都落於其中一邊(都>x1或是都<x1)，那對於h3而言也會一樣的，所以2是對的，所以真正需要判定bad data的hypothesis只有h1及h2，4正確，當4正確，3就一定正確
![](https://i.imgur.com/ynI8ose.png)

