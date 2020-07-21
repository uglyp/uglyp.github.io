---
layout: blog
istop: true
title: "移动web之viewport详解"
background-image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577689473&di=7f4d8468b12f66079dda8be6af28d7c8&imgtype=jpg&er=1&src=http%3A%2F%2Fwww.wansion.net%2Fupload%2F201905%2F04%2F201905041150175999.jpg
date:  2019-12-05 22:45:00
category: meta
description: <p>移动web之viewport详解</p>
tags:
- meta
- http
- 元数据
- HTML
- viewport
- 移动端
---

说起移动web开发，别的可以不讲，但不得不提viewport这位兄台，viewport是何方神圣？他从何而来又将去往何处？下面请跟随我们的镜头去一探究竟！

# **viewport来自何方**：
由于社会主义提倡节俭，我们的手机屏幕通常较小，如果一个西方资本主义大网页直接在屏幕上渲染出来会显得稀奇古怪，所以需要先在一个较大的viewport中进行布局，

然后再缩放至我们的手机屏幕大小，除了个别不和谐的浏览器外，通常浏览器都会把viewport的宽度统一设为980。

但浏览器毕竟只是浏览器，只解决了一般的问题，要是你用浏览打开一张很小的图片比如200x200，你会发现它会定位在屏幕的左上角，

而不是完美的在屏幕上呈现。而这时一位国际友人apple提出了meta viewport来解决这个问题，大家试用了几个疗程之后，发现效果还不错，都开始吃这个药。


# **viewport证件照**：

```
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

通常viewport就长这个样子，相信大家在很多地方也见过他，但是为了把他装扮的更好看一点，我们可以为他添置下面这些装饰：

>width 设置viewport 的宽度，为一个正整数，或字符串"device-width"
>initial-scale 设置页面的初始缩放值，为一个数字，可以带小数
>minimum-scale 允许用户的最小缩放值，为一个数字，可以带小数
>maximum-scale 允许用户的最大缩放值，为一个数字，可以带小数
>height 设置viewport 的高度，这个属性对我们并不重要，很少使用
>user-scalable 是否允许用户进行缩放，值为"no"或"yes", no 代表不允许，yes代表允许

viewport最重要的两个装饰当然是width和initial-scale了，这就好比上衣和裤子一样。width用于设置viewport的宽度，

通过document.documentElement.clientWidth可获得viewport的宽度，initial-sacle用于设置页面的缩放，即在viewport中布局后缩放到屏幕上显示的倍率。 

# **走进viewport的生活：**

为了更了解viewport，下面我们跟随镜头走进viewport的生活！

注：测试环境：chrome 53.0.2785.143 m设备模拟器 Galaxy s5    图片大小：600x400


- **裸奔**


当content=" "（相当于不引入viewport）时，viewport就会开始裸奔，浏览器拿他没办法，只能使用默认的width=980（或其他），

这时候浏览器先将网页在980的viewport中进行“渲染”，然后再将整个网页（包括不在视窗中的部分）缩放成屏幕大小（逻辑像素），当网页内容较小时就会出现大片的空白。

![](https://img.mukewang.com/57ff8a5a0001f77301950335.jpg) 

- **只穿上衣**  


为了节约洗衣粉，viewport常常穿个上衣，只指定width就出门了。这时候浏览器会在指定宽度的viewport中布局网页，然后自动给他穿上合适的裤子，

调整initial-scale，将网页缩放到屏幕宽度（这时initial-scale=屏幕宽度/viewport网页实际宽度）。

- **只穿裤子**


夏天到了，又到了交费的季节，由于没钱交电费，viewport只好脱了上衣来散热，只指定initial-scale了。

按正常情况来想，这时浏览器应该默认给他一件980元的外衣才对。可事实上就像在固定width让initial-scale自适应屏幕一样，这里会让width自适应，以在屏幕上较好的显示。

具体的width=屏幕宽度/initial-scale，在viewport中布局完后，在以initial-scale缩放到屏幕上显示。

注意，由于不能调整initial-scale，在viewport实际网页宽度大于width时仍不会适应屏幕，会产生滚动条。

![](https://img.mukewang.com/57ff8da500019fdf01950342.png)

- **套装出门**


夏天已经到了，冬天还会远吗？viewport终于心不甘情不愿的穿上了套装。这时浏览器的处理就很简单了，直接在宽度为width的viewport中布局，

然后以initial-scale进行缩放，最后显示在屏幕上，是什么样就是什么样了。


# **结语**

说了这么多，那我们为什么总是使用`<meta name="viewport" content="width=device-width, initial-scale=1.0">`呢？
解析这个语句发现，它实现的功能就是让viewport的宽度为设备宽度，在viewport中布局完成后显示在屏幕上时不缩放，
即布局是怎样，显示就是怎样。这样我们在制作网页时只需要直接针对手机屏幕大小进行设计，不用考虑那么多复杂的问题，大大节约我们的脑细胞。




链接：[https://www.imooc.com/article/13605](https://www.imooc.com/article/13605)








