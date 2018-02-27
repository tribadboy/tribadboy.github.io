---
layout: post
title: Multiple Github Mangement
categories: [devops]
tags: [github]
---

<br>
多个github账户时，账户管理和密钥的简单配置。

<br>

* **_Step 1 建立新的 user （email）的 `ssh` 密钥_**
>  #bash ssh-keygen -t rsa -C "xxx@abc.com"<br>
>  其中，save file 指定到对应位置

 <br>

* **_Step 2 新生成的密钥（私钥）添加到ssh agent中_**
> #bash ssh-add ~/.ssh/xxx/xxx.rsa<br>
> 如果重启终端后失效，可以将其添加到 .bash_profile 中

<br>

* **_Step 3 新生成的密钥（公钥）rsa.pub 填写到对应的github账号的项目设置中_**

<br>



