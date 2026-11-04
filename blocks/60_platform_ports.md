<p id="port" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⬡ Platform ports

Non-CUDA runtimes and specialized accelerator backends.

<p id="port-apple" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Apple Silicon

| Name | Format | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **MLX 8bit** | MLX (mflux) | ![int8][badge-int8] | 24.04 GB | [![][gh-JoyFusionAI]](https://huggingface.co/JoyFusionAI/Qwen-Image-2.1-MLX-8bit) | For [mflux](https://github.com/filipstrand/mflux). 13 TE shards. |
| **Coreml** | CoreML `.mlpackage` | ![bf16][badge-bf16] | 14.74 GB | [![][gh-devin--lai]](https://huggingface.co/devin-lai/Qwen-Image-2.1-Coreml) | 4 transformer blocks (~3.5 GB each) + embed + VAE decoder. |
| **QIPACK base** | QIPACK1 `.qipack` | ![fp16][badge-fp16] | 14.23 GB | [![][gh-netdur]](https://huggingface.co/netdur/Qwen-Image-2.1-QIPACK) | For the native C++/Metal [qwen-image-cplus](https://github.com/netdur/qwen-image-cplus) runtime. 40-step base, defaults to TaylorSeer caching. |
| **QIPACK distilled** | QIPACK1 `.qipack` | ![fp16][badge-fp16] | 14.23 GB | [![][gh-netdur]](https://huggingface.co/netdur/Qwen-Image-2.1-QIPACK) | Same runtime, 4-step. The Viggle v0.1 **full fine-tune**, not its LoRA and not v0.2.1. |
| **MLX 4bit** | MLX | ![int4][badge-int4] | 11.59 GB | [![][gh-themindstudio]](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit) | Smallest viable Apple build. |
| **Uncensored MLX** | MLX (4/6/8-bit) | ![int4][badge-int4] ![int8][badge-int8] | 7.56 GB | ⚠️ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized) | Apple build of the Turbo DiT: 7.56 GB at 8-bit, 5.78 at 6-bit, 4.00 at 4-bit. The repo also carries a BF16 text encoder (17.53 GB), an int8 ConvRot one (9.35 GB) and the VAE (0.68 GB), but those are plain diffusers-layout safetensors rather than MLX builds, so an MLX pipeline still pairs with one of the two repos above. |

Needs macOS 14+. 1024×1024 works; 2048×2048 does not yet.

<p id="port-mnn" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Mobile &amp; edge (MNN)

Alibaba MNN runtime for on-device inference. The full repos are large, and the MNN build bundles the text encoder, DiT, and VAE together.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **MNN fp16** | ![fp16][badge-fp16] | 30.72 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-fp16) | Highest fidelity, and by far the largest. |
| **MNN int8** | ![int8][badge-int8] | 21.42 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int8) | |
| **MNN int4** | ![int4][badge-int4] | 14.39 GB | [![][gh-yunfengwang]](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int4) | |
| **MNN** | ![int4][badge-int4] | 10.62 GB | [![][gh-evankuo]](https://huggingface.co/evankuo/Qwen-Image-2.1-MNN) | `llm.mnn.weight` 4.73 + `dit.mnn.weight` 4.47 + VAE 0.51 GB. |

<p id="port-alt" align="center">· · · · · · · · · · · · · ·</p>

### ▣ AMD &amp; domestic accelerators

**p150** is a full port to a **single Tenstorrent Blackhole p150a** via `tt-nn`, all three sub-models resident on-chip. ~20.5 s for a 40-step 1024² generation, and 1.4–1.6× an RTX 5090 with CPU offload end to end. Editing support for 1–4 condition images. No weights redistributed; it pulls the official ones.

**[ROCm gfx1151](https://huggingface.co/kingjones777/Qwen-Image-2.1-ROCm-gfx1151)** targets AMD Strix Halo / RX 9070-class iGPUs. Code only; no weights in the repo.

**Intel Arc (SYCL).** **[Frosty40](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF)** is a serving package for the `sd.cpp` SYCL backend, not a fine-tune; "turbo" refers to the recipe, not step distillation. The weights are Qwen's own, quantized from a BF16 master.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **SYCL quality** | ![q5_0][badge-Q5_0] | 7.89 GB | [![][gh-Frosty40]](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF) | Attention qkv/o kept at F16; 73 BF16 + 128 F16 + 96 Q5_0. |
| **SYCL lean** | ![q5_0][badge-Q5_0] | 5.07 GB | [![][gh-Frosty40]](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF) | All-exception Q5_0, 73 BF16 + 224 Q5_0. |

Both ship with a pinned Qwen3-VL-8B-Instruct Q8_0 text encoder and the BF16 VAE. `--vae-tiling` is mandatory at 1024² or the SYCL VAE overflows int32, and run **one `sd-cli` per GPU**: a second concurrent run wedges the xe driver. The 3.84× speedup comes from `--eager-load --cache-mode easycache`, not fewer steps.

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

### ▣ VAE alternatives

Two real finetunes and one preview-only toy. The second finetune is built on the first, so the two are shape-identical; the toy is not a VAE at all.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Texture-fix VAE** | ![bf16][badge-bf16] | 0.68 GB | [![][gh-madebyollin]](https://huggingface.co/madebyollin/texture-fix-vae-for-qwen-image-2.1/resolve/main/texture_fix_vae_for_qwen_image_2.1_bf16.safetensors) | **Drop-in replacement** for `qwen_image_2.1_vae_bf16.safetensors`. Decoder-only finetune targeting checkerboard artifacts and flat texture in photo-style images: rFID 3.37 → **2.08** on COCO at 256², at the cost of PSNR and LPIPS. Ships a diffusers folder too. |
| **None VAE** | ![fp32][badge-fp32] | 1.35 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/None-qwenimage2.1-vae-comfyui/resolve/main/None-qwenimage2.1-vae_comfy_fp32.safetensors) | **Drop-in replacement** for the stock FP32 VAE. A further finetune of the Texture-Fix VAE above, so the two are shape-identical, but kept in FP32 rather than BF16. |
| **TAE preview decoder** | ![fp16][badge-f16] | 0.003 GB | [![][gh-AcademiaSD]](https://huggingface.co/AcademiaSD/TAE-Qwen-Image-2.1) | **Not a VAE**: a 1.63M-param 16× approximate decoder for ComfyUI live previews, ~15 ms for 1024² on an RTX 5080. **RGB only, it drops the alpha channel**, so never use it for the final decode of an RGBA generation. Goes in `models/vae_approx/` and needs **ComfyUI-KJNodes**; the built-in TAESD path only handles 8× decoders. |
| **hdr vae test** | ![fp16][badge-fp16] | 0.68 GB | [![][gh-471Def]](https://huggingface.co/471Def/qwen_image_2.1_hdr_vae_test) | Untested in the wild; treat as an experiment, not a drop-in replacement. |

