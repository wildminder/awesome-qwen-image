<p id="port" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⬡ Platform ports

Non-CUDA runtimes and specialized accelerator backends.

<p id="port-apple" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Apple Silicon

| Name | Format | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **MLX 8bit** | MLX (mflux) | ![int8][badge-int8] | 24.04 GB | [![][gh-JoyFusionAI]](https://huggingface.co/JoyFusionAI/Qwen-Image-2.1-MLX-8bit) | For [mflux](https://github.com/filipstrand/mflux). 13 TE shards. |
| **Coreml** | CoreML `.mlpackage` | ![bf16][badge-bf16] | 14.74 GB | [![][gh-devin--lai]](https://huggingface.co/devin-lai/Qwen-Image-2.1-Coreml) | 4 transformer blocks (~3.5 GB each) + embed + VAE decoder. |
| **MLX 4bit** | MLX | ![int4][badge-int4] | 11.59 GB | [![][gh-themindstudio]](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit) | Smallest viable Apple build. |

<p id="port-mnn" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Mobile &amp; edge (MNN)

Alibaba MNN runtime for on-device inference. The full repos are large — the MNN build bundles the text encoder, DiT, and VAE together.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **MNN fp16** | ![fp16][badge-fp16] | 30.72 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-fp16) | Highest fidelity, and by far the largest. |
| **MNN int8** | ![int8][badge-int8] | 21.42 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int8) | |
| **MNN int4** | ![int4][badge-int4] | 14.39 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int4) | |
| **MNN** | ![int4][badge-int4] | 10.62 GB | [![][gh-evankuo]](https://huggingface.co/evankuo/Qwen-Image-2.1-MNN) | `llm.mnn.weight` 4.73 + `dit.mnn.weight` 4.47 + VAE 0.51 GB. |

<p id="port-alt" align="center">· · · · · · · · · · · · · ·</p>

### ▣ AMD &amp; domestic accelerators

**p150** is the standout entry here. A full port to a **single Tenstorrent Blackhole p150a** via `tt-nn`, with all three sub-models resident on-chip. ~20.5 s for a 40-step 1024² generation (513 ms/step sustained), and it beats an RTX 5090 with CPU offload by 1.4–1.6× end to end. Includes editing support for 1–4 condition images. No weights are redistributed — it pulls the official ones.

**[ROCm gfx1151](https://huggingface.co/kingjones777/Qwen-Image-2.1-ROCm-gfx1151)** targets AMD Strix Halo / RX 9070-class iGPUs. Code only; no weights in the repo.

**FlagOS** ships eight builds for Chinese NPUs, same 28-file layout in each:

| Name | Target | Precision | Size | Links |
| :--- | :---: | :---: | :---: | :---: |
| **BF16 nvidia** | NVIDIA (FlagOS path) | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-nvidia-FlagOS) |
| **BF16 hygon** | Hygon DCU | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-hygon-FlagOS) |
| **BF16 ascend** | Ascend NPU | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-ascend-FlagOS) |
| **BF16 metax** | MetaX CGC | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-metax-FlagOS) |
| **BF16 enflame** | Enflame GCU | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-enflame-FlagOS) |
| **BF16 zhenwu** | Zhenwu MUSA | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-zhenwu-FlagOS) |
| **BF16 mthreads** | Moore Threads MUSA | ![bf16][badge-bf16] | 33.13 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-mthreads-FlagOS) |
| **W8A8 arm** | ARM | ![w8a8][badge-w8a8] | 29.38 GB | [![][gh-FlagRelease]](https://huggingface.co/FlagRelease/Qwen-Image-2.1-W8A8-arm-FlagOS) |

<p id="port-pkg" align="center">· · · · · · · · · · · · · ·</p>

### ▣ ComfyUI package formats

Same weights, re-laid-out for a ComfyUI-side loader. Each ships a YAML manifest next to the shards, so no diffusers config is needed.

| Name | Format | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **libwaifu bf16** | libwaifu (yaml + 8 shards) | ![bf16][badge-bf16] | 30.03 GB | [![][gh-ling0322]](https://huggingface.co/ling0322/libwaifu-qwen-image-2.1) | Full-precision layout for the `libwaifu` loader. |
| **libwaifu fp8** | libwaifu (yaml + 4 shards) | ![fp8][badge-fp8] | 15.98 GB | [![][gh-ling0322]](https://huggingface.co/ling0322/libwaifu-qwen-image-2.1) | Same layout, `weight_format: fp8`. Half the download. |

<p id="port-vae" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Experimental VAE

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **hdr vae test** | ![fp16][badge-fp16] | 0.68 GB | [![][gh-471Def]](https://huggingface.co/471Def/qwen_image_2.1_hdr_vae_test) | Untested in the wild; treat as an experiment, not a drop-in replacement. |

