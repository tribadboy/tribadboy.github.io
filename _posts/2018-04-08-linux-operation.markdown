---
layout: post
title: Linux Operation
categories: [devops]
tags: [linux]
---

<br>
一些简单的linux操作

<br>

_1. linux 命令行_

    光标跳转到一行的开始： Crtl + a
    光标跳转到一行的末尾： Crtl + e
    清除光标之前的字符： Crtl + u
    清除光标之后的字符： Crtl + k

<br>

_2. vim_

    光标方向键： h j k l
    插入字符： i o
    翻页： Crtl + f / Crtl + b
    光标移到行的开头或结尾： ^ / $
    从开头开始搜索关键词： /key
    从末尾开始搜索关键词： ?key
    搜索下一个关键词： n
    搜索上一个关键词： Shift + n

 <br>
 
 _3. crontab_
 
     crontab -l： 列出当前权限下所有定时任务
     crontab -e： 修改当前权限下定时任务
     
     eg:
     * * * * *   cd xx/xx/xx && sh xx.sh >> xx.log
     （分 时 日 月 星期）
     
     注意在用户权限和root权限下，可能由于依赖环境（PATH路径）
     不一致造成定时任务出现问题
  
  <br>
  
   _4. 其他_
   
      
       文本检索与替换:
       
       eg:    "2019-01-01T00:00:00:00.000Z"  => 
              "2019-01-01 00:00:00"
       
       SEARCH="\([0-9]\{4\}-[0-9]\{2\}-[0-9]\{2\}\)\(T\)\([0-9]\{2\}:[0-9]\{2\}:[0-9]\{2\}\)\([0-9\.]\{4\}\)\(Z\)"
       REPLACE="\1 \3"
       cat $from_file | sed -e "s/$SEARCH/$REPLACE/g" > $to_file
 
  <br>

 _**To Be Continued**_ ......
