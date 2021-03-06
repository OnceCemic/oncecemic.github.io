---
layout: page
title: 手牌
permalink: /docs/libsaki/hand/
---

## 定义

「手牌」是个歧义词，有时包括副露，有时不包括。
在 Libsaki 中，「手牌」指代包括副露的广义的手牌。

代表手牌的类是`Hand`，定义在`libsaki/form/hand.h`。

`Hand`主要由三部分组成：*closed*, *barks*, *drawn*。
- *Closed* 包含 1, 4, 7, 10, 或 13 张牌，
  代表不包括副露及自摸牌的纯手牌部分
- *Barks* 用于记录鸣牌与暗杠
- *Drawn* 代表摸上来的牌

<br />

## 常用方法

通过`closed()`或`barks()`方法可以随时取得Closed或Barks部分。

通过`hasDrawn()`方法可判断当前是否有自摸牌（是否轮到自己出牌）。
当`hasDrawn()`返回`true`时，可通过`drawn()`方法读取当前的自摸牌；
当`hasDrawn()`返回`false`时，调用`drawn()`的后果是未定义的。

`Hand`有一堆检测当前是否可鸣牌的方法，如`canPon()`；
也有一堆实际执行鸣牌的方法，如`pon()`。
这些方法的作用都很直白。

与`TileCount`类似地，`Hand`也有一系列向听数和有效牌计算方法。
计算手牌的向听数或有效牌时，直接使用这些方法会更方便——
它们会自动将当前副露数、是否有自摸牌这些因素考虑进去。

`Hand`也支持预计算一个行动以后的向听数和有效牌，
也就是「切了这张就是x向听，听y种……」这种操作。
这些方法都是以`peek`开头的，
例如`peekSwap`就是预计算手切某一张牌以后的状况。

```
// 计算「切掉1p以后的向听数」
int s = hand.peekSwap(1_p, &Hand::step);
```

`peek`系列的东西是用变长参数的模板函数和指向成员函数的指针实现的。
除了向听数和有效牌以外，其实任何`Hand`的方法都能成为预测对象。

