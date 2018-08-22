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

[OWSAP Session Management Cheat Sheet](https://www.owasp.org/index.php/Session_Management_Cheat_Sheet#Introduction)
