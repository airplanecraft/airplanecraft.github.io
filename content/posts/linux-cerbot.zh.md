+++
Categories = ["Technology"]
Description = "online analytical processing (or OLAP)"
Tags = ["linux", "cert"]
date = "2020-05-08T14:40:37+08:00"
menu = "linux"
title = "给nginx安装免费的证书"
toc = true
+++

## https vs http
- https比http安全原因是传输的过程中使用了加密，因为https在服务器端使用证书
- 证书的认证需要认证机构，随便一个https的证书阿里巴巴云最便宜的一年要2000多，aws更是贵到天上，所以对于个人程序员来来说最好有免费的证书
- cerbot就是你的选择

## cerbot简介
- cerbot就是Electronic Frontier Foundation (EFF)这个机构给大家发的福利，简单的一句话来说就是给你提供3个月的免费证书，证书到期后继续免费续约，个人网站是用最方便，每三个月更新一次就可以了
  
## cerbot 使用

- cerbot 安装脚本

```
yum -y install yum-utils

yum-config-manager --enable rhui-REGION-rhel-server-extras rhui-REGION-rhel-server-optional

sudo yum install certbot python2-certbot-nginx

sudo certbot --nginx(sudo certbot certonly --nginx)


echo "0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && certbot renew -q" | sudo tee -a /etc/crontab > /dev/null (auto renewal)

```

## cerbot 证书验证 ##

- To confirm that your site is set up properly, visit https://yourwebsite.com/ in your browser and look for the lock icon in the URL bar. If you want to check that you have the top-of-the-line installation, you can head to https://www.ssllabs.com/ssltest/



