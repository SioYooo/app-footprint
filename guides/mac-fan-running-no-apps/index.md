---
title: Mac fan running with no apps open? How to find what's using the CPU
description: When your Mac's fan spins up or it gets warm with nothing open, something is still using the CPU. How to find it in Activity Monitor, the usual system causes, and what kernel_task means.
lang: en
locale: en_US
date: 2026-10-17
author: SioYoo
seo:
  type: Article
alternates:
  en: /guides/mac-fan-running-no-apps/
  zh-Hans: /zh-Hans/guides/mac-fan-running-no-apps/
---

English · [简体中文]({{ "/zh-Hans/guides/mac-fan-running-no-apps/" | relative_url }})

# Mac fan running with no apps open? How to find what's using the CPU

**Short answer:** the fan speeds up because something is still working. Open Activity Monitor, choose **View › All Processes**, and sort the CPU tab by **% CPU**. The process at the top is usually either macOS doing housekeeping (indexing, photo analysis, backups, syncing) or the background part of an app you thought you had closed.

MacBook Air models with Apple silicon have no fan; when they get warm they slow down instead, and the same steps apply.

## Step 1: find the process

In Activity Monitor, **View › All Processes**, CPU tab, click **% CPU** so the busiest process is at the top. Watch it for a minute; short spikes are normal.

## Step 2: is it macOS housekeeping?

These are macOS processes and are usually busy for a reason that ends on its own:

| Process | What it is doing |
|---|---|
| `mds`, `mds_stores`, `mdworker` | Spotlight indexing files, typically after a macOS update, a migration or a large copy |
| `photoanalysisd`, `mediaanalysisd` | Photos analysing your library |
| `backupd` | A Time Machine backup |
| `bird`, `cloudd` | iCloud Drive syncing |
| `softwareupdated` | Downloading or preparing a macOS update |

If it's one of these, the simplest thing is to keep the Mac on power and let it finish.

## Step 3: is it an app you closed?

If the top entry is a Helper, an updater or a sync service, see which app it belongs to (double-click it, **Open Files and Ports**, and look at the path). Then quit that app with ⌘Q. If it's a background service you don't need, turn the app's switch off in **System Settings › General › Login Items & Extensions**; the app may stop syncing or updating until you turn it back on. [What are the Helper processes in Activity Monitor?]({{ "/guides/helper-processes-activity-monitor/" | relative_url }}) explains the common names.

## What kernel_task means

A high `kernel_task` is not the cause. Apple explains that it is macOS keeping the processor from overheating by limiting how much CPU other processes get; you may hear more fan noise while it happens. Look for the other processes using a lot of CPU, as in Step 1. ([Apple: If kernel_task is using a large percentage of your Mac CPU](https://support.apple.com/102172))

## When it already stopped

The hard case is when the fan was loud ten minutes ago and is quiet now: Activity Monitor only shows the present. [App Footprint](https://apps.apple.com/app/app-footprint/id6809305281), which I make, keeps a record of recent activity on your Mac, so you can look back at which apps were busy around that time and see the possible causes with the evidence for and against each. It does not cool your Mac or change fan speeds, and the App Store version can't read temperatures or fan speeds at all; the optional, free [App Footprint Sensors]({{ "/" | relative_url }}#support) adds those readings.

Related: [How to see what's running in the background on your Mac]({{ "/guides/see-background-apps-mac/" | relative_url }}).
