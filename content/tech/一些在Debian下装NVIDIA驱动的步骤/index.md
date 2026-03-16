---
title: "一些在Debian下装NVIDIA驱动的步骤"
date: "2026-03-16T20:50:00+08:00"
tags:
    - "技术"
categories:
    - "技术"
---

最近在电脑上装了N遍Nvidia驱动，目前的安装虽然已经非常方便了，但是后续还是会有一些问题。

对于安装nvidia驱动，这里不再叙述，网上多得很，主要步骤为：


1. 禁用nouveau驱动
2. 在debian下启用non-free源
3. 安装一些依赖，大概有 `dkms build-xxxx(我给忘了) linux-headers-xxxx`
4. 然后直接`apt install nvidia-driver`即可

----
但是后续会有什么问题呢，在我自己的环境`Debian 13`, `kde`下呈现为`apt full-upgrade`之后导致外界屏幕不亮。
其实本质原因是nvidia driver在内核加载的时候失败了，在开机的时候就能看见刚开始加载的时候有三四条红色的error,就是那个刷屏的界面。之所以用外屏这个特征来表述是因为这个现象最明显。

更深一点的原因是因为更新了linux 内核，但是没有自己安装header包，再深一点我也没管了

解决办法就是出现问题的时候再安装header: `sudo apt install linux-headers-$(uname -r)`即可，没有其他步骤需要做。然后内核模块就可以成功加载。dkms之后就会自己处理其他事情（大概是吧）。