---
title: smartgit配置beyond compare作为对比工具
author: 陈德强
date: 2019-10-24 18:58:00
categories:
- 优化
tags: 其他
toc: true
top: 
img: /medias/paperimg/c.jpg
summary: smartgit配置beyondCompare对比工具。
---


`Diff`
Select **Edit | Preferences**.
Go to **Tools > Diff Tools**.
Click **Add**.
**File Pattern**: *
Select **External diff tool**.
**Command**: C:\Program Files (x86)\Beyond Compare 4\bcomp.exe  [ 如果是 Linux 参考： “/usr/bin/bcompare” ]
**Arguments**: /readonly /lefttitle="${leftTitle}" /righttitle="${rightTitle}" "${leftFile}" "${rightFile}"

-----

`Merge`
Select **Edit | Preferences**.
Go to **Tools > Conflict Solvers**.
Click **Add**.
**File Pattern**: *
Select **External Conflict Solver**.
**Command**: C:\Program Files (x86)\Beyond Compare 4\bcomp.exe
**Arguments**: "${leftFile}" "${rightFile}" "${baseFile}" /mergeoutput="${mergedFile}"


参考链接：
https://blog.csdn.net/u010620626/article/details/78818687