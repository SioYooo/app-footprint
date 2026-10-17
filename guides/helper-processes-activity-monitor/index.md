---
title: What are the Helper processes in Activity Monitor?
description: Why Chrome, Slack, Adobe and other apps show up in Activity Monitor as many Helper, updater and sync processes, how to tell which app one belongs to, and when it makes sense to quit one.
lang: en
locale: en_US
date: 2026-10-17
author: SioYoo
seo:
  type: Article
alternates:
  en: /guides/helper-processes-activity-monitor/
  zh-Hans: /zh-Hans/guides/helper-processes-activity-monitor/
---

English · [简体中文]({{ "/zh-Hans/guides/helper-processes-activity-monitor/" | relative_url }})

# What are the Helper processes in Activity Monitor?

**Short answer:** they are parts of an app that run as separate processes. Browsers and apps built on the same engine split pages, graphics and networking into Helper processes, so one crashed page doesn't take the whole app down. Other helpers are updaters and sync services that keep running after you close the app's window. They are normal. What matters is whether one keeps using a lot of CPU or memory for a long time.

## The common kinds

**Browser and web-view helpers.** Chrome shows up as `Google Chrome Helper (Renderer)`, `Google Chrome Helper (GPU)` and similar. Pages and extensions run in renderer processes, so a few open tabs can mean a dozen helpers. Apps built with Electron, such as Slack or Visual Studio Code, do the same thing under their own names (`Slack Helper (Renderer)`, `Code Helper (Renderer)`). Safari's pages also run in separate processes, listed by website or as Safari Web Content.

**Updaters.** `Microsoft AutoUpdate` checks for Office updates; Google and Adobe ship their own update agents. They usually start at login and run on a schedule whether or not the app is open.

**Sync and background services.** Cloud drives and suites keep services running so files stay in sync. Adobe Creative Cloud, for example, runs processes such as `Adobe Desktop Service`, `AdobeIPCBroker`, `Core Sync` and `CCXProcess`.

## Why they keep running after you close the window

On a Mac, closing an app's last window usually leaves the app open; ⌘Q quits it. And updaters and sync services are separate programs, started by `launchd` when you log in, so quitting the main app doesn't stop them.

## How to tell which app a helper belongs to

In Activity Monitor:

1. Double-click the process and open **Open Files and Ports**. The first lines show the program's path. Anything under `/Applications/Google Chrome.app/` came with Chrome.
2. Choose **View › All Processes, Hierarchically** to see the parent process.
3. On the **Energy** tab, click the triangle next to an app to see the processes macOS counts toward it.

[App Footprint](https://apps.apple.com/app/app-footprint/id6809305281), which I make, does this grouping for you and keeps a history, so you can see which app's components were busy over the last 1 to 24 hours. When it can't tell from the process's app, its app bundle or its parent which app a process belongs to, it labels it **Not identified** rather than guessing from the name.

## Should you quit them?

- If a helper uses a lot of CPU for minutes at a time, quit the app it belongs to with ⌘Q and open it again. Quitting a single renderer helper closes the page it was showing.
- If you don't want an app's updater or sync service in the background at all, turn its switch off in **System Settings › General › Login Items & Extensions**. The app may stop updating or syncing until you turn it back on.
- Leave processes you can't identify alone, especially ones whose path is under `/System/`, or under `/usr/` but not `/usr/local/`. Those belong to macOS.

Related: [How to see what's running in the background on your Mac]({{ "/guides/see-background-apps-mac/" | relative_url }}).
