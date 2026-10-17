---
title: How to see what's running in the background on your Mac
description: Four ways to check background activity on a Mac, what each one shows and what it leaves out. Activity Monitor, Login Items & Extensions, launchctl in Terminal, and App Footprint.
lang: en
locale: en_US
date: 2026-10-17
author: SioYoo
seo:
  type: Article
alternates:
  en: /guides/see-background-apps-mac/
  zh-Hans: /zh-Hans/guides/see-background-apps-mac/
---

English · [简体中文]({{ "/zh-Hans/guides/see-background-apps-mac/" | relative_url }})

# How to see what's running in the background on your Mac

**Short answer:** open Activity Monitor, choose **View › All Processes** and sort by **% CPU**. That shows everything running right now. To see which apps are allowed to start background work on their own, open **System Settings › General › Login Items & Extensions**. Neither keeps a history of which app was busy an hour ago; for that you need something that records over time.

Each method below answers a slightly different question.

## 1. Activity Monitor: what is running right now

Open it from **Applications › Utilities**, or press ⌘-Space and type Activity Monitor.

- **View › All Processes** lists every process, including ones without a window. Click the **% CPU** or **Memory** column to sort.
- **View › All Processes, Hierarchically** shows which process started which, so a helper appears under its parent.
- The **Energy** tab groups an app's processes under the app (click the triangle next to the app's name). On Mac laptops its **12 hr Power** column shows the average energy impact over the last 12 hours, or since the Mac started.
- Double-click a process and open **Open Files and Ports**. The first lines show the program's path. A path inside `/Applications/Dropbox.app/` tells you the process came with Dropbox.

What it leaves out: the CPU and Memory tabs show the current moment. If your Mac was slow ten minutes ago and is fine now, the process responsible may already be gone from the list.

## 2. Login Items & Extensions: which apps may run in the background

Open **System Settings › General › Login Items & Extensions** (called **Login Items** on macOS 14).

- **Open at Login** lists apps that open when you log in.
- Further down is the list of apps allowed to run in the background to sync data or check for updates. The section is called **Allow in the Background** on macOS 14 and 15 and **Background App Activity** on newer versions, which also say whether each item is running.
- Turning an app's switch off stops its background tasks. The app may stop syncing or updating until you turn it back on.

What it leaves out: this is a list of permissions. It does not tell you how much CPU or memory any of them uses.

## 3. Terminal: launchctl

Background agents on macOS are started by `launchd`. To see the ones loaded for your user account:

```sh
launchctl list
```

The first column is the process ID (a `-` means the job is not running right now), the last column is the job's label. The files that define these jobs live in `~/Library/LaunchAgents`, `/Library/LaunchAgents` and `/Library/LaunchDaemons`.

What it leaves out: labels are reverse-DNS names, not app names, and there is no usage information. Reading the list is safe; to stop a job, use the app's own settings or the switch in Login Items & Extensions rather than deleting files.

## 4. App Footprint: background activity by app, with history

[App Footprint](https://apps.apple.com/app/app-footprint/id6809305281) is a Mac app I make. Its **Background** page lists the apps that still have something running, with how many components each one has and a **While closed** badge on apps whose main window is gone but whose components kept working. For each app it shows current CPU, the combined memory of its processes and an energy estimate for the last 1 to 24 hours.

![The Background page in App Footprint, listing Google Chrome, macOS, Adobe, Dropbox, node and Microsoft AutoUpdate with their components, CPU and memory]({{ "/assets/background-en.jpg" | relative_url }})

It decides which app a process belongs to from evidence: the process's own app, the app bundle its program lives in, or a parent process that belongs to an app. When none of these holds, it marks the row **Not identified** instead of guessing from the name. It keeps a record on your Mac, so after a slowdown you can look back at which apps were busy at the time.

What it leaves out: combined memory adds up each process, so memory they share is counted more than once. The energy figure is estimated from CPU time, not measured in watts. The App Store version can't read temperatures or fan speeds; the optional, free [App Footprint Sensors]({{ "/" | relative_url }}#support) adds them.

## Which one to use

| Question | Use |
|---|---|
| What is using CPU or memory right now? | Activity Monitor |
| Which apps are allowed to start background work? | Login Items & Extensions |
| Which launchd jobs are loaded for my account? | `launchctl list` |
| Which apps were busy in the background over the last few hours? | App Footprint |

For what the many “Helper” entries in Activity Monitor are, see [What are the Helper processes in Activity Monitor?]({{ "/guides/helper-processes-activity-monitor/" | relative_url }}).

Sources: [Apple: View energy consumption in Activity Monitor](https://support.apple.com/HT203184), [Apple: Open items automatically when you log in](https://support.apple.com/guide/mac-help/open-items-automatically-when-you-log-in-mh15189/mac).
