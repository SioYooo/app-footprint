---
title: Mac 没开 App 风扇也在转？怎么查是谁在占 CPU
description: Mac 什么都没开，风扇却在转或者机身发热，说明还有东西在用 CPU。怎么在活动监视器里找到它、常见的系统原因，以及 kernel_task 是什么意思。
lang: zh-Hans
locale: zh_CN
date: 2026-10-17
author: SioYoo
home: /zh-Hans/
seo:
  type: Article
alternates:
  en: /guides/mac-fan-running-no-apps/
  zh-Hans: /zh-Hans/guides/mac-fan-running-no-apps/
---

[English]({{ "/guides/mac-fan-running-no-apps/" | relative_url }}) · 简体中文

# Mac 没开 App 风扇也在转？怎么查是谁在占 CPU

**简单说：** 风扇转起来，是因为还有东西在干活。打开「活动监视器」，选「显示 › 所有进程」，在「CPU」标签页按「% CPU」排序。排在最上面的，通常要么是 macOS 在做例行工作（建索引、分析照片、备份、同步），要么是你以为已经关掉的某个 App 的后台部分。

使用 Apple 芯片的 MacBook Air 没有风扇，热了会降速，查法一样。

## 第 1 步：找到是哪个进程

活动监视器里选「显示 › 所有进程」，在「CPU」标签页点「% CPU」，最忙的排到最上面。看一分钟，短暂的跳高是正常的。

## 第 2 步：是不是 macOS 的例行工作

下面这些是 macOS 自己的进程，忙一阵通常会自己结束：

| 进程 | 在做什么 |
|---|---|
| `mds`、`mds_stores`、`mdworker` | 聚焦（Spotlight）在建索引，常见于系统更新、迁移数据或复制大量文件之后 |
| `photoanalysisd`、`mediaanalysisd` | 「照片」在分析图库 |
| `backupd` | 时间机器在备份 |
| `bird`、`cloudd` | iCloud 云盘在同步 |
| `softwareupdated` | 在下载或准备 macOS 更新 |

如果是这些，最省事的做法是接上电源，让它做完。

## 第 3 步：是不是关掉的 App 还在跑

如果最上面是某个 Helper、更新程序或同步服务，先看它属于哪个 App（双击它，打开「打开的文件和端口」看路径），再用 ⌘Q 退出那个 App。如果是用不着的后台服务，就到「系统设置 › 通用 › 登录项与扩展」里关掉那个 App 的开关；在你重新打开之前，它可能不再同步或更新。常见的名字见[活动监视器里的 Helper 进程是什么]({{ "/zh-Hans/guides/helper-processes-activity-monitor/" | relative_url }})。

## kernel_task 很高是什么意思

`kernel_task` 占得多，它本身不是原因。Apple 的说明是：这是 macOS 在限制其他进程能用的 CPU，防止处理器过热，这时风扇声可能会变大。按第 1 步去找其他占用 CPU 多的进程。（[Apple：如果 kernel_task 占用了很高的 Mac CPU 百分比](https://support.apple.com/zh-cn/102172)）

## 已经停了，想回头查

最难的情况是：风扇十分钟前还很响，现在安静了，而活动监视器只显示当前。我做的 [App Footprint](https://apps.apple.com/app/app-footprint/id6809305281) 会在本机记录最近的活动，可以回看那段时间哪些 App 比较忙，并列出可能的原因和支持、反对的证据。它不会给 Mac 降温，也不调风扇；App Store 版完全读不到温度和风扇转速，要看这些读数，可以另外装免费的 [App Footprint Sensors]({{ "/zh-Hans/" | relative_url }}#支持)。

相关：[怎么看 Mac 后台在运行什么]({{ "/zh-Hans/guides/see-background-apps-mac/" | relative_url }})。
