# Awesome Qwen-Image 2.1

A curated list of checkpoints, quants, prompt engines, LoRAs, and tooling for **Qwen-Image 2.1** — Alibaba's 7B unified text-to-image and image-editing model.

<div align="center">

[![Hugging Face][hf-shield]][hf-url]
[![License][lic-shield]][lic-url]
[![Last Commit][commit-shield]][commit-url]
[![PRs Welcome][prs-shield]][prs-url]

</div>

> [!NOTE]
> **Qwen-Image 2.1** was released **2026-09-20** with day-0 support in Diffusers, ComfyUI, vLLM-Omni, SGLang, and LightX2V. It unifies generation and editing in one 7B DiT, adds native RGBA output, and accepts up to 10 reference images. Licensed under the **Qwen Research License**.

<details>
<summary><b>Table of Contents</b></summary>

* [Quick start](#quick-start)
* [Official checkpoints](#official)
  * [Base model](#official-base)
  * [ComfyUI official](#official-comfy)
* [Text encoders &amp; prompt engines](#encoders)
  * [Prompt rewriters (PE)](#pe)
  * [Compact rewriters](#pe-pocket)
  * [Heretic &amp; abliterated](#pe-heretic)
  * [Quantized &amp; ported PE](#pe-quant)
  * [Heretic text encoders](#te-heretic)
* [Quantizations](#quant)
  * [GGUF](#quant-gguf)
  * [4-bit &amp; 8-bit](#quant-lowbit)
  * [FP8 &amp; bf16](#quant-fp8)
  * [Nunchaku (SVDQ)](#quant-nunchaku)
* [Turbo &amp; step distillation](#turbo)
* [LoRA &amp; adapters](#lora)
* [Platform ports](#port)
  * [Apple Silicon](#port-apple)
  * [Mobile &amp; edge (MNN)](#port-mnn)
  * [AMD &amp; domestic accelerators](#port-alt)
  * [Experimental VAE](#port-vae)
* [Tools &amp; notebooks](#tools)

</details>

---

<a id="quick-start"></a>

## ⌬ Quick start

Pick your entry point based on the runtime you already have.

| I want to… | Use | Why |
| :--- | :--- | :--- |
| Run the reference model in Diffusers | **[Qwen/Qwen-Image-2.1](https://huggingface.co/Qwen/Qwen-Image-2.1)** | Official BF16 diffusers repo, `QwenImage21Pipeline` |
| Use it in ComfyUI | **[Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** | Pre-split folder layout, Day-0 native nodes |
| Fit it in 8–12 GB VRAM | **[chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI](https://huggingface.co/chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI)** | Complete ComfyUI pack incl. int4 ConvRot DiT + TE |
| Run locally with llama.cpp / GGUF | **[unsloth/Qwen-Image-2.1-GGUF](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** | Widest quant spread, Q2_K → Q8_0 |
| Generate in 4 steps | **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** | Official turbo distill + LoRA variants |
| On a Mac (Apple Silicon) | **[themindstudio/Qwen-Image-2.1-MLX-4bit](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit)** | MLX 4-bit, native unified memory |
| Improve prompt quality | **[Qwen/Qwen-Image-2.1-PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)** | Official prompt rewriter + aspect-ratio picker |

**Official resources**

* [Qwen-Image-2.1 model card](https://huggingface.co/Qwen/Qwen-Image-2.1) — official weights, license, aspect-ratio table
* [Qwen-Image-2.1 GitHub repo](https://github.com/QwenLM/Qwen-Image-2.1) — inference code, news, supported frameworks
* [Qwen blog: Qwen-Image-2.1](https://qwen.ai/blog?id=qwen-image-2.1) — release notes with architecture and benchmark figures
* [ComfyUI native workflow example](https://docs.comfy.org/tutorials/image/qwen/qwen-image-2-1) — official ComfyUI tutorial
* [ModelScope mirror](https://modelscope.cn/models/Qwen/Qwen-Image-2.1) — official CN mirror

**Model facts worth knowing**

* **7B parameters** in the visual generation component, **32 single-stream DiT layers**.
* Mixed-granularity attention (token-level causal for text, chunk-level for image) with **prefix KV cache reuse** for multi-reference editing.
* Native **RGBA** output — the prompt decides whether the result has an alpha channel.
* Editing accepts **up to 10 reference images**, plus circles, painted annotations, or separate masks for local edits.
* **Qwen3-VL-8B** is the text encoder, so the text encoder is the largest single download at ~17.5 GB BF16.
* Default sample setting is **40 steps without classifier-free guidance**; CFG is available for prompt adherence.

<p id="official" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▓ Official checkpoints

The reference weights. Start here before touching any community conversion.

<p id="official-base" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Base model

| Repo | Precision | Layout | DiT | Text encoder | VAE | Notes |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **[Qwen/Qwen-Image-2.1](https://huggingface.co/Qwen/Qwen-Image-2.1)** | ![bf16][badge-bf16] | diffusers | 14.23 GB | 17.53 GB (Qwen3-VL-8B) | 1.35 GB | Official. `QwenImage21Pipeline`. |
| **[KasugaiSakura/Qwen-Image-2.1-Original](https://huggingface.co/KasugaiSakura/Qwen-Image-2.1-Original)** | ![bf16][badge-bf16] | diffusers | 14.23 GB | 17.53 GB | 1.35 GB | Unmodified mirror, same shard layout. |

<p id="official-comfy" align="center">· · · · · · · · · · · · · ·</p>

### ▣ ComfyUI official

**[Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** — the Day-0 ComfyUI repackage. Files land directly in `models/` with no renaming.

| File | Precision | Size | Place in |
| :--- | :--- | ---: | :--- |
| `diffusion_models/qwen_image_2.1_bf16.safetensors` | ![bf16][badge-bf16] | 14.23 GB | `models/diffusion_models/` |
| `diffusion_models/qwen_image_2.1_int8_convrot.safetensors` | ![int8][badge-int8] | 7.26 GB | `models/diffusion_models/` |
| `text_encoders/qwen3vl_8b_bf16.safetensors` | ![bf16][badge-bf16] | 17.53 GB | `models/text_encoders/` |
| `text_encoders/qwen3vl_8b_int8_convrot.safetensors` | ![int8][badge-int8] | 9.35 GB | `models/text_encoders/` |
| `text_encoders/qwen3vl_8b_w4a8.safetensors` | ![w4a8][badge-w4a8] | 6.31 GB | `models/text_encoders/` |
| `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_t2i.int8_convrot.safetensors` | ![int8][badge-int8] | 9.47 GB | `models/text_encoders/` |
| `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors` | ![int8][badge-int8] | 9.47 GB | `models/text_encoders/` |
| `vae/qwen_image_2.1_vae_bf16.safetensors` | ![bf16][badge-bf16] | 0.68 GB | `models/vae/` |

> [!TIP]
> `ConvRot` files are ComfyUI's native rotated-channel integer format. Use a recent ComfyUI build and load them with the standard diffusion-model and text-encoder loaders — no custom nodes required.

<p id="encoders" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚲ Text encoders &amp; prompt engines

Qwen-Image 2.1 conditions on **Qwen3-VL-8B**, which at BF16 is the largest file in the stack. It also ships dedicated **prompt-rewriting** models — separate text models that take a short request in any language and return a detailed English prompt plus a recommended aspect ratio. Neither is part of the image pipeline's diffusion path; both are optional, and you can run the base model without either.

<p id="pe" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Official prompt rewriters

| Repo | Task | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[Qwen/Qwen-Image-2.1-PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)** | text → image | ![bf16][badge-bf16] | 18.82 GB | Fine-tuned Qwen3.5-VL 9B. Returns `{rewritten_prompt, wh_ratio}`. |
| **[Qwen/Qwen-Image-2.1-PE-I2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-I2I)** | image → image | ![bf16][badge-bf16] | 18.82 GB | Identical shard layout; editing-oriented rewriting. |

Both ship a `system_prompt.txt` and work with `AutoModelForCausalLM` + `AutoTokenizer`. Output is JSON after a reasoning block, so split on the think-tag before parsing.

<p id="pe-pocket" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Compact rewriters

Much smaller rewriters from ML-Intern Lab, fine-tuned from Qwen3.5 base models instead of the 9B VL model. Roughly 5× smaller than the official rewriter.

| Repo | Params | Precision | Size | Notes |
| :--- | :---: | :--- | ---: | :--- |
| **[ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B)** | 2B | ![bf16][badge-bf16] | 3.76 GB | Best size/quality tradeoff for local rewriting. |
| **[ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B)** | 0.8B | ![Q8_0][badge-Q8_0] | 1.50 GB | Smallest; also ships a `Q8_0` GGUF (0.81 GB) for llama.cpp. |

<p id="pe-heretic" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Heretic &amp; abliterated rewriters

> [!WARNING]
> These repos have the safety refusal direction **abliterated** from the rewriter. It no longer refuses prompt content, which means it will happily rewrite anything you ask. Same Qwen Research License as the base weights — the license does not grant you additional rights.

| Repo | Task | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[pottokao/Qwen-Image-2.1-PE-T2I-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-T2I-Heretic-GGUF)** | text → image | ![Q4_K_M][badge-Q4_K_M] | 5.89 GB | Most-downloaded Heretic rewriter. |
| **[pottokao/Qwen-Image-2.1-PE-I2I-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-I2I-Heretic-GGUF)** | image → image | ![Q4_K_M][badge-Q4_K_M] | 6.81 GB | Includes an `mmproj` projector for llama.cpp. |
| **[t8star/qwen-image-2.1-comfy](https://huggingface.co/t8star/qwen-image-2.1-comfy)** | both | ![Q4_K_M][badge-Q4_K_M] | 18.07 GB | Bundle of PE-T2I + PE-I2I + heretic T2I, ComfyUI-oriented naming. |
| **[base11231/Qwen-Image-2.1-PE-I2I-Abliterated](https://huggingface.co/base11231/Qwen-Image-2.1-PE-I2I-Abliterated)** | image → image | ![bf16][badge-bf16] | 18.82 GB | Editing-only abliteration, not GGUF. |
| **[darrellbest/Qwen-Image-2.1-PE-T2I-Heretic](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic)** | text → image | ![bf16][badge-bf16] | 18.82 GB | Source for the NVFP4 build below. |
| **[darrellbest/Qwen-Image-2.1-PE-I2I-Heretic](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic)** | image → image | ![bf16][badge-bf16] | 18.82 GB | Source for the NVFP4 build below. |
| **[darrellbest/Qwen-Image-2.1-PE-T2I-Heretic-NVFP4](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic-NVFP4)** | text → image | ![nvfp4][badge-nvfp4] | 11.20 GB | 4-bit NF4. |
| **[darrellbest/Qwen-Image-2.1-PE-I2I-Heretic-NVFP4](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic-NVFP4)** | image → image | ![nvfp4][badge-nvfp4] | 11.20 GB | 4-bit NF4. |

<p id="pe-quant" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Quantized &amp; ported rewriters

| Repo | Format | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[HarleyWang/Qwen-Image-2.1-PE-ComfyUI](https://huggingface.co/HarleyWang/Qwen-Image-2.1-PE-ComfyUI)** | BF16 + int8 ConvRot | ![bf16][badge-bf16] ![int8][badge-int8] | 64.68 GB | PE-T2I and PE-I2I at two precisions, ComfyUI naming. |
| **[prithivMLmods/Qwen-Image-2.1-PE-T2I-MLX](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-T2I-MLX)** | MLX (4/8/16-bit) | ![int4][badge-int4] ![int8][badge-int8] ![bf16][badge-bf16] | 35.20 GB | Apple Silicon. |
| **[prithivMLmods/Qwen-Image-2.1-PE-I2I-MLX](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-I2I-MLX)** | MLX (4/8/16-bit) | ![int4][badge-int4] ![int8][badge-int8] ![bf16][badge-bf16] | 35.20 GB | Apple Silicon. |
| **[foofifoo/Qwen-Image-2.1-Prompt-Enhancement-INT8-Convrot](https://huggingface.co/foofifoo/Qwen-Image-2.1-Prompt-Enhancement-INT8-Convrot)** | int8 ConvRot | ![int8][badge-int8] | 24.69 GB | Both PE-T2I and PE-I2I in one repo — the only single-download option for both. |
| **[diffnamehard/Qwen-Image-2.1-PE-T2I-Heretic-int8-tensorwise-convrot](https://huggingface.co/diffnamehard/Qwen-Image-2.1-PE-T2I-Heretic-int8-tensorwise-convrot)** | int8 tensorwise ConvRot | ![int8][badge-int8] | 9.99 GB | Heretic rewriter, tensorwise per-channel scaling. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ3-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ3-G64)** | MLX oQ3 G64 | ![Q3][badge-Q3] | — | ⚠️ **Empty repo** — only `.gitattributes`. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ4-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ4-G64)** | MLX oQ4 G64 | ![Q4][badge-Q4] | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ5-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ5-G64)** | MLX oQ5 G64 | ![Q5][badge-Q5] | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ6-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ6-G64)** | MLX oQ6 G64 | ![Q6][badge-Q6] | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ8-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ8-G64)** | MLX oQ8 G64 | ![Q8][badge-Q8] | — | ⚠️ **Empty repo** — no weights uploaded. |

<p id="te-heretic" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Heretic text encoders

The same abliteration applied to the **text encoder** rather than the rewriter — the conditioning signal itself no longer refuses.

| Repo | Format | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF)** | GGUF + mmproj | ![Q4_K_M][badge-Q4_K_M] ![fp8][badge-fp8] ![bf16][badge-bf16] | 33.07 GB | The most-used text encoder in this list. Q4_K_M (5.03 GB), fp8 (9.34 GB), bf16 (17.53 GB), plus a 1.16 GB `mmproj`. |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-NVFP4](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-NVFP4)** | NVFP4 | ![nvfp4][badge-nvfp4] | 6.31 GB | 4-bit NF4, diffusers format. |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-int8-convrot](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-int8-convrot)** | int8 ConvRot | ![int8][badge-int8] | 9.35 GB | ComfyUI ConvRot format, same size as the official int8 TE. |
| **[Karsus1997/Qwen-Image-2.1-Text-Encoder-Heretic-W4A8](https://huggingface.co/Karsus1997/Qwen-Image-2.1-Text-Encoder-Heretic-W4A8)** | W4A8 | ![w4a8][badge-w4a8] | 6.31 GB | 4-bit weights, 8-bit activations. |
| **[kkxao/Qwen-Image-2.1-Text-Encoder-Heretic](https://huggingface.co/kkxao/Qwen-Image-2.1-Text-Encoder-Heretic)** | BF16 | ![bf16][badge-bf16] | 17.53 GB | Base is Qwen3-VL-8B-Instruct. |

<p id="quant" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◈ Quantizations

Community conversions of the base DiT. Sizes are total weight bytes per repo. Distilled turbo GGUFs live under [Turbo](#turbo) instead.

<p id="quant-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ GGUF

For llama.cpp and anything that reads GGUF. Q4_K_M is the usual quality/size balance point.

| Repo | Quant | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[abenzerps/Qwen-Image-2.1-Uncensored-GGUF](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF)** | full ladder + fp8 | ![Q4_0][badge-Q4_0] ![Q8_0][badge-Q8_0] ![bf16][badge-bf16] ![fp8][badge-fp8] ![int8][badge-int8] | 83.61 GB | ⚠️ Uncensored. The single most-downloaded repo in this list. |
| **[unsloth/Qwen-Image-2.1-GGUF](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** | Q2_K → Q8_0 (12 files) | ![F16][badge-fp16] ![Q2_K][badge-Q2_K] ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 64.83 GB | Widest quant spread available, includes F16. |
| **[0xSojalSec/Qwen-Image-2.1-Uncensored-HF](https://huggingface.co/0xSojalSec/Qwen-Image-2.1-Uncensored-HF)** | full ladder | ![Q4_0][badge-Q4_0] ![Q8_0][badge-Q8_0] ![bf16][badge-bf16] | 69.24 GB | ⚠️ Uncensored. Same UC weights as abenzerps plus safetensors TEs. |
| **[ped4enko/Qwen-Image-2.1-Dessi](https://huggingface.co/ped4enko/Qwen-Image-2.1-Dessi)** | Q4_0 → Q8_0 | ![Q4_0][badge-Q4_0] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 54.91 GB | Ships TE safetensors + VAE alongside. |
| **[realrebelai/Qwen-Image-2.1_GGUFs](https://huggingface.co/realrebelai/Qwen-Image-2.1_GGUFs)** | 5-step ladder | ![Q2][badge-Q2] ![Q3][badge-Q3] ![Q4][badge-Q4] ![Q5][badge-Q5] ![Q8][badge-Q8] | 28.75 GB | Q2/Q3/Q4/Q5/Q8. |
| **[vantagewithai/Qwen-Image-2.1-ComfyUI-GGUF](https://huggingface.co/vantagewithai/Qwen-Image-2.1-ComfyUI-GGUF)** | Q3_K_M → Q8_0 | ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 25.94 GB | ComfyUI-oriented. |
| **[pottokao/Qwen-Image-2.1-DiT-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-GGUF)** | 3 quants | ![Q4_K_M][badge-Q4_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 18.02 GB | DiT only, pairs with a separate TE. |
| **[gguf-org/qwen-image-2.1-gguf](https://huggingface.co/gguf-org/qwen-image-2.1-gguf)** | Q4_K_M + NVFP4 | ![Q4_K_M][badge-Q4_K_M] ![nvfp4][badge-nvfp4] | 17.48 GB | **Transferred from `chatpig`** — the old URL redirects here. Ships the TE `mmproj` (0.75 GB), TE quants, and VAE GGUFs. |
| **[zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed](https://huggingface.co/zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed)** | Q4 HVQ3 | ![Q4][badge-Q4] | 5.96 GB | For **sd.cpp**, not stock llama.cpp. |

<p id="quant-lowbit" align="center">· · · · · · · · · · · · · ·</p>

### ▣ 4-bit &amp; 8-bit

Every FP4 / NVFP4 / MXFP4 / INT8 / INT4 / W4A4 conversion in one place.

| Repo | Format | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI](https://huggingface.co/chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI)** | ConvRot pack | ![bf16][badge-bf16] ![int8][badge-int8] ![int4][badge-int4] ![w4a8][badge-w4a8] | 65.91 GB | **Best all-in-one ComfyUI pack.** DiT and TE at three precisions plus VAE, in correct folder layout. |
| **[HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-W4A4-NVFP4](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-W4A4-NVFP4)** | W4A4 NVFP4 | ![nvfp4][badge-nvfp4] | 23.66 GB | NVIDIA ModelOpt-derived, full repo. |
| **[pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI)** | NVFP4 | ![nvfp4][badge-nvfp4] | 13.82 GB | Three ComfyUI cuts: `nvfp4`, `nvfp4_T2`, `nvfp4_T3`. |
| **[Rin247/Qwen-Image-2.1-FP4](https://huggingface.co/Rin247/Qwen-Image-2.1-FP4)** | FP4 | ![fp4][badge-fp4] | 11.74 GB | Full diffusers repo (DiT 6.46 + TE 4.94 + VAE 0.34). |
| **[Rin247/Qwen-Image-2.1-INT8](https://huggingface.co/Rin247/Qwen-Image-2.1-INT8)** | INT8 | ![int8][badge-int8] | 17.96 GB | Full diffusers repo. |
| **[Rin247/Qwen-Image-2.1-INT4](https://huggingface.co/Rin247/Qwen-Image-2.1-INT4)** | INT4 | ![int4][badge-int4] | 11.08 GB | Full diffusers repo. |
| **[addlabsviral/Qwen-Image-2.1-4bit](https://huggingface.co/addlabsviral/Qwen-Image-2.1-4bit)** | 4-bit | ![int4][badge-int4] | 11.41 GB | Full diffusers repo. |
| **[circulus/Qwen-Image-2.1-bnb-4bit](https://huggingface.co/circulus/Qwen-Image-2.1-bnb-4bit)** | bitsandbytes NF4 | ![int4][badge-int4] | 11.41 GB | Standard bnb 4-bit. |
| **[ModelsLab/Qwen-Image-2.1-W4A4-int4](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4)** | W4A4 INT4 | ![int4][badge-int4] | 4.66 GB | DiT only. |
| **[ModelsLab/Qwen-Image-2.1-W4A4-nvfp4](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4)** | W4A4 NVFP4 | ![nvfp4][badge-nvfp4] | 4.88 GB | DiT only; near-identical alternative to the INT4 build. |
| **[EliovpAI/Qwen_Image-2.1-MXFP4](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4)** | MXFP4 (Paiton) | ![mxfp4][badge-mxfp4] | 9.31 GB | Paiton backend, 57 shards. |
| **[EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4)** | MXFP4 RDNA4 | ![mxfp4][badge-mxfp4] | 9.31 GB | RDNA4-specific kernel variant, identical layout. |
| **[EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton](https://huggingface.co/EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton)** | MXFP4 Paiton | ![mxfp4][badge-mxfp4] | 9.31 GB | ⚠️ Uncensored, derived from abenzerps GGUF. |

<p id="quant-fp8" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; bf16

| Repo | Precision | Size | Notes |
| :--- | :--- | ---: | :--- |
| **[HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8)** | ![fp8][badge-fp8] | 26.33 GB | NVIDIA ModelOpt-derived, full repo. |
| **[Rin247/Qwen-Image-2.1-FP8](https://huggingface.co/Rin247/Qwen-Image-2.1-FP8)** | ![fp8][badge-fp8] | 17.96 GB | Full diffusers repo — closest thing to a drop-in smaller BF16. |
| **[dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor)** | ![bf16][badge-bf16] | 14.23 GB | ⚠️ Uncensored BF16, single file. |
| **[mingyi456/Qwen-Image-2.1-DF11-ComfyUI](https://huggingface.co/mingyi456/Qwen-Image-2.1-DF11-ComfyUI)** | ![bf16][badge-bf16] | 9.72 GB | `qwen_image_2.1_bf16-DF11.safetensors` for ComfyUI. |
| **[RunningHubAI/rh-qwen-image-2.1-bf16-unet](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet)** | ![bf16][badge-bf16] | — | ⚠️ **Empty placeholder** — no files uploaded yet. |

<p id="quant-nunchaku" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Nunchaku (SVDQ)

SVDQ-based 4-bit for Nunchaku, which targets low-VRAM systems and 4090-class cards.

| Repo | Precision | Size | Notes |
| :--- | :--- | ---: | :--- |
| **[catplusplus/nunchaku-qwen-image-2.1](https://huggingface.co/catplusplus/nunchaku-qwen-image-2.1)** | ![fp4][badge-fp4] | 8.75 GB | Two builds: `best_quality_fp4` and `svdq-fp4_r32`. |
| **[BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4](https://huggingface.co/BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4)** | ![int4][badge-int4] | 4.15 GB | "Lite" variant — half the size of the FP4 build. |

<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚡ Turbo &amp; step distillation

4-step generation. Useful for iteration and batch work; expect some quality loss versus 40 steps.

| Repo | Kind | Steps | Precision | Size | Notes |
| :--- | :--- | ---: | :--- | ---: | :--- |
| **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** | Full distill + LoRAs | 4 / 5 | ![bf16][badge-bf16] | 19.33 GB | Source repo. `4step-lora-r64` and `5step-lora-r256` PEFT adapters plus a full `transformer/`. |
| **[realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs)** | GGUF | 4 | ![Q2_K][badge-Q2_K] ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 34.07 GB | Turbo in GGUF, Q2_K → Q8_0. |
| **[Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF)** | GGUF | 4 | ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 29.91 GB | Alternate turbo GGUF set. |
| **[RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora)** | LoRA | 4 | ![bf16][badge-bf16] | 0.34 GB | Rank-64 Viggle turbo LoRA, ComfyUI naming. |
| **[RunningHubAI/rh-qwen-image-2.1-turbo-4step-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-turbo-4step-lora)** | LoRA | 4 | ![bf16][badge-bf16] | — | ⚠️ **Empty placeholder** — created 2026-09-23, no files yet. |

<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, control, and fix adapters. All target `Qwen/Qwen-Image-2.1` unless noted.

| Repo | Type | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[e-n-v-y/Qwen-Image-2.1-Fix](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix)** | Quality fix | ![bf16][badge-bf16] | 0.11 GB | `qwen-image-2.1-fix-1.0-comfy.safetensors`. The most-liked community LoRA. |
| **[RunningHubAI/rh-qwen-image-2.1ai-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora)** | Style | ![bf16][badge-bf16] | 2.45 GB | 8-file "remove the AI look" pack (CN filenames). |
| **[prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview)** | Control | ![bf16][badge-bf16] | 0.50 GB | Bbox object *moving*, 6 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview)** | Control | ![bf16][badge-bf16] | 0.50 GB | Bbox object removal, full-quality variant, 6 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA)** | Style | ![bf16][badge-bf16] | 0.42 GB | Exposure correction, 5 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo)** | Control | ![bf16][badge-bf16] | 0.42 GB | Bbox removal, 4-step-compatible variant. |
| **[Aero-Ex/Qwen-Image2.1_Normal2RGB](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB)** | Utility | ![bf16][badge-bf16] | 0.25 GB | Normal map → RGB render, 3 checkpoints (2000/3000/4000). |
| **[Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer)** | Style | ![fp16][badge-fp16] | 0.10 GB | `deckbuilder_cardart_style_lora_v1_fp16`. |
| **[AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0)** | ⚠️ NSFW | ![bf16][badge-bf16] | 0.17 GB | Character-style LoRA. |
| **[RunningHubAI/rh-qwen-image-2.1-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora)** | General | ![bf16][badge-bf16] | — | ⚠️ **Empty placeholder** — no files uploaded. |
| **[RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora)** | ⚠️ NSFW | ![bf16][badge-bf16] | — | ⚠️ **Empty placeholder** — no files uploaded. |
| **[RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora)** | ⚠️ NSFW | ![bf16][badge-bf16] | — | ⚠️ **Empty placeholder** — no files uploaded. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used — the uncensored GGUF in particular is the most-downloaded repo here. They carry the same **Qwen Research License** as the base model; a research license is not a license to do whatever you want, and you remain responsible for how you use them. Several are empty placeholders created on release day — check before planning around them.

<p id="port" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⬡ Platform ports

Non-CUDA runtimes and specialized accelerator backends.

<p id="port-apple" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Apple Silicon

| Repo | Format | Precision | Size | Notes |
| :--- | :--- | :--- | ---: | :--- |
| **[JoyFusionAI/Qwen-Image-2.1-MLX-8bit](https://huggingface.co/JoyFusionAI/Qwen-Image-2.1-MLX-8bit)** | MLX (mflux) | ![int8][badge-int8] | 24.04 GB | For [mflux](https://github.com/filipstrand/mflux). 13 TE shards. |
| **[devin-lai/Qwen-Image-2.1-Coreml](https://huggingface.co/devin-lai/Qwen-Image-2.1-Coreml)** | CoreML `.mlpackage` | ![bf16][badge-bf16] | 14.74 GB | 4 transformer blocks (~3.5 GB each) + embed + 1024×1024 VAE decoder. |
| **[themindstudio/Qwen-Image-2.1-MLX-4bit](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit)** | MLX | ![int4][badge-int4] | 11.59 GB | Smallest viable Apple build. |

<p id="port-mnn" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Mobile &amp; edge (MNN)

Alibaba MNN runtime, for on-device inference. The full repos are large — the MNN build bundles the text encoder, DiT, and VAE together.

| Repo | Precision | Size | Notes |
| :--- | :--- | ---: | :--- |
| **[yunfengwang/Qwen-Image-2.1-MNN-fp16](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-fp16)** | ![fp16][badge-fp16] | 30.72 GB | Highest fidelity mobile build — and by far the largest. |
| **[yunfengwang/Qwen-Image-2.1-MNN-int8](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int8)** | ![int8][badge-int8] | 21.42 GB | |
| **[yunfengwang/Qwen-Image-2.1-MNN-int4](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int4)** | ![int4][badge-int4] | 14.39 GB | |
| **[evankuo/Qwen-Image-2.1-MNN](https://huggingface.co/evankuo/Qwen-Image-2.1-MNN)** | ![int4][badge-int4] | 10.62 GB | `llm.mnn.weight` 4.73 GB + `dit.mnn.weight` 4.47 GB + VAE 0.51 GB. |

<p id="port-alt" align="center">· · · · · · · · · · · · · ·</p>

### ▣ AMD &amp; domestic accelerators

**[changh95/qwen-image-2.1-p150](https://huggingface.co/changh95/qwen-image-2.1-p150)** — the standout entry here. A full port of Qwen-Image 2.1 to a **single Tenstorrent Blackhole p150a** via `tt-nn`, with all three sub-models resident on-chip. ~20.5 s for a 40-step 1024² generation (513 ms/step sustained), and it beats an RTX 5090 with CPU offload by 1.4–1.6× end to end. Includes editing support for 1–4 condition images. No weights are redistributed — it pulls the official ones.

**[kingjones777/Qwen-Image-2.1-ROCm-gfx1151](https://huggingface.co/kingjones777/Qwen-Image-2.1-ROCm-gfx1151)** — AMD ROCm build for `gfx1151` (Strix Halo / RX 9070-class iGPU). Code only; no weights in the repo.

**[FlagRelease/Qwen-Image-2.1](https://huggingface.co/FlagRelease)** — eight BF16/W8A8 builds targeting FlagOS on Chinese NPUs. Same 28-file layout in every one; pick by target:

| Repo | Target | Precision | Size |
| :--- | :--- | :--- | ---: |
| **[Qwen-Image-2.1-BF16-nvidia-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-nvidia-FlagOS)** | NVIDIA (FlagOS path) | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-hygon-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-hygon-FlagOS)** | Hygon DCU | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-ascend-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-ascend-FlagOS)** | Ascend NPU | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-metax-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-metax-FlagOS)** | MetaX CGC | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-enflame-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-enflame-FlagOS)** | Enflame GCU | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-zhenwu-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-zhenwu-FlagOS)** | Zhenwu MUSA | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-BF16-mthreads-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-mthreads-FlagOS)** | Moore Threads MUSA | ![bf16][badge-bf16] | 33.13 GB |
| **[Qwen-Image-2.1-W8A8-arm-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-W8A8-arm-FlagOS)** | ARM | ![w8a8][badge-w8a8] | 29.38 GB |

<p id="port-vae" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Experimental VAE

**[471Def/qwen_image_2.1_hdr_vae_test](https://huggingface.co/471Def/qwen_image_2.1_hdr_vae_test)** — an HDR VAE test build. Untested in the wild; treat as an experiment, not a drop-in replacement.

| Repo | Precision | Size |
| :--- | :--- | ---: |
| **[471Def/qwen_image_2.1_hdr_vae_test](https://huggingface.co/471Def/qwen_image_2.1_hdr_vae_test)** | ![fp16][badge-fp16] | 0.68 GB |

<p id="tools" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚙ Tools &amp; notebooks

| Repo | Type | Notes |
| :--- | :--- | :--- |
| **[SimpleTuner/Qwen-Image-2.1-training-assistant-v1](https://huggingface.co/SimpleTuner/Qwen-Image-2.1-training-assistant-v1)** | LoRA for training | A working `pytorch_lora_weights.safetensors` + `training_details.json` you can inspect to learn a real SimpleTuner Qwen-Image-2.1 config. Trained on `webshart/cc12m-structured-captions`. |
| **[bluemorpholimited/qwen_image2.1_colab](https://huggingface.co/bluemorpholimited/qwen_image2.1_colab)** | Colab notebook | 7-cell notebook with form UI and `enable_model_cpu_offload()`. Works on L4 22 GB; **T4 16 GB will OOM by design**. ~33 GB download on first run. |
| **[bluemorpholimited/qwen_image2.1_molab](https://huggingface.co/bluemorpholimited/qwen_image2.1_molab)** | Marimo MoLab | Script version of the above, tuned for Marimo and Blackwell. |
| **[iamvts/Qwen-Image-2.1-Skills](https://huggingface.co/iamvts/Qwen-Image-2.1-Skills)** | Agent skill | `qwen-image-2-1-amateur-photography` skill that turns a short scene into a structured prompt for believable casual phone photography. 22 example images. |
| **[changh95/qwen-image-2.1-p150](https://huggingface.co/changh95/qwen-image-2.1-p150)** | Inference port | Tenstorrent Blackhole p150a. `tt-model pull --with-weights` then `tt-model serve`. Port code is Apache-2.0; weights stay under the Qwen Research License. |

**Gotchas that will save you an afternoon**

1. `QwenImage21Pipeline` requires diffusers **`main`** — release `0.40.0` does not have it. Install from git.
2. The CFG kwarg is **`true_cfg_scale`**, not `guidance_scale`, and it needs a `negative_prompt` to engage.
3. Width and height must be **multiples of 32**, max edge 2048.
4. With CPU offload, the generator should live on **`'cpu'`**.
5. The default sample is **40 steps with no CFG**. Adding CFG changes the look; don't assume more steps help.
6. Chinese prompts work natively — the PE models are what rewrite them into English.

---
<div align="center">

**Awesome Qwen-Image 2.1**

<sub>95 unique repositories, verified against the Hugging Face API.</sub>

</div>

<!-- MARKDOWN LINK REFERENCES -->
[hf-shield]: https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black
[hf-url]: https://huggingface.co/Qwen/Qwen-Image-2.1
[lic-shield]: https://img.shields.io/badge/model%20license-Qwen%20Research-yellow?style=for-the-badge
[lic-url]: https://huggingface.co/Qwen/Qwen-Image-2.1/blob/main/LICENSE
[commit-shield]: https://img.shields.io/badge/docs-auto--generated-blue?style=for-the-badge
[commit-url]: https://github.com/QwenLM/Qwen-Image-2.1
[prs-shield]: https://img.shields.io/badge/PRs-welcome-brightgreen?style=for-the-badge
[prs-url]: https://github.com/QwenLM/Qwen-Image-2.1/pulls

[badge-bf16]: https://img.shields.io/badge/bf16-0077cc?style=flat-square
[badge-fp16]: https://img.shields.io/badge/fp16-0077cc?style=flat-square
[badge-fp32]: https://img.shields.io/badge/fp32-6c757d?style=flat-square
[badge-fp8]: https://img.shields.io/badge/fp8-28a745?style=flat-square
[badge-fp4]: https://img.shields.io/badge/fp4-20c997?style=flat-square
[badge-nvfp4]: https://img.shields.io/badge/nvfp4-6f42c1?style=flat-square
[badge-mxfp4]: https://img.shields.io/badge/mxfp4-6f42c1?style=flat-square
[badge-mxfp8]: https://img.shields.io/badge/mxfp8-20c997?style=flat-square
[badge-int8]: https://img.shields.io/badge/int8-17a2b8?style=flat-square
[badge-int4]: https://img.shields.io/badge/int4-ffc107?style=flat-square
[badge-w4a8]: https://img.shields.io/badge/w4a8-fe7d37?style=flat-square
[badge-w8a8]: https://img.shields.io/badge/w8a8-fe7d37?style=flat-square

[badge-Q2]: https://img.shields.io/badge/Q2-e05d44?style=flat-square
[badge-Q3]: https://img.shields.io/badge/Q3-fe7d37?style=flat-square
[badge-Q4]: https://img.shields.io/badge/Q4-dfb317?style=flat-square
[badge-Q5]: https://img.shields.io/badge/Q5-97c00f?style=flat-square
[badge-Q6]: https://img.shields.io/badge/Q6-0077cc?style=flat-square
[badge-Q8]: https://img.shields.io/badge/Q8-28a745?style=flat-square

[badge-Q2_K]: https://img.shields.io/badge/Q2__K-e05d44?style=flat-square
[badge-Q3_K_M]: https://img.shields.io/badge/Q3__K__M-fe7d37?style=flat-square
[badge-Q3_K_S]: https://img.shields.io/badge/Q3__K__S-fe7d37?style=flat-square
[badge-Q3_K_XL]: https://img.shields.io/badge/Q3__K__XL-ff3b30?style=flat-square
[badge-Q4_0]: https://img.shields.io/badge/Q4__0-dfb317?style=flat-square
[badge-Q4_1]: https://img.shields.io/badge/Q4__1-dfb317?style=flat-square
[badge-Q4_K_M]: https://img.shields.io/badge/Q4__K__M-dfb317?style=flat-square
[badge-Q4_K_S]: https://img.shields.io/badge/Q4__K__S-dfb317?style=flat-square
[badge-Q5_0]: https://img.shields.io/badge/Q5__0-97c00f?style=flat-square
[badge-Q5_1]: https://img.shields.io/badge/Q5__1-97c00f?style=flat-square
[badge-Q5_K_M]: https://img.shields.io/badge/Q5__K__M-97c00f?style=flat-square
[badge-Q5_K_S]: https://img.shields.io/badge/Q5__K__S-97c00f?style=flat-square
[badge-Q6_K]: https://img.shields.io/badge/Q6__K-0077cc?style=flat-square
[badge-Q8_0]: https://img.shields.io/badge/Q8__0-28a745?style=flat-square
[badge-IQ1_S]: https://img.shields.io/badge/IQ1__S-b02a37?style=flat-square
[badge-IQ1_M]: https://img.shields.io/badge/IQ1__M-d64545?style=flat-square
[badge-UD-Q2_K_XL]: https://img.shields.io/badge/UD-Q2__K__XL-e05d44?style=flat-square
[badge-UD-Q3_K_XL]: https://img.shields.io/badge/UD-Q3__K__XL-fe7d37?style=flat-square
