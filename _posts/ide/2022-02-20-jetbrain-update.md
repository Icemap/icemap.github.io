---
layout: single
title:  "升级JetBrain系的IDE，升级后闪退"
date:   2022-02-20 02:22:51 +0800
categories:
  - ide
tags:
  - JetBrain
  - IDE
  - 闪退
---

今天，重新搞了一下自己的电脑，将JetBrain系的IDE整个升级了一下，包含：

- IntelliJ IDEA
- PyCharm
- Goland
- Clion

发现升级完都打不开了，找到日志，查看了一下，发现是2019版本到2021版本跨度太大，配置不能兼容了。`垃圾JetBrain`

需要继续使用的话，要删除旧版本的配置了，记录一下删除的语句, 以PyCharm为例，其他IDE自行替换内容中的名称，这个不敢做自动化：

```bash
# 删除旧版配置
rm -rf ~/Library/Preferences/PyCharm*
rm -rf ~/Library/Caches/PyCharm*
rm -rf ~/Library/Application\ Support/PyCharm*
rm -rf ~/Library/Logs/PyCharm*

# 删除新版配置
rm -rf ~/Library/Preferences/JetBrains/PyCharm*
rm -rf ~/Library/Caches/JetBrains/PyCharm*
rm -rf ~/Library/Application\ Support/JetBrains/PyCharm*
rm -rf ~/Library/Logs/JetBrains/PyCharm*
```

删除完毕后，即可打开，但是之前的IDE配置也没了……