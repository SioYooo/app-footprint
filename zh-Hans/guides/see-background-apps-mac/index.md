---
title: 怎么看 Mac 后台在运行什么
description: 在 Mac 上查看后台活动的四种方法：活动监视器、登录项与扩展、终端里的 launchctl，以及 App Footprint。每种能看到什么、看不到什么。
lang: zh-Hans
locale: zh_CN
date: 2026-10-17
author: SioYoo
home: /zh-Hans/
seo:
  type: Article
alternates:
  en: /guides/see-background-apps-mac/
  zh-Hans: /zh-Hans/guides/see-background-apps-mac/
---

[English]({{ "/guides/see-background-apps-mac/" | relative_url }}) · 简体中文

# 怎么看 Mac 后台在运行什么

**简单说：** 打开「活动监视器」，选「显示 › 所有进程」，按「% CPU」排序，就能看到此刻在运行的所有进程。想知道哪些 App 被允许自己在后台运行，打开「系统设置 › 通用 › 登录项与扩展」。这两处都不会记录一小时前是哪个 App 在忙，要回看，得用能持续记录的工具。

下面四种方法，各自回答的问题不太一样。

## 1. 活动监视器：此刻在运行什么

在「应用程序 › 实用工具」里打开，或者按 ⌘ 空格键输入「活动监视器」。

- 「显示 › 所有进程」会列出所有进程，包括没有窗口的。点「% CPU」或「内存」列标题排序。
- 「显示 › 所有进程，分层显示」能看到谁启动了谁，Helper 会排在它的父进程下面。
- 「能耗」标签页会把同一个 App 的进程归在一起，点 App 名字前的三角形展开。在 Mac 笔记本上，这一页还有一列显示过去 12 小时内（或开机以来）的平均能耗影响。
- 双击一个进程，打开「打开的文件和端口」，最上面几行是程序的路径。路径在 `/Applications/Dropbox.app/` 里面，就说明它是 Dropbox 带来的。

看不到的：「CPU」和「内存」标签页只显示当前。如果 Mac 十分钟前很卡、现在好了，当时占资源的进程可能已经不在列表里了。

## 2. 登录项与扩展：哪些 App 可以在后台运行

打开「系统设置 › 通用 › 登录项与扩展」（macOS 14 上叫「登录项」）。

- 「登录时打开」列出登录时会自动打开的 App。
- 再往下是允许在后台运行、用来同步数据或检查更新的 App。这一节的名称因 macOS 版本而异，较新的系统叫「后台App活动」，还会标出每一项是否正在后台运行。
- 关掉某个 App 的开关，它的后台任务就会停下。在你重新打开之前，这个 App 可能不再同步或更新。

看不到的：这只是一份权限列表，看不出谁用了多少 CPU 和内存。

## 3. 终端：launchctl

macOS 的后台代理由 `launchd` 启动。查看当前用户已载入的任务：

```sh
launchctl list
```

第一列是进程 ID（`-` 表示现在没在运行），最后一列是任务标签。定义这些任务的文件在 `~/Library/LaunchAgents`、`/Library/LaunchAgents` 和 `/Library/LaunchDaemons`。

看不到的：标签是 `com.xxx.yyy` 这样的反向域名，不是 App 名字，也没有资源占用。只读查看是安全的；要停掉某个任务，用 App 自己的设置或者「登录项与扩展」里的开关，不要直接删文件。

## 4. App Footprint：按 App 看后台，还能回看

[App Footprint](https://apps.apple.com/app/app-footprint/id6809305281) 是我做的一个 Mac App。它的「后台」页面列出还有东西在运行的 App：每个 App 有几个组件在跑，主窗口已经关掉、组件还在工作的会标「关闭后仍在运行」。每个 App 都能看到当前 CPU、所有进程合计的内存，以及过去 1 到 24 小时的能耗估算。

![App Footprint 的「后台」页面：Google Chrome、macOS、Adobe、Dropbox、node 和 Microsoft AutoUpdate，每个 App 下有几个后台组件、CPU 和内存]({{ "/assets/background-zh-Hans.jpg" | relative_url }})

*示意图：App 是真实的，数字是为演示编的。*

它按证据判断进程属于哪个 App：进程自己就是那个 App、程序文件在那个 App 的包里，或者父进程属于那个 App。都对不上时，它标「未能确认」，不按名字去猜。记录保存在你的 Mac 上，卡顿过去以后也能回看当时哪些 App 比较忙。

看不到的：合计内存是把各进程加起来，进程之间共享的内存会被重复计算。能耗按 CPU 时间估算，不是实测瓦数。App Store 版读不到温度和风扇转速，可以另外装免费的 [App Footprint Sensors]({{ "/zh-Hans/" | relative_url }}#支持)。

## 用哪个

| 想知道 | 用 |
|---|---|
| 现在谁在占 CPU 或内存 | 活动监视器 |
| 哪些 App 可以自己在后台运行 | 登录项与扩展 |
| 当前用户载入了哪些 launchd 任务 | `launchctl list` |
| 过去几小时哪些 App 在后台忙 | App Footprint |

活动监视器里那一堆「Helper」是什么，见[活动监视器里的 Helper 进程是什么]({{ "/zh-Hans/guides/helper-processes-activity-monitor/" | relative_url }})。

参考：[Apple：在活动监视器中查看能耗](https://support.apple.com/zh-cn/HT203184)、[Apple：登录时自动打开项目](https://support.apple.com/zh-cn/guide/mac-help/mh15189/mac)。
