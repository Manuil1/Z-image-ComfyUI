# Z-Image High Fidelity T2I Workflow for ComfyUI

A high-fidelity **text-to-image workflow for ComfyUI** built around **Z-Image Turbo**, designed for realistic vertical image generation with optional LoRA support, tiled upscaling, sharpening, and subtle film grain post-processing.

This workflow is intended for users who want a fast but detailed Z-Image setup that can generate realistic images and then enhance them through an integrated upscale and finishing pipeline.

## Features

* Text-to-image generation using **Z-Image Turbo**
* Optimized for vertical portrait-style images
* Uses **Qwen 3 4B** as the text encoder
* Uses the **Z-Image AE VAE**
* Supports multiple LoRAs through **Power Lora Loader**
* Uses **RES4LYF ClownsharKSampler_Beta** for sampling
* Includes an optional post-processing chain:

  * Ultimate SD Upscale
  * UltraSharpV2 upscale model
  * Laplacian sharpening
  * Subtle cinematic film grain
* Includes image comparison nodes for checking before/after results

## Workflow Overview

The workflow follows this general structure:

```text
Z-Image Turbo model
        ↓
Qwen text encoder + VAE
        ↓
Positive / Negative prompt encoding
        ↓
Optional LoRA loading
        ↓
Latent image generation
        ↓
RES4LYF sampler
        ↓
VAE decode
        ↓
Preview image
        ↓
GPU cleanup
        ↓
Ultimate SD Upscale
        ↓
Sharpening + film grain
        ↓
Final comparison preview
```

The default canvas size is set to:

```text
Width: 912
Height: 1696
Batch size: 1
```

This makes the workflow suitable for portrait, fashion, character, influencer-style, and realistic vertical images.

## Required Models

Place the required model files in the correct ComfyUI folders.

### 1. Diffusion Model

Required model:

```text
zImageTurboQuantized_fp8ScaledE4m3fnKJ.safetensors
```

Recommended location:

```text
ComfyUI/models/diffusion_models/
```

The workflow note references the Kijai FP8 scaled Z-Image Turbo model.

### 2. Text Encoder

Required text encoder:

```text
qwen_3_4b.safetensors
```

Recommended location:

```text
ComfyUI/models/text_encoders/
```

### 3. VAE

Required VAE:

```text
ae.safetensors
```

Recommended location:

```text
ComfyUI/models/vae/
```

### 4. Upscale Model

Required upscale model:

```text
4x-UltraSharpV2.safetensors
```

Recommended location:

```text
ComfyUI/models/upscale_models/
```

## Required Custom Nodes

This workflow uses several custom ComfyUI node packs. Install them with **ComfyUI Manager** or manually in the `custom_nodes` folder.

Required or recommended custom nodes:

```text
RES4LYF
rgthree-comfy
ComfyUI-GGUF
ComfyUI Ultimate SD Upscale
ComfyUI-Easy-Use
comfyui-vrgamedevgirl
```

Main custom nodes used in the workflow include:

```text
ClownsharKSampler_Beta
Power Lora Loader (rgthree)
CLIPLoaderGGUF
UltimateSDUpscale
easy cleanGpuUsed
easy clearCacheAll
FastLaplacianSharpen
FastFilmGrain
Image Comparer (rgthree)
```

## LoRA Support

The workflow includes a Power Lora Loader node with several LoRA slots. Some LoRAs are enabled by default, while others are included but disabled.


You can replace, disable, or adjust these LoRAs depending on the style you want. For a cleaner general-purpose workflow, you may choose to disable all body/style-specific LoRAs and keep only realism or quality-focused LoRAs.

## Recommended Prompt Example

The workflow currently has empty prompt fields, so you should add your own prompt before generating.

Example positive prompt:

```text
realistic portrait photo, natural skin texture, soft daylight, detailed face, realistic eyes, high fidelity, candid photography, 35mm lens
```

Example negative prompt:

```text
plastic skin, blurry, distorted anatomy, extra fingers, bad hands, oversharpened, waxy face, artificial skin, low quality
```

## Default Sampling Settings

The active sampler is:

```text
ClownsharKSampler_Beta
```

Default settings:

```text
Steps: 8
CFG: 1
Scheduler: beta
Sampler: linear/ralston_2s
Denoise: 1
Sampler mode: standard
```

## Upscale and Post-Processing

The workflow includes an upscale stage using Ultimate SD Upscale.

Default upscale settings:

```text
Upscale by: 1.5
Steps: 8
CFG: 1
Sampler: euler
Scheduler: simple
Denoise: 0.11
Tile size: 512 x 512
Tile padding: 32
```

If the image looks too sharp or the skin looks too artificial, reduce the sharpening strength or lower the upscale denoise value.

Suggested softer settings:

```text
FastLaplacianSharpen strength: 0.25 - 0.45
UltimateSDUpscale denoise: 0.08 - 0.13
Film grain intensity: 0.004 - 0.008
```
