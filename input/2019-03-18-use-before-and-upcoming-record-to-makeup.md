---
layout: post
title: 前推后补业务逻辑设计
tags: Software Design
---

用户的权益记录有配量要求，当期不能达标时可以向前借（前补）而超标时可以补足将来不达标的记录（后推）。

## 算法步骤
1. 收集计算是否达标的数据
2. 当期达标->不用向前借
3. 当期不够->向以前的借， 需要计算最后借的一条记录，之前的都是所有可借额度
4. 当期多余->可以补足以前没有达标的。前期有可借额度的记录在补足第一条记录的时候会全部用完，需要计算补足第一条记录时候本条记录借了多少（因为不是全部用完）

## 表记录
需要记录每一期的可借用额度，而且每次有前推后补的时候都要更新使用到的记录
