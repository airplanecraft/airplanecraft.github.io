+++
Categories = ["Technology"]
Description = ""
Tags = ["linux","tmpfiles"]
date = "2022-05-07T10:30:37+08:00"
menu = "linux"
title = "linux 临时文件的管理"
toc = true
+++

## Centos临时目录的系统管理
  Linux产生大量的临时文件和目录，例如/tmp、/run 。RHEL7或者CentOS7中，systemd提供了一个结构化的可配置方法来管理临时文件和目录，即systemd-tmpfiles，可以创建、删除和管理临时文件的服务。旧版本系统使用watchtmp+cron来共同实现管理临时文件。

  systemd启动后，其中一个启动的服务单元是systemd-tmpfiles-setup，该服务的命令为:systemd-tmpfiles --creat/--clean  *conf  , *conf是可选的，不写默认是使用所有配置文件

## 配置文件，优先级从上到下。

    /etc/tmpfiles.d/*conf  ，管理员可修改的配置文件

    /run/tmpfiles.d/*conf ，daemon应用程序自己管理的配置文件，不建议更改

    /usr/lib/tmpfiles.d/*conf，rpm软件安装时，自动更新的配置文件，不能更改

## 定期清理timer

    systemd定时器单元会按固定间隔调用systemd-tmpfiles --clean  。

    systemctl status systemd-tmpfiles-clean.timer 查看timer状态。

    systemctl cat systemd-tmpfiles-clean.timer  查看timer具体内容，也可以进入/usr/lib/systemd/system目录后使用more  systemd-tmpfiles-clean.timer 来查看。

    从timer具体内容可以知道系统启动15分钟后和每天会运行一次systemd-tmpfiles --clean.那么timer定时运行的是哪些服务呢？ 是在systemd-tmpfiles-clean.service里面定义的：

    systemctl cat systemd-tmpfiles-clean.service可以查看。
 