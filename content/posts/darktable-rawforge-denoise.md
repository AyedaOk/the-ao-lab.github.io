---
title: "Improve Darktable AI Denoise with RawForge"
date: 2026-07-19
description: "Install RawForge and use it with Darktable 5.6."
tags: ["darktable", "rawforge", "denoise", "linux"]
showShare: false
---

RawForge is one of the best free and open-source AI denoise tools, and you can easily use it from Darktable.

Video: [Improve Darktable AI Denoise with RawForge](https://youtu.be/1_R0TLTs3Y0)

## Installation outline

1. Install [uv](https://docs.astral.sh/uv/getting-started/installation/).
2. Choose one option below, then run the matching command:

```sh
uv tool install "rawforge[cuda]"
```

Replace `cuda` with `directml`, `cpu`, or `web`, depending on your hardware and setup. Use only one command.

3. Check the installation with `rawforge -h`.
4. Install Git.
5. In Darktable, install my [custom script repository](https://youtu.be/NJA8KtFLxao).
6. Enable the RawForge script and restart Darktable.
7. In the Executable Manager, select the RawForge executable, usually inside `~/.local/bin` on Linux and macOS.

After that, select RawForge in Darktable, choose a denoise model, and process your image.

Links: [RawForge](https://github.com/rymuelle/RawForge) · [RawRefinery](https://github.com/rymuelle/RawRefinery)

## Important notes

- RawForge does not work with the Darktable Flatpak.
- Support for RAW formats varies.
- In the video, Deep Blur and Deep Sharpen are not working. The denoise model works.
