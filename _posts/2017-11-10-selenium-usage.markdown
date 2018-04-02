---
layout: post
title: Simple Usage of Selenium
categories: [devops]
tags: [html]
---

<br>
使用selenium模拟用户操作，完成自动化测试或维护脚本。

<br>

#### **_Preparation_**

    # 虚拟显示
    `from pyvirtualdisplay import Display`  
    `display = Display(visible=0, size=(2000,1500))`  
    `display.start()`  

 *当 `size` 指定过小时，程序可能会找不到 html 页面中的元素*

    `from selenium import webdriver`  
    `import time`  
    `browser = webdriver.Firefox()`
 
 <br>

#### **_Usage_**

    # 登录特定页面
    `url = https://xxx.xxx.com`
    `browser.get(url)`
    # 根据id查找指定元素
    `browser.find_element_by_id('xxx')`
    `browser.find_element_by_id('xxx').clear()`
    `browser.find_element_by_id('xxx').send_keys('xxx')`
    `browser.find_element_by_id('xxx').click()`
    `time.sleep(5)`

 *通过查找用户名与密码的元素 `id` 可以进行模拟登录，登录前登录后可以控制 `time.sleep(5)` 避免操作延时*

     # 检查当前页面位置
     `success_url = https://xxx.xxx.com`
     `if browser.current_url != success_url`
         `raise Exception('xxxxx')`
     # 可以通过模糊匹配方式查找没有 id 的元素
     `image = browser.find_element_by_xpath("//img[contains(@src, 'a.png')]")`
     `time.sleep(5)`
     `image.click()`
     `time.sleep(5)`

 *尝试查找 `<img src=xx[a.png]xx>` , 当找不到该元素时，会报异常，可以根据是否抛出异常判断是否找到了该元素（a.png)*

    # 结束该浏览器
    `browser.close()`

*在有些服务器上浏览器无法正常关闭，会导致线程永久占用资源，当反复执行类似脚本时会导致资源穷尽而宕机，可以通过定时清理线程资源而避免*

    #!/bin/bash
    proc_name="Xvfb|geckodriver"
    name_suffix="\>"
    proc_id=`ps -ef|grep -E ${proc_name}${name_suffix}|grep -v "grep"|awk '{print $2}'`
    echo ${proc_id}
    if [[ -z $proc_id ]];then
        echo "The task is not running ! "
    else
         echo ${proc_name}" pid:"
         echo ${proc_id[@]}
         echo "------kill the task!------"
         for id in ${proc_id[*]}
         do
           echo ${id}
           thread=`ps -mp ${id}|wc -l`
           echo "threads number: "${thread}
           kill -9 ${id}
           if [ $? -eq 0 ];then
                echo "task is killed ..."
           else
                echo "kill task failed "
           fi
         done
    fi

*上述清理脚本源自网络资源*

<br>



