---
date: "2026-04-09T15:50:00+08:00"
title: "NVIDIA,FKU"
tags:
  - "运维"
  - "ARCH"
  - "NVIDIA"
  - "驱动"
keywords:
  - "NVIDIA"
  - "ARCH"
---

## 可能是NVIDIA驱动的问题导致的问题

NVIDIA驱动实在是太可怕了。

我的环境：4060m, arch，kde，wayland。

> 从debian换arch不是因为想当男娘。debian下会导致我的笔记本的几个传感器完全不工作，没法调笔记本的性能模式，软件版本太落后了。同时，我的主力开发转移到mac了，所以所谓稳定性对我来说不再重要了，且在debian下甚至没有arch稳定是hyw。

> 虽然用N卡，但是我还是换到了wayland,单纯是发现wayland现在是usable的状态，不会跟我两年前装的时候出现什么花屏等等原因。我都用arch了当然要用最新最激进的。

> 现在nvidia驱动在arch的仓库里直接用的是nvidia-open,我用的是nvidia-open-dkms。

> 相较于debain,linux headers就一个包会直接跟随普通的升级自己升级，挺好的，我喜欢这样。

### 黑屏，花屏
不用想直接重装。

---
### 找不到音频输出设备，音频输出有回音
原因没找，但是重装nvidia驱动即可恢复。

---
### 待补充