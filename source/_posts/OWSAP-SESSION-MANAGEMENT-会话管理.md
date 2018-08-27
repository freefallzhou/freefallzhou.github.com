---
title: '[OWSAP] SESSION MANAGEMENT(会话管理)'
date: 2018-08-22 12:34:37
tags:
- OWSAP
- SESSION
---
<!-- markdown-toc start - Don't edit this section. Run M-x markdown-toc-refresh-toc -->
**Table of Contents**

- [简介](#简介)
    - [授权, 会话管理, 访问控制](#授权-会话管理-访问控制)
- [Session Id 的性质](#session-id-的性质)
    - [Session Id 的名称](#session-id-的名称)
    - [Session Id 的长度](#session-id-的长度)
    - [Session Id 的信息熵](#session-id-的信息熵)
    - [Session Id 的value](#session-id-的value)
- [会话管理的实现](#会话管理的实现)
    - [Built-in 会话管理机制的实现](#built-in-会话管理机制的实现)

<!-- markdown-toc end -->

# 简介
## 授权, 会话管理, 访问控制
一个web session指的是:

>同一个用户的一系列http请求与响应。

业务中经常需要对同一个用户在一系列操作中保存用户信息，状态的需求。比如访问权限，地域设置等。

我们比较关注的是访问权限的控制。http协议是一个无状态协议RFC2616 [5]

>每一个请求与响应的状态是独立的

所以我们需要保持用户的信息，这里引入会话管理的概念，会话管理的简图如下：

![session management](fig1.png)

一般情况下，用户在得到访问授权之后，会得到一个session ID, 或者token, 这个标记会记录在之后的http请求交互中, 这个标识将用于用户之后的访问行为中标识用户已经拥有权限的标记。这个标记在之后的鉴权中，效果等同于应用的最高安全等级验证方式（密码，人脸，短信，视网膜等等）。由此这样一个标记的安全性是非常关键的。

>在这样一个无状态的协议中，我们不得不将权限控制分成如上所述的三个部分。另外常用的web框架中对这三个模块的定义也不严格。所以这部分的工作一般是开发者手动实现的。所以实现一个安全的会话管理是很有挑战的。

对于session ID的泄露, 盗取, 预测, 暴力破解, 修复等等行为，都能导致session劫持。而这种攻击的后果是攻击者将获取受害者在业务中的所有权限。

而一般的常见攻击行为大致可以分为两类，针对性攻击，以及无差别攻击。

# Session Id 的性质 #

为了保持对用户操作序列的感知，以及对用户权限的感知。应用一般会在session创建的时候颁布一个session的唯一标识（Session Id 或者 token）,这个标识会在用户与业务端交互中在前后端传递。Session Id 是一个 name = value 的属性。

## Session Id 的名称 ##
Session Id 的名称应当不包含任何信息不必要的信息。大部分的web框架会有默认session id名称规则，从session id的名称，可以反推出网站所用框架，这样会给攻击者提供关键信息。

## Session Id 的长度 ##
Session Id 的长度至少要128bit来防止暴力破解。

## Session Id 的信息熵 ##
Session Id 的信息熵至少64bit。并且要足够随机，以预防猜测破解。

## Session Id 的value ##
Session Id 应当仅仅是一个标识，不包含任何敏感信息，以预防信息泄露以及利用已经泄露的敏感信息来生成session id。

# 会话管理的实现 #

会话管理的实现决定用户与应用交换以及会话信息的机制。在http协议中有多种机制可以用来保持会话状态，例如 cookie， url_rewrite, url_param(GET), url_body(POST), html中的隐藏信息，或者一些http的header。

Session Id 的交换机制，需要满足对一般token的性能要求。比较推荐的会话机制是使用cookie
[(RFCs 2109 & 2965 & 6265 [1])](https://tools.ietf.org/html/rfc6265)

一些容易暴露Session Id的机制，比如url参数，会暴露session id。比如浏览器书签等方式，这样导致的session id泄露会导致其他威胁。

## Built-in 会话管理机制的实现 ##



[OWSAP Session Management Cheat Sheet](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Introduction)
