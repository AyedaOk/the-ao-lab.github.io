---
title: "How to Install and Update Custom Lua Scripts in Darktable 5.6"
date: 2026-07-25
description: "Install and update a third-party Darktable Lua script repository with Git."
tags: ["darktable", "lua", "linux", "git"]
showShare: false
---

This guide explains how to install and update my custom Darktable Lua script repository.

Video: [How to Install and Update Custom Lua Scripts in Darktable 5.6](https://youtu.be/NJA8KtFLxao)

Repository: [AyedaOk/DT_custom_script](https://github.com/AyedaOk/DT_custom_script)

## Install the repository

1. Install [Git](https://git-scm.com/install).
2. Restart Darktable.
3. Open the script module and select **Action → Install Update Script**.
4. Paste the repository URL and choose a folder name, such as `Custom`.
5. Click **Install**, then restart Darktable again.
6. If the scripts do not appear, select **Action → Start Stop Script**, choose the `Custom` folder, and enable the scripts.

## Update the repository

Close Darktable and open a terminal. On Linux and macOS, run:

```sh
cd ~/.config/darktable/lua/Custom
git pull --ff-only
```

ON Windows, run:

```sh
cd $env:LOCALAPPDATA\darktable\lua\Custom
git pull --ff-only
```

Restart Darktable after updating.
