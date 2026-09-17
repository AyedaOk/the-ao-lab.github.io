---
title: "Improve Darktable AI Masking with the Large SAM2.1 Model"
date: 2026-07-04
description: "Install the SAM2.1 Large model for AI masking in Darktable 5.6."
tags: ["darktable", "ai", "masking", "sam2"]
showShare: false
---

Darktable 5.6 includes the smaller SAM2.1 model. The Large model can produce slightly better masks, but it uses more memory and takes longer.

Video: [Improve Darktable AI Masking with the Large SAM2.1 Model](https://youtu.be/MUj4v6Y2OQQ)

## Easy installation

1. Download `mask-object-sam21-large.dtmodel` from the [Hugging Face repository](https://huggingface.co/AyedaOk/darktable-sam21-large).
2. Open Darktable and go to **Settings → AI**.
3. Select **Import from file** and choose the downloaded model.
4. Enable the SAM2.1 Large model.

I measured about 4 GB of RAM during testing. Check that your system can handle the model before installing it.

The video also explains how to build the model yourself, but importing the `.dtmodel` file is the simple option.
