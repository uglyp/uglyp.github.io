---
layout: blog
istop: false
title: "Referrer Policy"
background-image: https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1577689473&di=7f4d8468b12f66079dda8be6af28d7c8&imgtype=jpg&er=1&src=http%3A%2F%2Fwww.wansion.net%2Fupload%2F201905%2F04%2F201905041150175999.jpg
date:  2019-10-10 22:45:00
category: meta
description: <p>什么是引用策略（Referrer Policy）？引用策略就是从一个文档发出请求时，是否在请求头部定义 Referrer 的设置。</p>
tags:
- meta
- http
- 元数据
- HTML
---

> 一个图片，本站或者外站，都要想办法拿到


# **前言介绍**

近期在一个项目中，需要引用大量外部网站的图片，竟意外的发现在生成环境中没有问题，但在本地的开发环境下，有部分图片无法显示。于是就开启了跨域图片显示的斗争中。

# **名词解释**

什么是引用策略（Referrer Policy）？引用策略就是从一个文档发出请求时，是否在请求头部定义 Referrer 的设置。

目前很多网站的防盗链机制都是用头部定义 Referrer 来判断是否是盗链。其实这个很容易破解，自己在请求时加上 Referrer 头部就行。

在哪些情况下会设置引用头呢？一般来说，加载一个 HTML 页面之后，本 HTML 页面里的 JavaScript 文件，CSS 文件，画面文件都会设置 Referrer。
然后，点击这个 HTML 页面里的链接，跳转其它画面时，也会设置 Referrer。

# **Referrer Policy 的值**

1. 空字符串
2. no-referrer
3. no-referrer-when-downgrade
4. same-origin
5. origin
6. strict-origin
7. origin-when-cross-origin
8. strict-origin-when-cross-origin
9.  unsafe-url


## **空字符串**

默认值：一般浏览器的默认值是 no-referrer-when-downgrade

## **no-referrer**

所有请求不发送 referrer。

## **no-referrer-when-downgrade**

当请求安全级别下降时不发送 referrer。目前，只有一种情况会发生安全级别下降，即从 HTTPS 到 HTTP。HTTPS 到 HTTP 的资源引用和链接跳转都不会发送 referrer。

## **same-origin**

对于同源的链接和引用，会发送referrer，其他的不会。

同源的意思是指同一个域名且同一协议。

如果设置成 same-origin，那么 aaa.com 引用 bbb.com 的资源，不会发送 referrer。子域名也不是同一个域名，aaa.com 引用 test.aaa.com 的资源，不会发送 referrer。

## **origin**

会发送 referrer，但只会发送源信息。源信息包括访问协议和域名。

## **strict-origin**

这个相当于 origin 和 no-referrer-when-downgrade 的 AND 合体。即在安全级别下降时不发送 referrer；安全级别未下降时发送源信息。

> 注意：这个是新加的标准，有些浏览器可能还不支持。

## **origin-when-cross-origin**

这个相当于 origin 和 same-origin 的 OR 合体。同源的链接和引用，会发送完全的 referrer 信息；但非同源链接和引用时，只发送源信息。

## **strict-origin-when-cross-origin**

这个比较复杂，同源的链接和引用，会发送 referrer。安全级别下降时不发送 referrer。其它情况下发送源信息。

> 注意：这个是新加的标准，有些浏览器可能还不支持。

## **unsafe-url**

无论是否发生协议降级，无论是本站链接还是站外链接，统统都发送 Referrer 信息。正如其名，这是最宽松而最不安全的策略。

# **Referrer Policy 的设置方法**

我们可以用以下方法来设置引用策略（Referrer Policy）。

- 通过 http 响应头中的 Referrer-Policy 字段
- 通过 meta 标签，name 为 referrer
- 通过`<a>`、`<area>`、`<img>`、`<iframe>`、`<link>`元素的 `referrerpolicy` 属性。
- 通过`<a>`、`<area>`、`<link>`元素的 `rel=noreferrer` 属性。


## **通过 http 响应头中的 Referrer-Policy 字段**

```
Content-Security-Policy: referrer no-referrer|no-referrer-when-downgrade|origin|origin-when-cross-origin|unsafe-url;
```

Apache 下，设置方法如下：

```
<IfModule mod_headers.c>
  Header set Content-Security-Policy: "referrer=no-referrer"
</IfModule>
```

## **通过 meta 标签，name 为 referrer**

```
<meta name="referrer" content="no-referrer|no-referrer-when-downgrade|origin|origin-when-crossorigin|unsafe-url">
```

## **通过标签的 referrer 属性**

```
<a href="http://example.com" referrer="no-referrer|origin|unsafe-url">xxx</a>
```

## **Referrer 策略还有其他历史遗留的值**


Referrer 策略还有其他历史遗留的值，不过不建议使用。

- never 等价于 no-referrer
- default 等价于 no-referrer-when-downgrade
- always 等价于 unsafe-url


# **小结**

所以在我当前的项目中使用same-origin是最好的选择了，在其他情况下就根据实际应用场景来进行选择即可



链接：[https://www.jianshu.com/p/b12c5b4fd9df](https://www.jianshu.com/p/b12c5b4fd9df)
