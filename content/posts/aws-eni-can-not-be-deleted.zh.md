+++
Categories = ["Technology"]
Description = "eni or eip can not be deleted"
Tags = ["aws", "sap"]
date = "2019-09-02T14:40:37+08:00"
menu = "sap"
title = "eni or eip can not be deleted"
toc = true
+++

- 我几乎删除了所有的service，但是eni就是不让我删除
- 关联的eip也不让删除
- 我去aws社区看了很多帖子最后发现主要原因有两个地方
- efs 使用eni
- nat gatway 使用了eni
- 根据这两点，我去查找，果然有个natgateway 使用eni，删除掉nat gateway后，eni可以删除，eip可以release了
