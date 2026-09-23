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
* [Prompt engines](#prompt)
  * [Official PE models](#prompt-official)
  * [Compact rewriters](#prompt-pocket)
  * [Heretic & abliterated PE](#prompt-heretic)
  * [Quantized & ported PE](#prompt-quant)
* [Text encoders](#textenc)
* [Quantizations](#quant)
  * [GGUF](#quant-gguf)
  * [FP4 / NVFP4 / MXFP4](#quant-fp4)
  * [INT8 & INT4](#quant-int)
  * [FP8 & bf16](#quant-fp8)
  * [Nunchaku (SVDQ)](#quant-nunchaku)
* [Turbo & step distillation](#turbo)
* [LoRA & adapters](#lora)
* [Platform ports](#port)
  * [Apple Silicon](#port-apple)
  * [Mobile & edge (MNN)](#port-mnn)
  * [AMD & domestic accelerators](#port-alt)
  * [Experimental VAE](#port-vae)
* [Tools & notebooks](#tools)
* [Coverage](#coverage)
* [Contributing](#contributing)
* [License](#license)

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

---

<a id="official"></a>

## ▓ Official checkpoints

The reference weights. Start here before touching any community conversion.

<a id="official-base"></a>

### ▣ Base model

| Repo | Precision | Layout | DiT | Text encoder | VAE | Notes |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **[Qwen/Qwen-Image-2.1](https://huggingface.co/Qwen/Qwen-Image-2.1)** | BF16 | diffusers | 14.23 GB | 17.53 GB (Qwen3-VL-8B) | 1.35 GB | Official. `QwenImage21Pipeline`. |
| **[KasugaiSakura/Qwen-Image-2.1-Original](https://huggingface.co/KasugaiSakura/Qwen-Image-2.1-Original)** | BF16 | diffusers | 14.23 GB | 17.53 GB | 1.35 GB | Unmodified mirror, same shard layout. |

<a id="official-comfy"></a>

### ▣ ComfyUI official

**[Comfy-Org/Qwen-Image-2.1](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** — the Day-0 ComfyUI repackage. Files land directly in `models/` with no renaming.

| File | Size | Place in |
| :--- | ---: | :--- |
| `diffusion_models/qwen_image_2.1_bf16.safetensors` | 14.23 GB | `models/diffusion_models/` |
| `diffusion_models/qwen_image_2.1_int8_convrot.safetensors` | 7.26 GB | `models/diffusion_models/` |
| `text_encoders/qwen3vl_8b_bf16.safetensors` | 17.53 GB | `models/text_encoders/` |
| `text_encoders/qwen3vl_8b_int8_convrot.safetensors` | 9.35 GB | `models/text_encoders/` |
| `text_encoders/qwen3vl_8b_w4a8.safetensors` | 6.31 GB | `models/text_encoders/` |
| `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_t2i.int8_convrot.safetensors` | 9.47 GB | `models/text_encoders/` |
| `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors` | 9.47 GB | `models/text_encoders/` |
| `vae/qwen_image_2.1_vae_bf16.safetensors` | 0.68 GB | `models/vae/` |

> [!TIP]
> `ConvRot` files are ComfyUI's native rotated-channel integer format. Use a recent ComfyUI build and load them with the standard diffusion-model and text-encoder loaders — no custom nodes required.

---

<a id="prompt"></a>

## ✦ Prompt engines (PE)

Qwen-Image 2.1 ships with dedicated **prompt-rewriting** models. They are separate 9B text models that take a short request in any language and return a detailed English prompt plus a recommended aspect ratio. They are *not* part of the image pipeline itself — you can use the base model without them.

<a id="prompt-official"></a>

### ▣ Official PE models

| Repo | Task | Size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[Qwen/Qwen-Image-2.1-PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)** | text → image | 18.82 GB | 2.6k | Fine-tuned Qwen3.5-VL 9B. Returns `{rewritten_prompt, wh_ratio}`. |
| **[Qwen/Qwen-Image-2.1-PE-I2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-I2I)** | image → image | 18.82 GB | 6.2k | Identical shard layout; editing-oriented rewriting. |

Both ship a `system_prompt.txt` and work with `AutoModelForCausalLM` + `AutoTokenizer`. Output is JSON after a reasoning block, so split on the think-tag before parsing.

<a id="prompt-pocket"></a>

### ▣ Compact rewriters

Much smaller prompt rewriters from ML-Intern Lab, fine-tuned from Qwen3.5 base models instead of the 9B VL model. Roughly 5× smaller than the official rewriter.

| Repo | Params | Size | Downloads | Notes |
| :--- | :---: | ---: | ---: | :--- |
| **[ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B)** | 2B | 3.76 GB | 771 | Best size/quality tradeoff for local rewriting. |
| **[ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B)** | 0.8B | 1.50 GB | 990 | Smallest; also ships a `Q8_0` GGUF (0.81 GB) for llama.cpp. |

<a id="prompt-heretic"></a>

### ▣ Heretic & abliterated PE

> [!WARNING]
> These repos have the safety refusal direction **abliterated** from the PE rewriter. The rewriter no longer refuses prompt content, which means it will happily rewrite anything you ask. Same Qwen Research License as the base weights — the license does not grant you additional rights.

| Repo | Task | Format | Size | Downloads | Notes |
| :--- | :--- | :--- | ---: | ---: | :--- |
| **[pottokao/Qwen-Image-2.1-PE-T2I-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-T2I-Heretic-GGUF)** | text → image | GGUF Q4_K_M | 5.93 GB | 5.5k | Most-downloaded Heretic rewriter. |
| **[pottokao/Qwen-Image-2.1-PE-I2I-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-I2I-Heretic-GGUF)** | image → image | GGUF Q4_K_M | 6.81 GB | 2.4k | Includes an `mmproj` projector for llama.cpp. |
| **[t8star/qwen-image-2.1-comfy](https://huggingface.co/t8star/qwen-image-2.1-comfy)** | both | GGUF Q4_K_M | 18.07 GB | 1.8k | Bundle of PE-T2I + PE-I2I + heretic T2I, ComfyUI-oriented naming. |
| **[base11231/Qwen-Image-2.1-PE-I2I-Abliterated](https://huggingface.co/base11231/Qwen-Image-2.1-PE-I2I-Abliterated)** | image → image | safetensors | 18.84 GB | — | Editing-only abliteration, not GGUF. |
| **[darrellbest/Qwen-Image-2.1-PE-T2I-Heretic](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic)** | text → image | safetensors | 18.84 GB | 39 | Source for the NVFP4 build below. |
| **[darrellbest/Qwen-Image-2.1-PE-I2I-Heretic](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic)** | image → image | safetensors | 18.84 GB | 103 | Source for the NVFP4 build below. |
| **[darrellbest/Qwen-Image-2.1-PE-T2I-Heretic-NVFP4](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic-NVFP4)** | text → image | NVFP4 | 11.22 GB | 13 | 4-bit NF4. |
| **[darrellbest/Qwen-Image-2.1-PE-I2I-Heretic-NVFP4](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic-NVFP4)** | image → image | NVFP4 | 11.22 GB | 29 | 4-bit NF4. |

<a id="prompt-quant"></a>

### ▣ Quantized & ported PE

| Repo | Format | Size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[HarleyWang/Qwen-Image-2.1-PE-ComfyUI](https://huggingface.co/HarleyWang/Qwen-Image-2.1-PE-ComfyUI)** | BF16 + int8 ConvRot | 64.69 GB | — | PE-T2I and PE-I2I at two precisions, ComfyUI naming. |
| **[prithivMLmods/Qwen-Image-2.1-PE-T2I-MLX](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-T2I-MLX)** | MLX (4/8/16-bit) | 35.26 GB | 584 | Apple Silicon. |
| **[prithivMLmods/Qwen-Image-2.1-PE-I2I-MLX](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-I2I-MLX)** | MLX (4/8/16-bit) | 35.26 GB | 518 | Apple Silicon. |
| **[foofifoo/Qwen-Image-2.1-Prompt-Enhancement-INT8-Convrot](https://huggingface.co/foofifoo/Qwen-Image-2.1-Prompt-Enhancement-INT8-Convrot)** | int8 ConvRot | 24.69 GB | — | Both PE-T2I and PE-I2I in one repo — the only single-download option for both. |
| **[diffnamehard/Qwen-Image-2.1-PE-T2I-Heretic-int8-tensorwise-convrot](https://huggingface.co/diffnamehard/Qwen-Image-2.1-PE-T2I-Heretic-int8-tensorwise-convrot)** | int8 tensorwise ConvRot | 10.01 GB | 44 | Heretic rewriter, tensorwise per-channel scaling. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ3-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ3-G64)** | MLX oQ3 G64 | — | — | ⚠️ **Empty repo** — only `.gitattributes`. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ4-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ4-G64)** | MLX oQ4 G64 | — | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ5-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ5-G64)** | MLX oQ5 G64 | — | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ6-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ6-G64)** | MLX oQ6 G64 | — | — | ⚠️ **Empty repo** — no weights uploaded. |
| **[groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ8-G64](https://huggingface.co/groxaxo/Qwen-Image-2.1-PE-I2I-Heretic-MLX-oQ8-G64)** | MLX oQ8 G64 | — | — | ⚠️ **Empty repo** — no weights uploaded. |

---

<a id="textenc"></a>

## ⚲ Text encoders

Qwen-Image 2.1 uses **Qwen3-VL-8B** as its text encoder. At BF16 this is the largest file in the whole stack (17.53 GB), so quantizing it is often the highest-leverage move. The "Heretic" line below has the refusal direction ablated from the encoder.

| Repo | Format | Size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF)** | GGUF Q4_K_M + mmproj | 5.03 GB | 74.3k | The most-used TE in this list. Also ships fp8 (9.34 GB) and bf16 (17.53 GB) in the same repo, plus a 1.16 GB `mmproj` projector. |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-NVFP4](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-NVFP4)** | NVFP4 | 6.31 GB | — | 4-bit NF4, diffusers format. |
| **[pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-int8-convrot](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-int8-convrot)** | int8 ConvRot | 9.35 GB | — | ComfyUI ConvRot format, same size as the official int8 TE. |
| **[Karsus1997/Qwen-Image-2.1-Text-Encoder-Heretic-W4A8](https://huggingface.co/Karsus1997/Qwen-Image-2.1-Text-Encoder-Heretic-W4A8)** | W4A8 | 6.31 GB | — | 4-bit weights, 8-bit activations. |
| **[kkxao/Qwen-Image-2.1-Text-Encoder-Heretic](https://huggingface.co/kkxao/Qwen-Image-2.1-Text-Encoder-Heretic)** | BF16 | 17.53 GB | 17 | ⚠️ Abliterated encoder — no refusal. Base is Qwen3-VL-8B-Instruct. |

---

<a id="quant"></a>

## ◈ Quantizations

Every community conversion of the base DiT, grouped by format. Sizes are the total weight bytes per repo. Distilled turbo GGUFs live under [Turbo](#turbo) instead.

<a id="quant-gguf"></a>

### ▣ GGUF

For llama.cpp and anything that reads GGUF. Q4_K_M is the usual quality/size balance point.

| Repo | Quants | Total size | Downloads | Likes | Notes |
| :--- | :--- | ---: | ---: | ---: | :--- |
| **[unsloth/Qwen-Image-2.1-GGUF](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** | Q2_K → Q8_0 (12 files) | 64.83 GB | 26.7k | 143 | Widest quant spread available, includes F16. |
| **[abenzerps/Qwen-Image-2.1-Uncensored-GGUF](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF)** | Q4_0 → Q8_0 + BF16/fp8/int8 | 83.61 GB | 350.7k | 1.4k | ⚠️ Uncensored. The single most-downloaded repo in this list. |
| **[realrebelai/Qwen-Image-2.1_GGUFs](https://huggingface.co/realrebelai/Qwen-Image-2.1_GGUFs)** | Q2, Q3, Q4, Q5, Q8 | 28.75 GB | 10.1k | 34 | Five-step ladder. |
| **[vantagewithai/Qwen-Image-2.1-ComfyUI-GGUF](https://huggingface.co/vantagewithai/Qwen-Image-2.1-ComfyUI-GGUF)** | Q3_K_M → Q8_0 | 25.94 GB | 1.8k | 7 | ComfyUI-oriented. |
| **[ped4enko/Qwen-Image-2.1-Dessi](https://huggingface.co/ped4enko/Qwen-Image-2.1-Dessi)** | Q4_0 → Q8_0 | 54.91 GB | 66 | 1 | Ships TE safetensors + VAE alongside. |
| **[0xSojalSec/Qwen-Image-2.1-Uncensored-HF](https://huggingface.co/0xSojalSec/Qwen-Image-2.1-Uncensored-HF)** | Q4_0 → Q8_0 + BF16 | 69.24 GB | 96 | 4 | ⚠️ Uncensored. Same UC weights as abenzerps plus safetensors TEs. |
| **[pottokao/Qwen-Image-2.1-DiT-GGUF](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-GGUF)** | Q4_K_M, Q6_K, Q8_0 | 18.02 GB | 4 | — | DiT only, pairs with a separate TE. |
| **[gguf-org/qwen-image-2.1-gguf](https://huggingface.co/gguf-org/qwen-image-2.1-gguf)** | Q4_K_M + NVFP4 | 17.48 GB | 636 | 3 | **Transferred from `chatpig`** — the old URL redirects here. Ships the TE `mmproj` (0.75 GB), TE quants, and VAE GGUFs. |
| **[zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed](https://huggingface.co/zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed)** | Q4 HVQ3 | 5.96 GB | 25 | — | For **sd.cpp**, not stock llama.cpp. |

<a id="quant-fp4"></a>

### ▣ FP4 / NVFP4 / MXFP4

Native 4-bit floating point. Best quality per bit on recent NVIDIA hardware (Blackwell) and RDNA4.

| Repo | Format | Total size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[EliovpAI/Qwen_Image-2.1-MXFP4](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4)** | MXFP4 (Paiton) | 9.31 GB | — | Paiton backend, 57 shards. |
| **[EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4)** | MXFP4 RDNA4 | 9.31 GB | — | RDNA4-specific kernel variant, identical layout. |
| **[EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton](https://huggingface.co/EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton)** | MXFP4 Paiton | 9.31 GB | — | ⚠️ Uncensored, derived from abenzerps GGUF. |
| **[Rin247/Qwen-Image-2.1-FP4](https://huggingface.co/Rin247/Qwen-Image-2.1-FP4)** | FP4 | 11.74 GB | 58 | Full diffusers repo (DiT 6.46 + TE 4.94 + VAE 0.34). |
| **[ModelsLab/Qwen-Image-2.1-W4A4-nvfp4](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4)** | NVFP4 W4A4 | 4.88 GB | 70 | DiT only (4.88 GB), full diffusers repo otherwise. |
| **[pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI)** | NVFP4 | 13.82 GB | — | Three ComfyUI cuts: `nvfp4`, `nvfp4_T2`, `nvfp4_T3`. |
| **[HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-W4A4-NVFP4](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-W4A4-NVFP4)** | W4A4 NVFP4 | 23.66 GB | 33 | NVIDIA ModelOpt-derived, full repo. |

<a id="quant-int"></a>

### ▣ INT8 & INT4

ConvRot is ComfyUI's native rotated-channel integer format — load these with the standard loaders on a recent build.

| Repo | Format | Total size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI](https://huggingface.co/chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI)** | bf16 + int8 + int4 ConvRot + W4A8 | 65.91 GB | 119 | **Best all-in-one ComfyUI pack.** DiT and TE at three precisions plus VAE, in correct folder layout. |
| **[ModelsLab/Qwen-Image-2.1-W4A4-int4](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4)** | INT4 W4A4 | 4.66 GB | 221 | DiT only; NVFP4 sibling is the near-identical alternative. |
| **[addlabsviral/Qwen-Image-2.1-4bit](https://huggingface.co/addlabsviral/Qwen-Image-2.1-4bit)** | 4-bit | 11.41 GB | 38 | Full diffusers repo. |
| **[circulus/Qwen-Image-2.1-bnb-4bit](https://huggingface.co/circulus/Qwen-Image-2.1-bnb-4bit)** | bitsandbytes NF4 | 11.41 GB | 28 | Standard bnb 4-bit. |
| **[Rin247/Qwen-Image-2.1-INT4](https://huggingface.co/Rin247/Qwen-Image-2.1-INT4)** | INT4 | 11.08 GB | 53 | Full diffusers repo. |
| **[Rin247/Qwen-Image-2.1-INT8](https://huggingface.co/Rin247/Qwen-Image-2.1-INT8)** | INT8 | 17.96 GB | 116 | Full diffusers repo. |

<a id="quant-fp8"></a>

### ▣ FP8 & bf16

| Repo | Format | Total size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8)** | FP8 | 26.33 GB | 35 | NVIDIA ModelOpt-derived, full repo. |
| **[dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor)** | BF16 | 14.23 GB | — | ⚠️ Uncensored BF16, single file. |
| **[Rin247/Qwen-Image-2.1-FP8](https://huggingface.co/Rin247/Qwen-Image-2.1-FP8)** | FP8 | 17.96 GB | 95 | Full diffusers repo — closest thing to a drop-in smaller BF16. |
| **[mingyi456/Qwen-Image-2.1-DF11-ComfyUI](https://huggingface.co/mingyi456/Qwen-Image-2.1-DF11-ComfyUI)** | BF16 single-file | 9.72 GB | 28 | `qwen_image_2.1_bf16-DF11.safetensors` for ComfyUI. |
| **[RunningHubAI/rh-qwen-image-2.1-bf16-unet](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet)** | — | — | — | ⚠️ **Empty placeholder** — no files uploaded yet. |

<a id="quant-nunchaku"></a>

### ▣ Nunchaku (SVDQ)

SVDQ-based 4-bit for Nunchaku, which targets low-VRAM systems and 4090-class cards.

| Repo | Format | Total size | Notes |
| :--- | :--- | ---: | :--- |
| **[catplusplus/nunchaku-qwen-image-2.1](https://huggingface.co/catplusplus/nunchaku-qwen-image-2.1)** | SVDQ FP4 | 8.75 GB | Two builds: `best_quality_fp4` and `svdq-fp4_r32`. |
| **[BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4](https://huggingface.co/BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4)** | INT4 | 4.15 GB | "Lite" Nunchaku variant — half the size of the FP4 build. |

---

<a id="turbo"></a>

## ⚡ Turbo & step distillation

4-step generation. Useful for iteration and batch work; expect some quality loss versus 40 steps.

| Repo | Kind | Steps | Size | Downloads | Likes | Notes |
| :--- | :--- | ---: | ---: | ---: | ---: | :--- |
| **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** | Full distill + LoRAs | 4 / 5 | 19.33 GB | 1.5k | 119 | Source repo. `4step-lora-r64` and `5step-lora-r256` PEFT adapters plus a full `transformer/`. |
| **[realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs)** | GGUF | 4 | 34.07 GB | 553 | 8 | Turbo in GGUF, Q2_K → Q8_0. |
| **[Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF)** | GGUF | 4 | 29.91 GB | — | 15 | Alternate turbo GGUF set, Q3_K_M → Q8_0. |
| **[RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora)** | LoRA | 4 | 0.34 GB | — | — | Rank-64 Viggle turbo LoRA, ComfyUI naming. |
| **[RunningHubAI/rh-qwen-image-2.1-turbo-4step-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-turbo-4step-lora)** | LoRA | 4 | — | — | — | ⚠️ **Empty placeholder** — created 2026-09-23, no files yet. |

---

<a id="lora"></a>

## ◉ LoRA & adapters

Style, control, and fix adapters. All target `Qwen/Qwen-Image-2.1` unless noted.

| Repo | Type | Size | Likes | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[e-n-v-y/Qwen-Image-2.1-Fix](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix)** | Quality fix | 0.11 GB | 38 | `qwen-image-2.1-fix-1.0-comfy.safetensors`. The most-liked community LoRA. |
| **[RunningHubAI/rh-qwen-image-2.1ai-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora)** | Style | 2.45 GB | — | 8-file "remove the AI look" pack (CN filenames). |
| **[prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview)** | Control | 0.50 GB | 1 | Bbox object *moving*, 6 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview)** | Control | 0.50 GB | 1 | Bbox object removal, full-quality variant, 6 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA)** | Style | 0.42 GB | 2 | Exposure correction, 5 checkpoints. |
| **[prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo)** | Control | 0.42 GB | 1 | Bbox removal, 4-step-compatible variant. |
| **[Aero-Ex/Qwen-Image2.1_Normal2RGB](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB)** | Utility | 0.25 GB | 2 | Normal map → RGB render, 3 checkpoints (2000/3000/4000). |
| **[Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer)** | Style | 0.10 GB | — | `deckbuilder_cardart_style_lora_v1_fp16`. |
| **[AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0)** | ⚠️ NSFW | 0.17 GB | 5 | Character-style LoRA. |
| **[RunningHubAI/rh-qwen-image-2.1-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora)** | General | — | — | ⚠️ **Empty placeholder** — no files uploaded. |
| **[RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora)** | ⚠️ NSFW | — | — | ⚠️ **Empty placeholder** — no files uploaded. |
| **[RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora)** | ⚠️ NSFW | — | — | ⚠️ **Empty placeholder** — no files uploaded. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used — the uncensored GGUF in particular is the most-downloaded repo here. They carry the same **Qwen Research License** as the base model; a research license is not a license to do whatever you want, and you remain responsible for how you use them. Several are empty placeholders created on release day — check before planning around them.

---

<a id="port"></a>

## ⬡ Platform ports

Non-CUDA runtimes and specialized accelerator backends.

<a id="port-apple"></a>

### ▣ Apple Silicon

| Repo | Format | Size | Downloads | Notes |
| :--- | :--- | ---: | ---: | :--- |
| **[JoyFusionAI/Qwen-Image-2.1-MLX-8bit](https://huggingface.co/JoyFusionAI/Qwen-Image-2.1-MLX-8bit)** | MLX 8-bit (mflux) | 24.04 GB | 787 | For [mflux](https://github.com/filipstrand/mflux). 13 TE shards. |
| **[devin-lai/Qwen-Image-2.1-Coreml](https://huggingface.co/devin-lai/Qwen-Image-2.1-Coreml)** | CoreML `.mlpackage` | 14.74 GB | 39 | 4 transformer blocks (~3.5 GB each) + embed + 1024×1024 VAE decoder. |
| **[themindstudio/Qwen-Image-2.1-MLX-4bit](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit)** | MLX 4-bit | 11.59 GB | 76 | Smallest viable Apple build. |

<a id="port-mnn"></a>

### ▣ Mobile & edge (MNN)

Alibaba MNN runtime, for on-device inference. Note the full repos are large — the MNN build bundles the text encoder, DiT, and VAE together.

| Repo | Precision | Size | Notes |
| :--- | :--- | ---: | :--- |
| **[evankuo/Qwen-Image-2.1-MNN](https://huggingface.co/evankuo/Qwen-Image-2.1-MNN)** | int4 | 10.62 GB | Full repo: `llm.mnn.weight` 4.73 GB + `dit.mnn.weight` 4.47 GB + VAE 0.51 GB. |
| **[yunfengwang/Qwen-Image-2.1-MNN-fp16](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-fp16)** | fp16 | 30.72 GB | Highest fidelity mobile build — and by far the largest. |
| **[yunfengwang/Qwen-Image-2.1-MNN-int8](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int8)** | int8 | 21.42 GB | |
| **[yunfengwang/Qwen-Image-2.1-MNN-int4](https://huggingface.co/yunfengwang/Qwen-Image-2.1-MNN-int4)** | int4 | 14.39 GB | |

<a id="port-alt"></a>

### ▣ AMD & domestic accelerators

**[changh95/qwen-image-2.1-p150](https://huggingface.co/changh95/qwen-image-2.1-p150)** — the standout entry here. A full port of Qwen-Image 2.1 to a **single Tenstorrent Blackhole p150a** via `tt-nn`, with all three sub-models resident on-chip. ~20.5 s for a 40-step 1024² generation (513 ms/step sustained), and it beats an RTX 5090 with CPU offload by 1.4–1.6× end to end. Includes editing support for 1–4 condition images. No weights are redistributed — it pulls the official ones.

**[kingjones777/Qwen-Image-2.1-ROCm-gfx1151](https://huggingface.co/kingjones777/Qwen-Image-2.1-ROCm-gfx1151)** — AMD ROCm build for `gfx1151` (Strix Halo / RX 9070-class iGPU).

**[FlagRelease/Qwen-Image-2.1](https://huggingface.co/FlagRelease)** — eight BF16/W8A8 builds targeting FlagOS on Chinese NPUs. Same 28-file layout in every one; pick by target:

| Repo | Target | Precision | Size |
| :--- | :--- | :--- | ---: |
| **[Qwen-Image-2.1-BF16-nvidia-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-nvidia-FlagOS)** | NVIDIA (FlagOS path) | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-hygon-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-hygon-FlagOS)** | Hygon DCU | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-ascend-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-ascend-FlagOS)** | Ascend NPU | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-metax-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-metax-FlagOS)** | MetaX CGC | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-enflame-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-enflame-FlagOS)** | Enflame GCU | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-zhenwu-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-zhenwu-FlagOS)** | Zhenwu MUSA | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-BF16-mthreads-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-BF16-mthreads-FlagOS)** | Moore Threads MUSA | BF16 | 33.13 GB |
| **[Qwen-Image-2.1-W8A8-arm-FlagOS](https://huggingface.co/FlagRelease/Qwen-Image-2.1-W8A8-arm-FlagOS)** | ARM | W8A8 (60 files) | 29.38 GB |

<a id="port-vae"></a>

### ▣ Experimental VAE

**[471Def/qwen_image_2.1_hdr_vae_test](https://huggingface.co/471Def/qwen_image_2.1_hdr_vae_test)** — an HDR VAE test build (0.68 GB, fp16). Untested in the wild; treat as an experiment, not a drop-in replacement.

---

<a id="tools"></a>

## ⚙ Tools & notebooks

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
