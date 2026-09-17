---
title: "How to Use SAM3 with Darktable Nightly"
date: 2026-08-15
description: "Test SAM3 with a separate Darktable nightly environment."
tags: ["darktable", "ai", "masking", "sam3", "nightly"]
showShare: false
---

SAM3 currently works here only with a Darktable nightly build, version 5.7. This guide is not for stable Darktable 5.6.

Video: [How to Use SAM3 with Darktable Nightly](https://youtu.be/Tubz-noyfM4)

## Installation outline

1. Download the Darktable nightly build from the [Darktable GitHub releases](https://github.com/darktable-org/darktable/releases). On Windows, download the `.exe`; on macOS, download the `.dmg`.
2. On Linux, make the AppImage executable:

```sh
chmod +x ./Darktable*.AppImage
```

3. Launch it with a separate configuration and cache:

```sh
Darktable*.AppImage \
  --configdir "$HOME/.config/darktable_sam3" \
  --cachedir "$HOME/.cache/darktable_sam3"
```

4. Open **Settings → AI** and enable AI processing.
5. Download `mask-object-SAM3.dtmodel` from the [SAM3 model repository](https://huggingface.co/AyedaOk/darktable-sam3).
6. Use **Settings → AI → Import from file** to import the model.
7. Enable SAM3, open an image, and use the Mask Manager's AI tool.

## Important warnings

- Use a separate Darktable configuration and cache for testing.
- SAM3 needs more resources than SAM2.
- Read the SAM license before using the model.
