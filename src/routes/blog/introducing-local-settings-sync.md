---
author: filiptronicek, jeanp413, loujaybee, akosyakov
date: Monday, 23 May 2022 15:00:00 UTC
excerpt: With local Settings Sync you can sync your VS Code preferences from VS Code Browser to VS Code Desktop and vice versa.
slug: introducing-local-settings-sync
image: teaser.png
teaserImage: teaser.png
title: Introducing VS Code Desktop Settings Sync
---

<script context="module">
  export const prerender = true;
</script>

**TL;DR;** from today, you can use [Settings Sync](https://code.visualstudio.com/docs/editor/settings-sync) with Gitpod in VS Code Desktop, allowing you to seamlessly sync your VS Code preferences from your desktop VS Code with VS Code running in the browser.

Visual Studio Code is an IDE loved by its community for its virtually endless customization capabilities, meaning you can spend hours upon hours installing the perfect extension, color theme and configuring the exact editor settings to enhance your workflow. This becomes a burden, though, as soon as you try to fire up VS Code sessions on another machine. This is where a feature called Settings Sync comes in.

Via Settings Sync, VS Code enables you to synchronize your settings, themes, extensions and other preferences across machines. This feature is convenient for both having all your machines' preferences in sync, but also for quickly bootstrapping your editor on any new machine, just the way you like it.

> **Note**: On Gitpod, every workspace you open is de facto a "new machine", so if it wasn't for Settings Sync, you would have to setup your IDE from scratch every time.

Implementing Settings Sync was achieved by creating a custom version of the Settings Sync backend, because the setting sync server that Microsoft provides with their Settings Sync service is private and therefore inaccessible to 3rd party services like Gitpod.

It's nice having Settings Sync work with VS Code in the browser, but how are you supposed to bring the settings you have on your local machine to Gitpod? Should you copy your whole JSON file with your settings and manually install every extension that you have?

These are questions, which we have asked ourselves at Gitpod as well - and we found a solution! You can now, with the help of the [Gitpod VS Code extension](https://marketplace.visualstudio.com/items?itemName=gitpod.gitpod-desktop) (which you might already have if you've used Gitpod with VS Code Desktop) have your settings synced to Gitpod instead of Microsoft, making it the place of your preferences across machines. Having the ability to enable settings sync locally with VS Code Desktop enables you to get around some browser limitations, like Ctrl+W [[1](https://github.com/gitpod-io/openvscode-server/discussions/322)] and others.

<div style="position: relative; padding-bottom: 77.92207792207792%; height: 0;"><iframe title="Gitpod Local Settings Sync guide" src="https://www.loom.com/embed/9a4b3cb32d134aa69f7eb1b39340ccf6" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>

The process for setting local Settings Sync up inside VS Code Desktop is as follows:

- Install the Gitpod extension
- Open the Command Palette (F1 or Cmd/Ctrl + Shift + P) and run Settings Sync: Enable Sign In with Gitpod,
- Restart your VS Code Desktop application.
- Enable settings sync from the Manage gear menu at the bottom of the Activity Bar.
- Resolve any settings conflicts - there will most likely be differences between your remote VS Code config and your local one. This step makes sure you retain the preferences you want.
- Voilà! Now you should see your preferences be reflected on VS Code Browser inside of Gitpod as well.

![Command Pallete screenshot](../../static/blog/../../../static/images/blog/introducing-local-settings-sync/palette.png)

We'd love for you to try out the new local Settings Sync for yourself, and would love to hear from you in our [community](https://www.gitpod.io/community). We look forward to hearing from you!
