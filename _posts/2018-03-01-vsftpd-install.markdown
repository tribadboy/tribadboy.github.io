---
layout: post
title: Install and Configure Vsftpd
categories: [devops]
tags: [ftp]
---

<br>
在linux系统上建立ftp服务，并配置虚拟用户和TLS认证。

<br>

_1. 安装 `vsftpd` 和 `db-util`_
>  yum -y install vsftpd db4-util<br>
>  apt-get -y install vsftpd db-util

 <br>

_2. 建立虚拟用户所挂载的系统用户, 并建立虚拟用户根目录_
>  sudo useradd -m -s /bin/bash ftp-user<br>
>  mkdir xxx/xxx
>  chown -R ftp-user:ftp-user xxx/xxx

 <br>

_3. 创建虚拟用户_
>  vim /etc/vsftpd/virtual-user  奇数行为用户名，偶数行为用户名密码
> db_load -T -t hash -f /etc/vsftpd/virtual-user /etc/vsftpd/virtual-user.db

 <br>

_4. 修改pam认证_
>  cp /etc/pam.d/vsftpd /etc/pam.d/vsftpd/vsftpd.backup<br>
>  vim /etc/pam.d/vsftpd<br>

    auth required pam_userdb.so db=/etc/vsftpd/virtual-user
    account required pam_userdb.so db=/etc/vsftpd/virtual-user

 <br>


_5. 修改vsftpd配置文件 /etc/vsftpd/vsftpd.conf 或 /etc/vsftpd.conf_

    ......
    local_enable=YES
    write_enable=YES
    chroot_list_enable=NO
    allow_writeable_chroot=YES
    pam_service_name=vsftpd
    ......
    guest_enable=YES
    user_config_dir=/etc/vsftpd/vu
    ......
    pasv_enable=YES
    pasv_min_port=10000
    pasv_max_port=20000

<br>

_6. 虚拟用户配置_
>  vim mkdir /etc/vsftpd/vu/xxx 每一个虚拟用户创建一个对应的配置文件

    guest_username=ftp-user(对应的系统用户）
    local_root=虚拟用户根目录（需要系统用户相应权限）
    ......
    virtual_use_local_privs=NO
    anon_world_readable_only=NO
    anon_upload_enable=YES
    anon_other_write_enable=YES
    anon_mkdir_write_enable=NO
    (配置虚拟用户操作权限，具体设置可参考网络资料）

<br>

_7. 启动/重启/关闭服务_
>  sudo service start/restart/stop vsftpd

<br>

* 注意防火墙设置，应开通20、21和pasv所用端口，在aws等服务中需要修改安全组配置

<br>

_8. 生成SSL/TLS证书_

    sudo mkdir /etc/ssl/private
    sudo openssl req -x509 -nodes -keyout /etc/ssl/private/vsftpd.pem -out /etc/ssl/private/vsftpd.pem -days 365 -newkey rsa:2048
    ......
    填写证书信息，注意Common Name应为服务器ip地址或域名
    ......
    修改vsftpd配置
    ......
    ssl_enable=YES
    ssl_tlsv1=YES
    ssl_sslv2=NO
    ssl_sslv3=NO
    ......
    rsa_cert_file=/etc/ssl/private/vsftpd.pem
    rsa_private_key_file=/etc/ssl/private/vsftpd.pem
    ......
    allow_anon_ssl=NO
    force_local_data_ssl=YES
    force_local_logins_ssl=YES
    ......
    require_ssl_reuse=NO
    ssl_ciphers=HIGH
    ......
    具体配置可参考网络资料，重启vsftpd服务

<br>

* 参考资料<br>
https://www.cnblogs.com/ilanni/p/4779376.html<br>
http://blog.csdn.net/zhangpfly/article/details/73160346<br>
https://www.cnblogs.com/chenbaoli/p/8195697.html<br>
https://linux.cn/article-8295-1.html?amputm_medium=rss<br>

<br>
