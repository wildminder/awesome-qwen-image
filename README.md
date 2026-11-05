# Awesome Qwen-Image 2.1

A curated list of checkpoints, quants, prompt engines, LoRAs, and tooling for **Qwen-Image 2.1**, Alibaba's 7B unified text-to-image and image-editing model.

<div align="center">

<img alt="awesome-qwen-image" src="https://github.com/user-attachments/assets/ea5c1e84-e58b-498f-9627-15e63154059d" />

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
  * [Official Turbo](#official-turbo)
  * [ComfyUI official](#official-comfy)
* [Text encoders &amp; prompt engines](#encoders)
  * [Prompt rewriters](#pe)
  * [Heretic &amp; abliterated](#heretic)
  * [Quantized &amp; ported](#pe-quant)
* [Quantizations](#quant)
  * [GGUF](#quant-gguf)
  * [4-bit &amp; 8-bit](#quant-lowbit)
  * [FP8 &amp; bf16](#quant-fp8)
  * [Nunchaku (SVDQ)](#quant-nunchaku)
  * [EXL3](#quant-exl3)
* [Turbo &amp; step distillation](#turbo)
  * [Turbo GGUF](#turbo-gguf)
  * [Official &amp; converted](#turbo-models)
* [LoRA &amp; adapters](#lora)
* [Platform ports](#port)
  * [Apple Silicon](#port-apple)
  * [Mobile &amp; edge (MNN)](#port-mnn)
  * [AMD &amp; domestic accelerators](#port-alt)
  * [ComfyUI package formats](#port-pkg)
  * [Experimental VAE](#port-vae)
* [Tools &amp; notebooks](#tools)
* [Contributing](#contributing)

</details>

---

<a id="quick-start"></a>

## ⌬ Quick start

Pick your entry point based on the runtime you already have.

| I want to… | Use | Why |
| :--- | :--- | :--- |
| Run the reference model in Diffusers | **[Qwen](https://huggingface.co/Qwen/Qwen-Image-2.1)** | Official BF16 diffusers repo, `QwenImage21Pipeline` |
| Use it in ComfyUI | **[Comfy-Org](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** | Pre-split folder layout, Day-0 native nodes |
| Fit it in 8–12 GB VRAM | **[INT4ConvRot-ComfyUI](https://huggingface.co/chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI)** | Complete ComfyUI pack incl. int4 ConvRot DiT + TE |
| Run locally with llama.cpp / GGUF | **[Unsloth GGUF](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** | Widest quant spread, Q2_K → Q8_0 |
| Generate in 8 steps (official) | **[Qwen-Image-2.1-Turbo](https://huggingface.co/Qwen/Qwen-Image-2.1-Turbo)** | Qwen's own 8-step checkpoint; CFG 1, schedule carried in the weights |
| Generate in 4 steps | **[Viggle Turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** | Community DMD distill + LoRA variants |
| On a Mac (Apple Silicon) | **[MLX-4bit](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit)** | MLX 4-bit, native unified memory |
| Improve prompt quality | **[PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)** | Official prompt rewriter + aspect-ratio picker |

**Official resources**

* [Qwen-Image-2.1 model card](https://huggingface.co/Qwen/Qwen-Image-2.1): official weights, license, aspect-ratio table
* [Qwen-Image-2.1 GitHub repo](https://github.com/QwenLM/Qwen-Image-2.1): inference code, news, supported frameworks
* [Qwen blog: Qwen-Image-2.1](https://qwen.ai/blog?id=qwen-image-2.1): release notes with architecture and benchmark figures
* [ComfyUI native workflow example](https://docs.comfy.org/tutorials/image/qwen/qwen-image-2-1): official ComfyUI tutorial
* [ModelScope mirror](https://modelscope.cn/models/Qwen/Qwen-Image-2.1): official CN mirror

**Model facts worth knowing**

* **7B parameters** in the visual generation component, **32 single-stream DiT layers**.
* Mixed-granularity attention (token-level causal for text, chunk-level for image) with **prefix KV cache reuse** for multi-reference editing.
* Native **RGBA** output: the prompt decides whether the result has an alpha channel.
* Editing accepts **up to 10 reference images**, plus circles, painted annotations, or separate masks for local edits.
* **Qwen3-VL-8B** is the text encoder, so the text encoder is the largest single download at ~17.5 GB BF16.
* Default sample setting is **40 steps without classifier-free guidance**; CFG is available for prompt adherence.

<p id="official" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▓ Official checkpoints

The reference weights. Start here before touching any community conversion.

<p id="official-base" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Base model

| Name | Precision | Layout | DiT | Text encoder | VAE | Links |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Qwen-Image-2.1** | ![bf16][badge-bf16] | diffusers | 14.23 GB | 17.53 GB (Qwen3-VL-8B) | 1.35 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1) |

The reference release, and the only repo you need for a standard Diffusers setup. The DiT and text encoder ship as numbered safetensors shards with an index, so pull the repo rather than a single file.

<p id="official-turbo" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Official Turbo

| Name | Precision | Layout | DiT | Text encoder | VAE | Links |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Qwen-Image-2.1-Turbo** | ![bf16][badge-bf16] | diffusers | 14.23 GB | 17.53 GB (Qwen3-VL-8B) | 0.68 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-Turbo) |

Qwen's own accelerated checkpoint, released **2026-10-09**: the same 7B architecture at **8 steps**, **CFG 1.0**, with the sampling schedule carried *inside the checkpoint*, so the pipeline configures the sampler itself. Prefix KV caching reuses the text and reference context across steps. Needs a diffusers build with pipeline-configured sampling. The ComfyUI pre-split build is the **Turbo** rows in the Comfy-Org table below; the community few-step lines live under [Turbo](#turbo).

<p id="official-comfy" align="center">· · · · · · · · · · · · · ·</p>

### ▣ ComfyUI official

**[Comfy-Org](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** is the Day-0 ComfyUI repackage, covering the base model and the official Turbo side by side. Files land directly in `models/` with no renaming.

| Name | Precision | Size | Links |
| :--- | :---: | :---: | :---: |
| **Image Model** | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_bf16.safetensors) |
| **Image Model** | ![int8][badge-int8] | 7.26 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_int8_convrot.safetensors) |
| **Image Model (Turbo)** | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_turbo_bf16.safetensors) |
| **Image Model (Turbo)** | ![int8][badge-int8] | 7.26 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_turbo_int8_convrot.safetensors) |
| **Text Encoder** | ![bf16][badge-bf16] | 17.53 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_bf16.safetensors) |
| **Text Encoder** | ![int8][badge-int8] | 9.35 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_int8_convrot.safetensors) |
| **Text Encoder** | ![w4a8][badge-w4a8] | 6.31 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_w4a8.safetensors) |
| **Prompt Engine T2I** | ![int8][badge-int8] | 9.47 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3.5_9b_qwen_image_2.1_pe_t2i.int8_convrot.safetensors) |
| **Prompt Engine I2I** | ![int8][badge-int8] | 9.47 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors) |
| **VAE** | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/vae/qwen_image_2.1_vae_bf16.safetensors) |
| **ControlNet Union** | ![bf16][badge-bf16] | 7.55 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/model_patches/qwen_image_2.1_fun_controlnet_union_bf16.safetensors) |
| **ControlNet Union** | ![int8][badge-int8] | 3.78 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/model_patches/qwen_image_2.1_fun_controlnet_union_int8_convrot.safetensors) |
| **Turbo LoRA** | ![bf16][badge-bf16] | 0.91 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/loras/qwen_image_2.1_turbo_lora_avg_rank_178_bf16.safetensors) |

> [!TIP]
> `ConvRot` files are ComfyUI's native rotated-channel integer format; a recent ComfyUI build loads them with the standard diffusion-model and text-encoder loaders, no custom nodes required.
>
> Destination folders: **Image Model** → `models/diffusion_models/`, **Text Encoder** and both **Prompt Engine** rows → `models/text_encoders/`, **VAE** → `models/vae/`, both **ControlNet Union** rows → `models/model_patches/`, **Turbo LoRA** → `models/loras/`.
>
> The two **Image Model (Turbo)** rows are the official 8-step checkpoint pre-split for ComfyUI, same weights as the Qwen repo above. To switch speed at runtime instead, the **Turbo LoRA** row is the adapter extracted from those weights (average rank 178, by Kijai): load it on the BF16 base with `LoraLoaderModelOnly` at strength 1.0, and do not stack it on a Turbo model.
>
> The two ControlNet rows are the Alibaba PAI Union checkpoint repacked, not an official Qwen release; the int8 build is the cheapest ControlNet here by half.

<p id="encoders" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚲ Text encoders &amp; prompt engines

Qwen-Image 2.1 conditions on **Qwen3-VL-8B**, the largest file in the stack at BF16. It also ships dedicated **prompt-rewriting** models: separate text models that take a short request in any language and return a detailed English prompt plus a recommended aspect ratio. Both are optional; the base model runs without either.

<p id="pe" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Prompt rewriters

The official pair is fine-tuned Qwen3.5-VL 9B. The **Pocket** builds are fine-tuned from Qwen3.5 base models instead, roughly 5× smaller and text-to-image only.

| Name | Task | Params | Precision | Size | Links |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **PE-T2I** | ![text → image][task-t2i] | 9B | ![bf16][badge-bf16] | 18.82 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I) |
| **PE-I2I** | ![image → image][task-i2i] | 9B | ![bf16][badge-bf16] | 18.82 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-I2I) |
| **PE-T2I Pocket 2B** | ![text → image][task-t2i] | 2B | ![bf16][badge-bf16] | 3.76 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B) |
| **PE-T2I Pocket 0.8B** | ![text → image][task-t2i] | 0.8B | ![Q8_0][badge-Q8_0] | 1.50 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B) |

The official rewriters ship a `system_prompt.txt` and work with `AutoModelForCausalLM` + `AutoTokenizer`. Output is JSON after a reasoning block, so split on the think-tag before parsing. The T2I rewriter also returns a recommended `wh_ratio` you can map straight to a size.

Each official repo splits into four `model-0000N.safetensors` shards plus a `model.safetensors.index.json`, so download the repo. There is no single-file build to link to. The 0.8B Pocket build also ships a `Q8_0` GGUF (0.81 GB) for llama.cpp.

<p id="heretic" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Heretic &amp; abliterated

> [!WARNING]
> These repos have the safety refusal direction **abliterated**. The model no longer refuses prompt content, which means it will happily rewrite anything you ask. Same Qwen Research License as the base weights, and the license does not grant you additional rights.
>
> Applied to both halves of the text stack: the **text encoder** (the conditioning signal itself no longer refuses) and the **prompt rewriter**.

| Type | Name | Task | Precision | Size | Links |
| :---: | :--- | :---: | :---: | :---: | :---: |
| ![TE][ltype-te] | **Heretic TE GGUF** | | ![Q4_K_M][badge-Q4_K_M] ![fp8][badge-fp8] ![bf16][badge-bf16] | 33.07 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF/resolve/main/qwen3vl_8b_heretic-Q4_K_M.gguf) |
| ![TE][ltype-te] | **Heretic TE DHQ GGUF** | | ![IQ4_XS][badge-IQ4_XS] ![Q4_K_M][badge-Q4_K_M] ![Q3_K_M][badge-Q3_K_M] | 4.56 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-DHQ-GGUF) |
| ![TE][ltype-te] | **Heretic TE NVFP4** | | ![nvfp4][badge-nvfp4] | 6.31 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-NVFP4/resolve/main/qwen3vl_8b_nvfp4_heretic.safetensors) |
| ![TE][ltype-te] | **Heretic TE int8 ConvRot** | | ![int8][badge-int8] | 9.35 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-int8-convrot/resolve/main/qwen3vl_8b_int8_convrot_heretic.safetensors) |
| ![TE][ltype-te] | **Heretic TE W4A8** | | ![w4a8][badge-w4a8] | 6.31 GB | [![][gh-Karsus1997]](https://huggingface.co/Karsus1997/Qwen-Image-2.1-Text-Encoder-Heretic-W4A8) |
| ![TE][ltype-te] | **Heretic TE BF16** | | ![bf16][badge-bf16] | 17.53 GB | [![][gh-kkxao]](https://huggingface.co/kkxao/Qwen-Image-2.1-Text-Encoder-Heretic) |
|  |  |  |  |  |  |
| ![PE][ltype-pe] | **PE-T2I Heretic GGUF** | ![text → image][task-t2i] | ![Q4_K_M][badge-Q4_K_M] | 5.89 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-T2I-Heretic-GGUF/resolve/main/pe_t2i_heretic-Q4_K_M.gguf) |
| ![PE][ltype-pe] | **PE-I2I Heretic GGUF** | ![image → image][task-i2i] | ![Q4_K_M][badge-Q4_K_M] | 6.81 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-PE-I2I-Heretic-GGUF/resolve/main/pe_i2i_heretic-Q4_K_M.gguf) |
| ![PE][ltype-pe] | **ComfyUI PE bundle** | ![both][task-both] | ![Q4_K_M][badge-Q4_K_M] | 18.07 GB | [![][gh-t8star]](https://huggingface.co/t8star/qwen-image-2.1-comfy) |
| ![PE][ltype-pe] | **PE-I2I Abliterated** | ![image → image][task-i2i] | ![bf16][badge-bf16] | 18.82 GB | [![][gh-base11231]](https://huggingface.co/base11231/Qwen-Image-2.1-PE-I2I-Abliterated) |
| ![PE][ltype-pe] | **PE-T2I Heretic** | ![text → image][task-t2i] | ![bf16][badge-bf16] | 18.82 GB | [![][gh-darrellbest]](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic) |
| ![PE][ltype-pe] | **PE-I2I Heretic** | ![image → image][task-i2i] | ![bf16][badge-bf16] | 18.82 GB | [![][gh-darrellbest]](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic) |
| ![PE][ltype-pe] | **PE-T2I Heretic NVFP4** | ![text → image][task-t2i] | ![nvfp4][badge-nvfp4] | 11.20 GB | [![][gh-darrellbest]](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-T2I-Heretic-NVFP4) |
| ![PE][ltype-pe] | **PE-I2I Heretic NVFP4** | ![image → image][task-i2i] | ![nvfp4][badge-nvfp4] | 11.20 GB | [![][gh-darrellbest]](https://huggingface.co/darrellbest/Qwen-Image-2.1-PE-I2I-Heretic-NVFP4) |

Start with the **TE GGUF** build if you want one download: Q4_K_M (5.03 GB), fp8 (9.34 GB), bf16 (17.53 GB), plus a 1.16 GB `mmproj` projector, all in one repo.

**DHQ** (Dual-Head Quant) is the same heretic weights re-quantized by what the **Qwen 2.1 DiT actually reads**, measured through the DiT's own text-input stage rather than by next-token loss. Every file is the same *size* as llama.cpp's build of that name. The ladder runs IQ2_S → IQ4_XS plus Q2_K / Q3_K_M / Q4_K_M, each with its imatrix recipe. Take DHQ below Q5; at Q5 and above the standard recipe already matches it. Tuned for this DiT specifically; other Qwen3-VL-8B consumers can load the files but were not tuned for.

<p id="pe-quant" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Quantized &amp; ported

| Name | Format | Precision | Size | Links |
| :--- | :---: | :---: | :---: | :---: |
| **PE ComfyUI pack** | BF16 + int8 ConvRot | ![bf16][badge-bf16] ![int8][badge-int8] | 64.68 GB | [![][gh-HarleyWang]](https://huggingface.co/HarleyWang/Qwen-Image-2.1-PE-ComfyUI) |
| **PE-T2I MLX** | MLX (4/8/16-bit) | ![int4][badge-int4] ![int8][badge-int8] ![bf16][badge-bf16] | 35.20 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-T2I-MLX) |
| **PE-I2I MLX** | MLX (4/8/16-bit) | ![int4][badge-int4] ![int8][badge-int8] ![bf16][badge-bf16] | 35.20 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-PE-I2I-MLX) |
| **Prompt Enhancement INT8** | int8 ConvRot | ![int8][badge-int8] | 24.69 GB | [![][gh-foofifoo]](https://huggingface.co/foofifoo/Qwen-Image-2.1-Prompt-Enhancement-INT8-Convrot) |
| **Heretic T2I int8 tensorwise** | int8 tensorwise ConvRot | ![int8][badge-int8] | 9.99 GB | [![][gh-diffnamehard]](https://huggingface.co/diffnamehard/Qwen-Image-2.1-PE-T2I-Heretic-int8-tensorwise-convrot) |
| **Heretic PE int8 ConvRot** | int8 ConvRot (T2I + I2I) | ![int8][badge-int8] | 19.91 GB | [![][gh-netrunner--exe]](https://huggingface.co/netrunner-exe/Qwen-Image-2.1-PE-Heretic) |

The INT8 ConvRot pack covers both PE-T2I and PE-I2I in one download, and so does the heretic int8 ConvRot pair, which unlike the tensorwise build keeps the MTP head (1,395 tensors against 829, one file per task at 9.96 GB).

<p id="quant" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◈ Quantizations

Community conversions of the base DiT. Sizes are total weight bytes per repo. Distilled turbo GGUFs live under [Turbo](#turbo) instead.

<p id="quant-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ GGUF

Transformer-only weights for llama.cpp, sorted from the highest quant down. **Q4_K_M** is the usual quality/size balance point. **[Unsloth](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** carries the widest ladder; where two repos offer a quant Unsloth does not, both are linked in the same cell.

| Quant | Size | Download |
| :---: | :---: | :---: |
| ![F16][badge-bf16] | 14.23 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-F16.gguf) |
| ![Q8_0][badge-q8] | 7.64 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q8_0.gguf) |
| ![Q6_K_XL][badge-q6k] | 6.72 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q6_K_XL.gguf) |
| ![Q6_K][badge-q6k] | 6.27 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q6_K.gguf) |
| ![Q5_K_M][badge-q5km] | 5.39 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q5_K_M.gguf) |
| ![Q5_K_S][badge-q5km] | 4.50 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q5_K_S.gguf) |
| ![Q4 · HVQ3 (sd.cpp)][badge-q4km] | 5.96 GB | [![][gh-zcf0508]](https://huggingface.co/zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed/resolve/main/Qwen-Image-2.1-Q4-sd.cpp.gguf) |
| ![NVFP4][badge-nvfp4] | 4.05 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/qwen-image-2.1-nvfp4.gguf) |
| ![Q4_K_M][badge-q4km] | 4.20 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q4_K_M.gguf) |
| ![Q4_0][badge-q4km] | 4.05 GB | [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized/resolve/main/qwen-image-2.1-turbo-Q4_0.gguf) ┊ [![][gh-ped4enko]](https://huggingface.co/ped4enko/Qwen-Image-2.1-Dessi/resolve/main/qwen-image-2.1-Q4_0.gguf) |
| ![Q4_K_S][badge-q4km] | 3.91 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q4_K_S.gguf) |
| ![Q3_K_XL][badge-q3km] | 3.61 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q3_K_XL.gguf) |
| ![Q3_K_M][badge-q3km] | 3.17 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q3_K_M.gguf) |
| ![Q3_K_S][badge-q3km] | 2.72 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q3_K_S.gguf) |
| ![Q2_K][badge-q2k] | 2.47 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q2_K.gguf) |
| Text encoder ![NVFP4][badge-nvfp4] | 6.30 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/qwen3vl-8b-nvfp4.gguf) |
| Text encoder ![Q4_K_M][badge-q4km] | 5.03 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/qwen3vl-8b-it-q4_k_m.gguf) |
| mmproj projector ![Q8_0][badge-q8] | 0.75 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/mmproj-qwen3vl-8b-it-q8_0.gguf) |
| VAE ![BF16][badge-bf16] | 0.68 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/pig_qwen_image_2.1_vae_bf16.gguf) |
| VAE ![F16][badge-f16] | 0.68 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/pig_qwen_image_2.1_vae_fp32-f16.gguf) |

<p id="quant-lowbit" align="center">· · · · · · · · · · · · · ·</p>

### ▣ 4-bit &amp; 8-bit

Every FP4 / NVFP4 / MXFP4 / INT8 / INT4 / W4A4 conversion in one place.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **INT4ConvRot-ComfyUI** | ![bf16][badge-bf16] ![int8][badge-int8] ![int4][badge-int4] ![w4a8][badge-w4a8] | 65.91 GB | [![][gh-chfm]](https://huggingface.co/chfm/Qwen-Image-2.1-INT4ConvRot-ComfyUI) | **Best all-in-one ComfyUI pack.** DiT and TE at three precisions plus VAE, in correct folder layout. |
| **Darkstar ModelOpt W4A4 NVFP4** | ![nvfp4][badge-nvfp4] | 23.66 GB | [![][gh-HangGlidersRule]](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-W4A4-NVFP4) | NVIDIA ModelOpt-derived, full repo. |
| **DiT NVFP4 ComfyUI** | ![nvfp4][badge-nvfp4] | 13.82 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI/resolve/main/qwen_image_2.1_nvfp4.safetensors) | Three ComfyUI cuts: `nvfp4`, `nvfp4_T2`, `nvfp4_T3`. |
| **bnb 4bit** | ![int4][badge-int4] | 11.41 GB | [![][gh-circulus]](https://huggingface.co/circulus/Qwen-Image-2.1-bnb-4bit) | Standard bitsandbytes NF4. |
| **MXFP4 (Paiton)** | ![mxfp4][badge-mxfp4] | 9.31 GB | [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4) | Paiton backend, 57 shards. |
| **MXFP4 Paiton RDNA4** | ![mxfp4][badge-mxfp4] | 9.31 GB | [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4) | RDNA4-specific kernel variant, identical layout. |
| **Uncensored MXFP4 Paiton** | ![mxfp4][badge-mxfp4] | 9.31 GB | ⚠️ [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton) | Uncensored, derived from the abenzerps GGUF. |
| **Pipi** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-pipi-qwen-image-2.1-int8-01-unet) | Merged photorealistic DiT in int8 ConvRot, single file for `models/diffusion_models/`, no LoRA needed. Trigger `photorealistic`, Euler, 40 steps, CFG 1.0, and no LoRA stacked on it. Uncensored. |
| **Noct Q** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-Noctaluna]](https://huggingface.co/Noctaluna/Noct-Q-Uncensored-Qwen-Image-2.1) | The photorealistic line, parent of the Anime row below. Merged int8 ConvRot DiT, single file for `models/diffusion_models/`. **Take V4**: the card measures it rendering explicit scenes as asked three times as often as V3. 25 steps, `euler`/`simple`, CFG 3 with a negative prompt. The wider int4/fp8/bf16 ladder and prompt gallery live on Civitai. |
| **Noct Q Anime** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-Noctaluna]](https://huggingface.co/Noctaluna/Noct-Q-Anime-Uncensored-Qwen-Image-2.1) | Merged anime fine-tune of the DiT, single file, int8 ConvRot, fits 8–12 GB cards. Without the word "anime" in the prompt you get a photo. 25 steps, `euler`/`simple`, CFG 3. Uncensored. |
| **Fwaun Tune** | ![int8][badge-int8] ![bf16][badge-bf16] | 6.76 / 13.25 GB | [![][cv-fwaunstp733]](https://civitai.com/models/2846802/fwaun-tune?modelVersionId=3373014) | Merged fine-tune for flat colour, no lineart, deformed character art. |
| **Ray Qwen2.1 edit-enhanced** | ![int8][badge-int8] | 6.76 GB | ⚠️ [![][cv-Ray3780]](https://civitai.com/models/2972899/ray-qwen21?modelVersionId=3369126) | Custom editing merged into the base DiT; keeps base step counts. |
| **Ray Qwen2.1 turbo v1.3** | ![int8][badge-int8] | 6.70 GB | ⚠️ [![][cv-Ray3780]](https://civitai.com/models/2972899/ray-qwen21?modelVersionId=3395380) | The same editing work folded into the official Turbo checkpoint. |
| **RedCraft 3.0** | ![int4][badge-int4] ![int8][badge-int8] ![fp8][badge-fp8] | 5.34 / 6.76 / 6.63 GB | ⚠️ [![][cv-AiMetatron]](https://civitai.com/models/958009/redcraft-or?modelVersionId=3407259) | Merged DiT fine-tune in three quant levels; the version also bundles the Texture-Fix VAE. |
| **W4A4 NVFP4** | ![nvfp4][badge-nvfp4] | 4.88 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4) | DiT only; near-identical to the INT4 build. |
| **W4A4 INT4** | ![int4][badge-int4] | 4.66 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4) | DiT only. |
| **Uncensored NVFP4** | ![nvfp4][badge-nvfp4] | 4.20 GB | ⚠️ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized/resolve/main/qwen-image-2.1-turbo-NVFP4.safetensors) | DiT only, native NVFP4 rather than a GGUF conversion. |

<p id="quant-fp8" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; bf16

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Darkstar ModelOpt FP8** | ![fp8][badge-fp8] | 26.33 GB | [![][gh-HangGlidersRule]](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8) | NVIDIA ModelOpt-derived, full repo. |
| **Uncensored BF16 SafeTensor** | ![bf16][badge-bf16] | 14.23 GB | ⚠️ [![][gh-dh123456789123]](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor/resolve/main/qwen-image-2.1-UC-BF16_bf16.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet/resolve/main/Qwen-Image-2.1-_bf16%E6%97%A0%E5%AE%A1%E6%9F%A5.safetensors) | Single 14.23 GB file, the full DiT in one piece, mirrored by both repos. |
| **Uncensored UC FP8 / Q4_K_M** | ![fp8][badge-fp8] ![Q4_K_M][badge-Q4_K_M] | 7.12 / 4.60 GB | ⚠️ [![][gh-og1936]](https://huggingface.co/og1936/qwen-image-2.1-uncensored-weights/resolve/main/qwen-image-2.1-UC-fp8.safetensors) ┊ [![][gh-og1936]](https://huggingface.co/og1936/qwen-image-2.1-uncensored-weights/resolve/main/qwen-image-2.1-UC-Q4_K_M.gguf) | Mirror of the abenzerps uncensored DiT removed on 2026-11-03. FP8 e4m3 DiT plus a Q4_K_M GGUF of the same weights; bring your own text encoder and VAE. |
| **Uncensored Genesis BF16** | ![bf16][badge-bf16] | 32.46 GB | ⚠️ [![][gh-LuffyTheFox]](https://huggingface.co/LuffyTheFox/Qwen-Image-2.1-Uncensored-Genesis-BF16-GGUF) | The abenzerps uncensored DiT with the author's "Genesis" denoising pass applied, a post-training SVD repair rather than a fine-tune, so a distinct weight set rather than a re-upload. Self-contained: DiT + Genesis text encoder + `mmproj` + VAE. |
| **DF11 ComfyUI** | ![bf16][badge-bf16] | 9.72 GB | [![][gh-mingyi456]](https://huggingface.co/mingyi456/Qwen-Image-2.1-DF11-ComfyUI/resolve/main/qwen_image_2.1_bf16-DF11.safetensors) | `qwen_image_2.1_bf16-DF11.safetensors`. |

<p id="quant-nunchaku" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Nunchaku (SVDQ)

SVDQ-based 4-bit for Nunchaku, which targets low-VRAM systems and 4090-class cards.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **nunchaku** | ![fp4][badge-fp4] | 8.75 GB | [![][gh-catplusplus]](https://huggingface.co/catplusplus/nunchaku-qwen-image-2.1/resolve/main/best_quality_fp4.safetensors) | Two builds: `best_quality_fp4` and `svdq-fp4_r32`. |
| **nunchaku lite int4** | ![int4][badge-int4] | 4.15 GB | [![][gh-BlazeMCworld]](https://huggingface.co/BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4) | Half the size of the FP4 build. |


<p id="quant-exl3" align="center">· · · · · · · · · · · · · ·</p>

### ▣ EXL3

EXL3 DiT quantizations for AMD ROCm. The text encoder and VAE stay BF16 and the weights need the [rocm_exl3_forme](https://github.com/jyohukuchan/rocm_exl3_forme) engine: these are custom EXL3 diffusion checkpoints, not a diffusers folder, GGUF, NF4 or Nunchaku checkpoint.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **EXL3** | ![EXL3 3bit][badge-exl3-3bit] ![EXL3 4bit][badge-exl3-4bit] ![EXL3 5bit][badge-exl3-5bit] | 2.89 / 3.77 / 4.64 GB | [![][gh-jyohukuchan]](https://huggingface.co/jyohukuchan/Qwen-Image-2.1-EXL3) | Base DiT in 3, 4 and 5 bit; validated on Radeon AI PRO R9700 and Radeon PRO V620. |
<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▸ Turbo &amp; step distillation

Few-step students of the base model, plus plain adapters that only change the schedule. Qwen's own 8-step checkpoint is the official reference in the [table above](#official-turbo); everything here is the community response, trading fidelity for speed.

**Current Viggle release: `v0.3` (2026-09-29), 6 steps, CFG 1.0, empty negative prompt.** It supersedes `v0.2.1` (2026-09-24), `v0.2` (5-step name, sampled at 6) and `v0.1` (4 steps). v0.3 trades some grain for cleaner surfaces with slightly softer fine texture, so v0.2.1 is still in the repository if you prefer the crisper look. A **9-step mode** buys small-text accuracy at about 1.4–1.5× the 6-step cost, diffusers- and Space-only. Official source: **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)**.

Three distillation lines are listed below and their checkpoints are **not interchangeable**, so pick one family: **Viggle** (the reference DMD line), **Alibaba PAI** (official 4-step, Parallel Decoding Distillation) and **Pruna** (third-party DMD, 8-step and 5-step). Beyond the three, **[congruency](https://huggingface.co/congruency/Qwen-Image-2.1-turbo-mfd)** trains against Mean Flow Distillation and **[lightx2v](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning)** ships a 4-step Lightning adapter; both are ordinary LoRAs on the base model, so they compose with whatever else you run.

Each Pruna adapter has its own sigma schedule and they are **not** interchangeable, so load exactly one; the grids are in the repo. Both want `shift=1.0` with dynamic shifting off, no CFG, no negative prompt, LoRA strength 1.0, and a pinned diffusers commit. Trained at 1K with up to 3 reference images; 2K runs but sits outside training coverage.

<p id="turbo-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Turbo GGUF

DiT-only quants for ComfyUI-GGUF, from the **v0.1 full fine-tune** (4-step). Both repos keep FP32 attention norms and BF16 patch embeddings, so do **not** stack a Viggle LoRA on top. **[realrebelai](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs)** has the wider ladder, **[Abiray](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF)** adds `Q4_K_S`; where both ship a quant, both are linked and sizes differ because the builds differ.

| Quant | Size | Download |
| :---: | :---: | :---: |
| ![Q8_0][badge-Q8_0] | 7.69 / 7.59 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q8_0.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q8_0.gguf) |
| ![Q6_K][badge-Q6_K] | 6.91 / 5.88 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q6_K.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q6_K.gguf) |
| ![Q5_K_M][badge-Q5_K_M] | 5.96 / 5.01 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q5_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q5_K_M.gguf) |
| ![Q4_K_M][badge-Q4_K_M] | 5.56 / 4.19 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q4_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_M.gguf) |
| ![Q4_K_S][badge-Q4_K_S] | 4.06 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_S.gguf) |
| ![Q3_K_M][badge-Q3_K_M] | 4.19 / 3.19 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q3_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q3_K_M.gguf) |
| ![Q2_K][badge-Q2_K] | 3.77 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q2_K.gguf) |

With the official Turbo out, **[Abiray](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF)** runs the full ladder off *it* instead, `Q2_K` through `Q8_0`. Sampling matches the official checkpoint: `euler` with `simple` or `normal`, 4–8 steps (6 is the card's pick), CFG 1.0. DiT only; text encoder and VAE come from the Comfy-Org pack.

| Quant | Size | Download |
| :---: | :---: | :---: |
| ![Q8_0][badge-Q8_0] | 7.59 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q8_0.gguf) |
| ![Q6_K][badge-Q6_K] | 5.88 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q6_K.gguf) |
| ![Q5_K_M][badge-Q5_K_M] | 5.01 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q5_K_M.gguf) |
| ![Q5_K_S][badge-q5km] | 4.94 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q5_K_S.gguf) |
| ![Q5_0][badge-q5km] | 4.94 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q5_0.gguf) |
| ![Q4_K_M][badge-Q4_K_M] | 4.19 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_M.gguf) |
| ![Q4_K_S][badge-Q4_K_S] | 4.06 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_S.gguf) |
| ![Q4_0][badge-q4km] | 4.05 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_0.gguf) |
| ![Q3_K_M][badge-Q3_K_M] | 3.19 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q3_K_M.gguf) |
| ![Q3_K_S][badge-q3km] | 3.11 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q3_K_S.gguf) |
| ![Q2_K][badge-Q2_K] | 2.45 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q2_K.gguf) |

<p id="turbo-models" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Official &amp; converted

Repo links: **[Viggle](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** · **[alibaba-pai](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Acc-LoRAs)** · **[Pruna](https://huggingface.co/PrunaAI/P-Q-Image-2.1)** · **[chfm](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo)** (v0.2 snapshot) · **[t8star](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy)** · **[xingewh](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy)** · **[RunningHubAI](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora)** · **[addlabsviral](https://huggingface.co/addlabsviral/Qwen-Image-2.1-Turbo-4bit)** · **[cgb](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX)** · **[Zabin](https://huggingface.co/Zabin/Qwen_Image_2.1_Turbo_8Steps)** · **[t8star HiFi](https://huggingface.co/t8star/Qwen-Image-2.1-Turbo-Hifi-Comfy)** · **[congruency](https://huggingface.co/congruency/Qwen-Image-2.1-turbo-mfd)** · **[lightx2v](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning)** · **[lightx2v v2 RL](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning-v2-RL)** · **[Abiray official-turbo GGUF](https://huggingface.co/Abiray/Qwen-Image-2.1-Turbo-GGUF)** · **[unsloth](https://huggingface.co/unsloth/Qwen-Image-2.1-Turbo-FP8)** · **[myllmbox](https://huggingface.co/myllmbox/Qwen-Image-2.1-Turbo-hibrid)** · **[lenML](https://civitai.com/models/2997646)**

Two ComfyUI conversions of the v0.2.1 adapters exist and are not interchangeable. **[xingewh](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy)** re-keys the weights and loads with the stock `Load LoRA` node. **[t8star](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy)** fuses gate/up for ComfyUI's merged MLP, which is why its files are larger; load those with `LoraLoaderBypassModelOnly` at strength 1.0 on a **BF16** base, because ordinary merging loaders lose adapter updates to rounding.

Only **v0.2.1** has third-party conversions. For v0.3, ComfyUI support means copying `viggle_turbo.py` into `custom_nodes/`; it ships an **unmerged** LoRA node and a **Viggle Turbo Sigmas** node, and the stock loaders drop about 30% of the adapter's update on bf16. The merged single-file transformers below skip the LoRA and need only the sigmas node, but they are *close to, not the same as*, the LoRA path, so use the LoRA if outputs must match diffusers.

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Sarvam Tez** | 2 | ![bf16][badge-bf16] | 1.34 GB | [![][gh-sarvamaigc]](https://huggingface.co/sarvamaigc/Qwen-Image-2.1-Sarvam-Tez/resolve/main/Qwen-Image-2.1-sarvam-tez-2step-lora-r256.safetensors) | **The shortest schedule on this page**, from Sarvam AI: 2 steps, no CFG, a plain LoRA on the stock pipeline. v1.1 retrain: DPG-Bench 86.79 against the base model's 84.30, both without CFG. Rank 256, the widest adapter here. Needs the repo's `sarvam_tez.py` helper, text-to-image only, and seed variety is about half the base model's. Research licence, non-commercial. |
| **v0.3 LoRA r256** | 6 | ![bf16][badge-bf16] | 1.36 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.3-6step-lora-r256.safetensors) | **Start here.** The current official release. Load on the base transformer at runtime, do not merge, keep strength 1.0. The r128 cut is an exact per-layer SVD of this update. |
| **v0.3 LoRA r128** | 6 | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.3-6step-lora-r128.safetensors) | What the shipped ComfyUI workflows use. |
| **v0.3 adapter (peft)** | 6 | ![fp32][badge-fp32] | 2.72 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/peft_v0.3) | The v0.3 LoRA in PEFT key format, F32 as trained. Same weights as the r256 file under different key names; load one or the other, never both. |
| **v0.2.1 LoRA r256** | 6 | ![bf16][badge-bf16] | 1.36 / 1.36 / 1.76 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r256.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r256_comfy.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/qwen_image_2.1_viggle_turbo_v0.2.1_r256_comfy.safetensors) | The crisper of the two 6-step releases, and the last one with third-party ComfyUI conversions. Runs the demo Space. |
| **v0.2.1 LoRA r128** | 6 | ![bf16][badge-bf16] | 0.68 / 0.68 / 0.88 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128_comfy.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/qwen_image_2.1_viggle_turbo_v0.2.1_r128_comfy.safetensors) | Same adapter cut to rank 128; what the shipped ComfyUI workflows use. |
| **v0.2.1 adapter (peft)** | 6 | ![fp32][badge-fp32] | 2.72 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/peft_v0.2.1) | The v0.2.1 LoRA in PEFT key format, F32 as trained. |
| **v0.2 LoRA r256** | 5 / 6 | ![bf16][badge-bf16] | 1.36 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) ┊ [![][gh-chfm]](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy.safetensors) | Step 600 of the same run. The `5step` in the name is the launch schedule; sample at 6 like v0.2.1. |
| **v0.2 LoRA r128** | 5 / 6 | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r128.safetensors) | Rank-128 cut of v0.2. |
| **v0.1 full fine-tune** | 4 | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/transformer) | Merged transformer, no LoRA needed. This is what the GGUF quants above were built from. |
| **v0.1 LoRA r64** | 4 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-lora-r64.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) | Superseded (diversity collapsed to 0.75× base, output visibly softer); kept for reproducibility. |
| **Fun-Acc 4Step (PDD)** | 4 | ![bf16][badge-bf16] | 0.35 GB | [![][gh-alibaba--pai]](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Acc-LoRAs/resolve/main/models/Qwen-Image-2.1-Fun-Acc-4Step.safetensors) | **Official Alibaba PAI line**, rank 64, via Parallel Decoding Distillation. Independent of Viggle, with 4 NFE for both T2I and instruction editing. | 
| **Pruna 8Step** | 8 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-PrunaAI]](https://huggingface.co/PrunaAI/P-Q-Image-2.1/resolve/main/p_q_image_2.1_8_steps_v0.1.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Pruna-Qwen-Image-2.1-Comfy/resolve/main/p_qwen_image_2.1_8step_v0.1-comfyui-T8.safetensors) | Third-party DMD line, rank 64 / alpha 128. Better of the two and the card's default, but v0.1 is marked work in progress. |
| **Pruna 5Step** | 5 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-PrunaAI]](https://huggingface.co/PrunaAI/P-Q-Image-2.1/resolve/main/p_q_image_2.1_5_steps_v0.1.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Pruna-Qwen-Image-2.1-Comfy/resolve/main/p_qwen_image_2.1_5step_v0.1-comfyui-T8.safetensors) | Same line, faster and visibly weaker. One adapter at a time; see the schedule note above. |
| **UltraFast 8Step** | 8 | ![bf16][badge-bf16] | 0.17 GB | [![][gh-Haverbex]](https://huggingface.co/Haverbex/Qwen-Image-2.1-UltraFast/tree/main/adapter) | Editing-focused rather than T2I, trained on MagicBrush. Rank 32, but loaded by the repo's own `inject_dit_lora` script, **not** a stock PEFT or ComfyUI loader. |
| **Turbo 4bit diffusers** | 8 | ![int4][badge-int4] | 11.41 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/Qwen-Image-2.1-Turbo-4bit) | The official Turbo pipeline at bnb **NF4**: DiT 4.00 + TE 6.73 + VAE 0.68, same saved 8-step sigmas and CFG 1. Needs `accelerate` + `bitsandbytes` and diffusers from git. Replaces the author's earlier BF16/FP4 repos, no longer public. |
| **Turbo 8bit diffusers** | 8 | ![int8][badge-int8] | 17.82 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/Qwen-Image-2.1-Turbo-diffuser-8bit) | torchao int8 weight-only over the same official Turbo: DiT 7.12 + TE 10.03 + VAE 0.68. Same 8 steps and CFG 1; needs `torchao` and a diffusers build with pipeline-configured sigmas. |
| **Turbo INT8-ConvRot (unsloth)** | 8 | ![int8][badge-int8] | 7.26 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-Turbo-FP8/resolve/main/Qwen-Image-2.1-Turbo-INT8-ConvRot.safetensors) | The official Turbo's transformer in INT8 with a group-256 Hadamard rotation. Same-seed LPIPS **0.037** against bf16, the best of the three Unsloth files and the card's pick. Swaps in place, so the repo's BF16 text encoder and VAE stay usable; needs a diffusers build with pipeline-configured sigmas. |
| **Turbo INT8 (unsloth)** | 8 | ![int8][badge-int8] | 7.26 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-Turbo-FP8/resolve/main/Qwen-Image-2.1-Turbo-INT8.safetensors) | The same recipe without the rotation, in the same VRAM class. LPIPS 0.089; take the ConvRot file above unless the rotation pass costs you time. |
| **Turbo FP8 (unsloth)** | 8 | ![fp8][badge-fp8] | 7.12 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-Turbo-FP8/resolve/main/Qwen-Image-2.1-Turbo-FP8.safetensors) | Weight-only FP8 e4m3 with per-row scales. Smallest of the three, LPIPS 0.125. Same loader conditions as the INT8 rows. |
| **Turbo NVFP4/BF16 hybrid** | 8 | ![nvfp4][badge-nvfp4] | 14.46 GB | [![][gh-myllmbox]](https://huggingface.co/myllmbox/Qwen-Image-2.1-Turbo-hibrid) | The whole Turbo pipeline quantized to fit one **16 GB Blackwell** card: DiT 5.69 + text encoder 8.09 + VAE 0.68 GB, torchao NVFP4 tensors in safetensors. Measured **8.0 s** at 1024². Needs Blackwell FP4 tensor cores, `mslk` 1.3.0 and pinned torch / diffusers builds; non-commercial research license. |
| **Turbo ONNX (browser)** | 4 | ![int4][badge-int4] | ~17.2 GB | [![][gh-cgb]](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX) | r64 LoRA merged into the denoiser, then Q4 MatMulNBits. WebGPU in-browser; needs the FreeGen pipeline and a desktop adapter. Experimental. |
| **Turbo EXL3** | 8 | ![EXL3 3bit][badge-exl3-3bit] ![EXL3 4bit][badge-exl3-4bit] ![EXL3 5bit][badge-exl3-5bit] | 2.89 / 3.77 / 4.64 GB | [![][gh-jyohukuchan]](https://huggingface.co/jyohukuchan/Qwen-Image-2.1-Turbo-EXL3) | The official Turbo in EXL3 for the same AMD ROCm engine; the text encoder and VAE stay BF16. |
| **v0.3 merged int8** | 6 | ![int8][badge-int8] | 7.26 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.3-6step-int8_convrot.safetensors) | Base transformer with the r256 LoRA merged in FP32, then quantized once. Goes in `models/diffusion_models/` with the stock loader; still needs the **Viggle Turbo Sigmas** node and the Comfy-Org text encoder and VAE. |
| **v0.3 merged fp8** | 6 | ![fp8][badge-fp8] | 7.25 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.3-6step-fp8_e4m3fn.safetensors) | Same, weight-only fp8 e4m3fn. Picks up the workflow's fp8 branch; the int8 file is the default. |
| **v0.3 merged Q8_0** | 6 | ![Q8_0][badge-Q8_0] | 7.69 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.3-6step-Q8_0.gguf) | Closest of the merged files to the LoRA path, and needs ComfyUI-GGUF. Q6_K, Q5_K_M and Q4_K_M sit in the same directory under the same naming. |
| **6NFE Distill** | 6 | ![fp32][badge-fp32] | 28.46 GB | [![][gh-Sutoonq]](https://huggingface.co/Sutoonq/Qwen-Image-2.1-6NFE-Distill/tree/main/transformer) | Full transformer distill in 8 shards, FP32, no LoRA to load. Handles T2I and editing. **Needs `shift_terminal=0.4`** and conditional-only sampling (`true_cfg_scale=1.0`, no negative prompt) while keeping the base's dynamic shift. Ships its own `inference.py`. Third-party, same research license. |
| **HDR full fine-tune** | 4 | ![bf16][badge-bf16] | 14.23 GB | [![][gh-yhzr2222]](https://huggingface.co/yhzr2222/qwen_image_2.1_bf16_hdr_ViggleHDR_noised) | Full-parameter BF16 fine-tune (single file) through a 3-stage curriculum: artistic style, Chinese mythology/guofeng, then 4-step distillation. **The one row here that wants CFG above 1**; the card tests 2.5. 4 steps, or 20–30 for extra detail, `euler`/`simple`. Goes in `models/unet/`. Marked experimental. |
| **Aisha NSFW v4** | 8 | ![bf16][badge-bf16] | 13.25 GB | ⚠️ [![][cv-AishaAI]](https://civitai.com/models/3009275/aisha-nsfw-qwen-21-turbo?modelVersionId=3414853) | Merged Turbo fine-tune for adult content, still a preview build. |
| **UltraFast Q4_K** | 8 | ![Q4_K][badge-Q4_K] | 4.05 GB | [![][gh-Haverbex]](https://huggingface.co/Haverbex/Qwen-Image-2.1-UltraFast-GGUF/resolve/main/Qwen-Image-2.1-UltraFast-Q4_K.gguf) | DiT-only Q4_K with the UltraFast adapter **already merged**, so do not stack the adapter on top. Still needs the text/vision encoders and the matching VAE. Quality and speed unbenchmarked. |
| **Pruna 8Step SDNQ** | 8 | ![int4][badge-int4] | 11.51 GB | [![][gh-SamuelTallet]](https://huggingface.co/SamuelTallet/Pruna-Qwen-Image-2.1-8steps-SDNQ-4bit-dynamic-hadamard256) | The Pruna 8-step adapter **merged** into the base and re-quantized with [SDNQ](https://github.com/Disty0/sdnq) (UINT4 dynamic, Hadamard rotation at group size 256). Full diffusers pipeline, so the text encoder is quantized too. Needs SDNQ 0.2.0+, Triton and diffusers from git. Follows the Pruna sigma schedule above. |
| **Official Turbo diffusers** | 8 | ![bf16][badge-bf16] | 32.44 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-Turbo) | The reference 8-step checkpoint itself, listed in full under [official checkpoints](#official-turbo): CFG 1.0, schedule carried in the weights, prefix KV cache on. The point of comparison for every row here. |
| **Merged 8Step (BF16)** | 8 | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Zabin]](https://huggingface.co/Zabin/Qwen_Image_2.1_Turbo_8Steps/resolve/main/QwenImage_2.1_bf16_turbo_8Steps_v1.safetensors) | A home-made merge, not a distillation: base BF16 with three adapters baked in (Viggle v0.2.1 r128 at 0.75, Viggle v0.1 r64 at 0.15, Turbo8 LoRA at 0.2). Single file for `models/diffusion_models/`, no LoRA to load. The card warns of anatomical errors and grid noise, and the recipe inherits the v0.1 branch's softer look. The repo keeps the rank-128 adapter SVD-extracted back out of this merge (0.64 GB). |
| **Merged 8Step (int8)** | 8 | ![int8][badge-int8] | 7.16 GB | [![][gh-Zabin]](https://huggingface.co/Zabin/Qwen_Image_2.1_Turbo_8Steps/resolve/main/QwenImage_2.1_turbo_8Step_Int8_Convrot_v1.safetensors) | The same merge in int8 ConvRot, half the VRAM of the BF16 file above. Identical recipe, so the same caveats. |
| **Merged 5Step (int8)** | 5 | ![int8][badge-int8] | 7.16 GB | [![][gh-Zabin]](https://huggingface.co/Zabin/Qwen_Image_2.1_Turbo_8Steps/resolve/main/qwen_image2.1_turbo_5Steps_int8_convrot_v3.safetensors) | The author's 5-step take, v3 of the same repo. Generation and editing from one file; the card does not separate its quality from the 8-step builds, so treat the step count as unvalidated. |
| **MFD research LoRA** | 2–12 | ![bf16][badge-bf16] | 0.07 GB | [![][gh-congruency]](https://huggingface.co/congruency/Qwen-Image-2.1-turbo-mfd) | Snapshot ladder from one distillation run, 2 to 12 steps. |
| **T8 HiFi v001** | 8 | ![f16][badge-f16] | 19.56 GB | [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-Turbo-Hifi-Comfy/resolve/main/qwen_image_2.1_turbo_lora_hifi_T8_v001.safetensors) | The Turbo delta itself, saved as an adapter: the official Turbo-minus-base weight difference, lifted out by adaptive-rank SVD rather than trained. It costs 19.56 GB because it preserves ~99.9% of the update energy, bigger than the DiT it patches. Load on the **base** BF16 model with `LoraLoaderModelOnly` at strength 1.0, never on a Turbo model or beside another Turbo LoRA. The sigma schedule is mandatory, see the card; Euler, CFG 1. |
| **T8 HiFi compact (r512 / r768 / r896)** | 8 | ![f16][badge-f16] | 2.61 / 3.91 / 4.56 GB | [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-Turbo-Hifi-Comfy) | The same extraction with the rank capped at 512 / 768 / 896, a quarter of v001's size. **Lossy**: 53–64% of the delta energy survives against v001's 99.9%. Same loader, same mandatory sigma grid. Pick by budget. |
| **Turbo LoRA r128 (lenML)** | 8 | ![bf16][badge-bf16] | 0.63 GB | [![][cv-lenML]](https://civitai.com/models/2997646/qwen-image-21-turbo-lora?modelVersionId=3400002) | The official Turbo delta as a rank-128 LoRA. |
| **LightX2V Lightning** | 4 | ![fp32][badge-fp32] | 0.07 GB | [![][gh-lightx2v]](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning/resolve/main/pytorch_lora_weights.safetensors) | A 4-step DMD adapter from the LightX2V project, trained on 16 Ascend 910B NPUs, covering generation and editing. Wants a fixed sigma grid (see card), CFG 1.0, prefix KV cache off. Diffusers-only; the card shows `load_lora_weights` and no ComfyUI path. F32-stored factors, and the repo declares no license. |
| **LightX2V Lightning v2 RL** | 4 | ![fp32][badge-fp32] | 28.46 / 0.69 GB | [![][gh-lightx2v]](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning-v2-RL/resolve/main/diffusion_pytorch_model.safetensors) ┊ [![][gh-lightx2v]](https://huggingface.co/lightx2v/Qwen-Image-2.1-Lightning-v2-RL/resolve/main/qwen_image_21_lightning_v2_rl_4steps_lora_rank64.safetensors) | RL-refined 4-step Lightning v2: a full FP32 transformer (28.46 GB) and a rank-64 F32 LoRA (0.69 GB). Not a LoRA adapter or standalone pipeline; swap the base DiT in with `QwenImage21Transformer2DModel.from_pretrained`, run at BF16 with the exact sigmas from `inference_config.json` and CFG 1.0. The card documents only the full-transformer path. Qwen Research License, non-commercial. |
| **Turbo Distill 8-Step** | 8 | ![bf16][badge-bf16] | 0.39 / 0.55 / 0.79 / 1.13 / 1.61 / 2.34 GB | [![][cv-thedivergentai]](https://civitai.com/models/3011544/qwen-image-21-turbo-distill-8-step-lora-svd-extract?modelVersionId=3417921) | The Turbo delta lifted out by SVD, not trained. r64 / r128 / r256 in a diffusers layout (0.39 / 0.79 / 1.61 GB) and a ComfyUI layout (0.55 / 1.13 / 2.34 GB); take the ComfyUI one. The r256 cut keeps 46.4% of the delta energy. 8 steps, euler/simple, CFG 1. |
| **DMD2 2-step** | 2 | ![bf16][badge-bf16] | 0.07 GB | [![][gh-linoyts]](https://huggingface.co/linoyts/Qwen-Image-2.1-DMD2-2steps/resolve/main/pytorch_lora_weights.safetensors) ┊ [![][gh-linoyts]](https://huggingface.co/linoyts/Qwen-Image-2.1-DMD2-2steps/resolve/main/pytorch_lora_weights_v1_5.safetensors) | A 2-step DMD2 distillation, attention-only at rank 32. Fixed sigmas `[1.0, 0.5]` and `true_cfg_scale=1.0`. V1.5 (2026-10-30) improves prompt adherence and edit fidelity; pass `weight_name="pytorch_lora_weights_v1_5.safetensors"` to `load_lora_weights`. |

<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, control, and fix adapters. All target the base DiT unless noted. Grouped by type, then by size.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **ControlNet-Union** | ![Control][ltype-control] | ![bf16][badge-bf16] | 7.55 GB | [![][gh-alibaba--pai]](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Controlnet-Union/resolve/main/Qwen-Image-2.1-Fun-Controlnet-Union.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-Fun-Controlnet-Union-Comfy/resolve/main/Qwen-Image-2.1-Fun-Controlnet-Union-ComfyUI.safetensors) | One ControlNet checkpoint covering 8 conditions plus inpainting. |
| **Object Mover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview) | Moves a masked object to another spot. |
| **Object Remover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview) | Removes a masked object at full quality. |
| **Object Remover Bbox turbo** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo) | The same removal at 4-step speed. |
| **BFS Head Swap v1** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.32 GB | [![][gh-Alissonerdx]](https://huggingface.co/Alissonerdx/BFS-Best-Face-Swap/resolve/main/bfs_head_v1_qwen_2.1.safetensors) | Takes a head from image 2 onto the person in image 1, keeping gaze and expression. |
| **Multiple Angles** | ![Control][ltype-control] | ![f16][badge-f16] | 0.32 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/Qwen-Image-2.1-Multiple-Angles-LoRA/resolve/main/checkpoints_v2%2Fangles_v2_full_qwen21_multiple_angles_v2_qwen21_multiple_angles_v2_000001500.safetensors) ┊ [![][gh-akhaliq]](https://huggingface.co/akhaliq/Qwen-Image-2.1-Multiple-Angles-LoRA/resolve/main/multiple_angles_step1000.safetensors) | Re-shoots a subject from a named camera position. |
| **Next Scene** | ![Control][ltype-control] | ![f16][badge-f16] | 0.32 GB | [![][gh-akhaliq]](https://huggingface.co/akhaliq/Qwen-Image-2.1-Next-Scene-LoRA/resolve/main/next_scene_step2500.safetensors) | Directs the next camera move so outputs chain into a storyboard. |
| **QMA camera angles** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.32 GB | [![][gh-gribbliest]](https://huggingface.co/gribbliest/qwen-image-2.1-qma-camera-angles/resolve/main/qwen-image-2.1-qma-camera-angles.safetensors) | Orbits the camera around a subject while the subject stays put. |
| **Orbit Alpha** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-viewpoint-orbit-LoRA/resolve/main/checkpoints/steps2000res768/orbit_alpha_lora_gate_up_split.safetensors) | Renders an RGBA-cutout object from a new viewpoint. |
| **FG extract v4** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-trmz]](https://huggingface.co/trmz/qwen-image-2.1-fg-extract-lora/resolve/main/qwen-image-2.1-fg-extract-v4.safetensors) | Cuts a masked element out as an RGBA cutout at its original size and place. |
| **Doodle-in** | ![Control][ltype-control] | ![f16][badge-f16] | 0.17 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-doodle-in-LoRA/resolve/main/doodle_in_lora_qwen21_gate_up_split.safetensors) | Turns a magenta scribble into the object you name. |
| **Outpaint v2** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Outpaint-LoRA/resolve/main/qwen-image-2.1-outpaint-v2.safetensors) | Extends the picture in any direction, filling the padding you added. |
| **Outpaint v1** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Outpaint-LoRA/resolve/main/qwen-image-2.1-outpaint.safetensors) | The earlier run, better on very large extensions. |
| **Pano 360** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-Gogodr]](https://huggingface.co/Gogodr/qwen-image-2.1-edit-pano360-lora/resolve/main/pano360_qwen21_edit_v1.safetensors) | Builds a 2:1 equirectangular 360 panorama from 1-3 photos of one spot. |
| **Grey-margin outpaint** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora-2111645768636518402/resolve/main/Qwen2.1_%E6%89%A9%E5%9B%BE_GH_3750.safetensors) | Seamlessly extends the picture past its edge, filling the grey mask you leave. Trained at 1536; the prompt is optional. |
| **VNCCS PoseStudio** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.15 GB | [![][gh-MIUProject]](https://huggingface.co/MIUProject/VNCCS_PoseStudio_QI2.1) ┊ [![][cv-AHEKOT]](https://civitai.com/models/2957080/vnccs-posestudio-qwen-image-21?modelVersionId=3403775) | Pose-reference LoRA for the VNCCS ComfyUI node. |
| **AnyAngle** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.12 GB | [![][gh-lilylilith]](https://huggingface.co/lilylilith/QI_2.1_AnyAngle/resolve/main/QI2.1_AnyAngle.safetensors) | Changes the camera angle using a coarse render of the angle you want. |
|  |  |  |  |  |  |
| **Fix Opinionated** | ![Fix][ltype-fix] | ![fp32][badge-fp32] | 0.50 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix-Opinionated-v1.0/resolve/main/qwen2.1-detail-fix-opinionated-v1.0.safetensors) | The stronger Fix variant, which also shifts composition. |
| **Anime consistency** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-loras-lora/resolve/main/Qwen2.1_Anime_consistency.safetensors) | Keeps a character consistent while restyling anime images. |
| **Outfit Swap Consistency** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Outfit-Swap-Consistency-LoRA/resolve/main/qwen-image-2.1-outfit-swap.safetensors) | Virtual try-on that keeps the picture from drifting. |
| **Edit consistency** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Consistency-LoRA/resolve/main/qwen-image-2.1-consistency.safetensors) | Stops edits from resizing or nudging the picture. |
| **COS auto-retouching v2** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.16 GB | [![][cv-l15625089091628]](https://civitai.com/models/2975411/cosv2cos-auto-retouching-v2?modelVersionId=3372259) | One-step automatic retouching for cosplay photos. |
| **Pixel-offset fix** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-GooHAI]](https://huggingface.co/GooHAI/Qwen-image-2.1_loras/resolve/main/Qwen2.1%20%E4%B8%80%E8%87%B4%E6%80%A7%E6%97%A0%E5%81%8F%E7%A7%BB_GH_V1.0.safetensors) | Removes the pixel offset that shifts Qwen 2.1 output. No trigger word; strength 0.7. |
| **Fix** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.11 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix/resolve/main/qwen-image-2.1-fix-1.0-comfy.safetensors) | Removes the AI look from generations. |
| **Deblur anything** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-Luntrix]](https://civitai.com/models/2975487/deblur-anything?modelVersionId=3372351) | Removes defocus blur from a photograph. |
| **Remove bokeh** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-Luntrix]](https://civitai.com/models/2976381/edit-remove-bokeh-remove-blur-sharp-background?modelVersionId=3373431) | Sharpens a background the camera left out of focus. |
| **Watermark removal** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-saladin]](https://civitai.com/models/2969142/watermark-removal-lora?modelVersionId=3364468) | Erases watermarks without repainting the rest of the image. |
| **Fix v2.0** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.05 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix-v2.0/resolve/main/qwen2.1-detail-fix-2.0.safetensors) | The Fix idea rerun at rank 8. |
|  |  |  |  |  |  |
| **De-AI LoRA pack** | ![Style][ltype-style] | ![bf16][badge-bf16] | 2.45 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora) | An 8-file pack for removing the AI look. |
| **Fyre** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.81 GB | [![][cv-jandordoe]](https://civitai.com/models/2386526/fyre-fictional-character?modelVersionId=3353158) | Fictional redhead character with freckles. |
| **Anime LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.67 GB | [![][gh-mfxq2l]](https://huggingface.co/mfxq2l/qwen-image2.1-lora-Anime) | Anime illustration style; trigger `Anime`. Eight checkpoints from step 1000 to 8000. |
| **Fwaun Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.61 GB | [![][cv-fwaunstp733]](https://civitai.com/models/2593831/fwaun-style?modelVersionId=3373050) | Semi-chibi character style with photorealistic backgrounds. |
| **Asian portrait photography** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.59 GB | [![][cv-Dream2046]](https://civitai.com/models/3011109/asian-portrait-photography-or-qwen-image-21-600px-lora?modelVersionId=3417262) | Portrait photography look for East Asian subjects. |
| **Natural Exposure LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA) | Exposure correction. |
| **ZAvali Qwen2.1** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.31 GB | [![][cv-Zarj]](https://civitai.com/models/3001929/zavali-qwen21?modelVersionId=3405375) | Character-style LoRA rebuilt for Qwen 2.1. |
| **Anything2RealCharacters** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][cv-AIGC--Singularity]](https://civitai.com/models/2808492/anything2realcharacters?modelVersionId=3363600) | Turns 2D or painted input into a photorealistic character. |
| **35mm Film Texture** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][cv-Deno2026]](https://civitai.com/models/3010645/qwen-image-21-krea2-35mm-film-texture-and-realism-lora?modelVersionId=3416647) | 35mm film texture and realism. |
| **Candid Amateur iPhone** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][cv-Artfat]](https://civitai.com/models/3012015/candid-amateur-iphone-moss-image-or-qwen-21?modelVersionId=3441588) | Candid phone-photo look: flat everyday light, natural skin, real rooms; trigger `cndamiph`. |
| **SweetCreamCake Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.30 GB | [![][cv-spanking__cutie]](https://civitai.com/models/979544/sweetcreamcake-style?modelVersionId=3437863) | Anime restyle; trigger `transform the image into a sweetcreamcake anime style`. |
| **Fantasy Art Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.27 GB | [![][cv-aimann]](https://civitai.com/models/2996512/art-styles-qwen-image-21?modelVersionId=3399871) | Painterly fantasy-art style. |
| **Flux Pro Ultra Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.27 GB | [![][cv-aimann]](https://civitai.com/models/2836241/flux-pro-ultra-style?modelVersionId=3399725) | The saturated Flux.1 Pro Ultra look. |
| **Clay Sculpture** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Clay-Sculpture-Style/resolve/main/Qwen-Image-2.1-Clay-Sculpture-Style-2000.safetensors) | Turns a photo into a clay-sculpture look. |
| **Voxel** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Voxel-Style/resolve/main/Qwen-Image-2.1-Voxel-Style-2000.safetensors) | The same author's voxel-cube look. |
| **Amethyst Theme** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Amethyst-Theme/resolve/main/Qwen-Image-2.1-Amethyst-Theme-2000.safetensors) | An amethyst mystical theme from the same series. |
| **Outrun** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Outrun/resolve/main/Qwen-Image-2.1-Outrun-2000.safetensors) | Neon-noir synthwave look with magenta and cyan rim light. |
| **Heatwave** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Heatwave/resolve/main/Qwen-Image-2.1-Heatwave-2000.safetensors) | A thermal-shimmer look from the same author. |
| **Casual phone photo** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-19nbrown]](https://huggingface.co/19nbrown/qwen-image-2.1-casual-phone-photo-style-lora/resolve/main/lbxstyle-qwen-image-2.1-step-1890.safetensors) | Adds a casual phone-photo aesthetic to generation. |
| **Product relight** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora-2104918997757157378/resolve/main/Qwenimag21_c2-st2000.safetensors) | Lights a product so it sits in its background with a real contact shadow. |
| **De-AI + lighting v5** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora-2071763057486946305/resolve/main/Qwen%20Image%202.1%E5%8E%BB%E9%99%A4ai%2B%E5%85%89%E5%BD%B1%E4%BC%98%E5%8C%96v5.safetensors) | Removes the AI look and relights, v5 being the newer pair. |
| **Dot Film** | ![Style][ltype-style] | ![fp32][badge-fp32] | 0.17 GB | [![][gh-foprc]](https://huggingface.co/foprc/Dot-Film-Qwen-Image-2.1/resolve/main/dot-film-v0.1-qwen-image-2.1-step1000.safetensors) | Direct-flash 35 mm point-and-shoot look with hard flash and coarse grain. |
| **Product detail** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image2.1-lora-2103019069862666241/resolve/main/qwen%20image2.1%E4%BA%A7%E5%93%81%E7%BB%86%E8%8A%82%E5%9B%BE.safetensors) | Product-detail photography look. |
| **ZZZ style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-wesjos]](https://huggingface.co/wesjos/Qwen-Image2.1-ZenlessZoneZero-Lora/resolve/main/zzz_style_v1.7_4500.safetensors) | Zenless Zone Zero-inspired look. |
| **Genshin TCG 2D** | ![Style][ltype-style] | ![f16][badge-f16] | 0.16 GB | [![][gh-wesjos]](https://huggingface.co/wesjos/Qwen-Image2.1-Genshin-2D-Lora/resolve/main/genshintcg_style_st3500.safetensors) | Trading-card anime rendering with flat cel shading and hard outlines. |
| **mikkoani Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.16 GB | [![][cv-mikkoph]](https://civitai.com/models/2470371/mikkoani-style?modelVersionId=3400689) | Muted-colour anime style with complex linework. |
| **mikkoph Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.16 GB | [![][cv-mikkoph]](https://civitai.com/models/2415137/mikkoph-style?modelVersionId=3399282) | Photo-real portrait style with distinctive makeup and lighting. |
| **Porsche 911 Targa** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.16 GB | [![][cv-mastermerlin]](https://civitai.com/models/1446901/porsche-911-targa-1970s?modelVersionId=3374780) | 1970s Porsche 911 Targa subject. |
| **SEEDRAAI Polaroid** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.16 GB | [![][cv-cfgscale]](https://civitai.com/models/3035191/seedraai-or-polaroid-instant-film-or-qwen-image-21?modelVersionId=3448724) | Polaroid instant-film look with a white frame and hard on-camera flash; trigger `p0laroid`, weight 0.7. |
| **Ricklistic Anime to real** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-RICK666]](https://civitai.com/models/2964789/ricklistic-anime-to-real?modelVersionId=3359189) | Turns anime and manga art into a realistic rendering. |
| **MoreReal** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-NO8D]](https://civitai.com/models/2904424/morereal?modelVersionId=3374264) | Turns any painting or render into a photograph. |
| **Manga to Real People V1** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-qingfengy36939]](https://civitai.com/models/2984825/qwen-image-21-manga-to-real-people-v1?modelVersionId=3383994) | Manga-to-photo conversion that keeps pose, clothing and scene. |
| **AniVive** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-VividMuse]](https://civitai.com/models/3001515/aniviveqwen21?modelVersionId=3404837) | Anime-to-photo edit that keeps hair, outfit and pose. |
| **Suzie** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-chloeana]](https://civitai.com/models/2999397/suzie-fictional-character?modelVersionId=3402154) | Fictional-character LoRA. |
| **Chloe** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-chloeana]](https://civitai.com/models/2407779/chloe-fictional-character?modelVersionId=3402097) | Fictional-character LoRA. |
| **Ivy** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-chloeana]](https://civitai.com/models/2406001/ivy-fictional-character?modelVersionId=3402000) | Fictional-character LoRA. |
| **Jianna** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-chloeana]](https://civitai.com/models/2403689/jianna-fictional-character?modelVersionId=3401938) | Fictional-character LoRA. |
| **Olesya Poryadko** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-rakamik]](https://civitai.com/models/2868124/olesya-poryadko-this-person-does-not-exist?modelVersionId=3412071) | Fictional-person character LoRA. |
| **DeepSeek Whalegirl** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-updatemp]](https://civitai.com/models/2947136/deepseek-whalegirl?modelVersionId=3410681) | Character LoRA for the whalegirl persona. |
| **Njsemi Fantasy Style** | ![Style][ltype-style] | ![fp16][badge-fp16] | 0.15 GB | [![][cv-NoahGreyWood]](https://civitai.com/models/3009320/njsemi-cinematic-fantasy-style-for-qwen-image-21?modelVersionId=3414917) | Cinematic fantasy style for the njsemi character; it pushes figures toward fuller bodies. |
| **Colored Pencil** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-ArsMachina]](https://civitai.com/models/1155749/hyperdetailed-colored-pencil?modelVersionId=3417967) | Hyperdetailed colored-pencil illustration style. |
| **Sorcery & Steel** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-ArsMachina]](https://civitai.com/models/2818260/sorcery-and-steel?modelVersionId=3420683) | Dungeons and dragons meets heavy-metal fantasy; trigger `Sorcery & Steel`. |
| **Opulent Darkness** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-ArsMachina]](https://civitai.com/models/2863183/opulent-darkness?modelVersionId=3428826) | A dark fantasy trading-card look; trigger `OpulentDarkness`. |
| **Classic Anime** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-__Envy__]](https://civitai.com/models/3017501/qwenvyqwen-21-classic-anime?modelVersionId=3425643) | A realistic anime style in the register of the serious anime shows. |
| **Dark Ghibli** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-ArsMachina]](https://civitai.com/models/1349631/dark-ghibli-fairytales?modelVersionId=3424136) | Studio Ghibli meets dark fairy tales; trigger `Studio Ghibli Dark Fairytale`. |
| **Claude chan (young)** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-updatemp]](https://civitai.com/models/3029351/claude-claude-chan?modelVersionId=3446685) | Character LoRA trained on 30 views; the Qwen 2.1 build of the young persona. |
| **Mecha Engrave** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-Xabsurd]](https://civitai.com/models/3032793/mecha-engrave?modelVersionId=3445670) | Adds armor depth, panel lines and glowing energy parts; trigger `nijimecha`. The author ranks this Qwen build last of the five. |
| **Siemens CX70** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.15 GB | [![][cv-Fenfel23]](https://civitai.com/models/2744351/siemens-cx70?modelVersionId=3450666) | Early-2000s CCD camera-phone look: sensor noise, highlight bloom and soft rendering; trigger `s13m3ns style.`, best at 1 MP. |
| **Sts2 Cards Drawer** | ![Style][ltype-style] | ![fp16][badge-f16] | 0.10 GB | [![][gh-Airmongsity]](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer) | 2D card-art style. |
| **ZZZ chibi** | ![Style][ltype-style] | ![f16][badge-f16] | 0.08 GB | [![][gh-wesjos]](https://huggingface.co/wesjos/Qwen-Image2.1-ZenlessZoneZeroChibi-Lora/resolve/main/zzz_chibi_v2_st2500.safetensors) | Compresses proportions to chibi rather than just restyling. |
| **2anime** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][gh-NicknickAI]](https://huggingface.co/NicknickAI/QwenImage2.1-2anime/resolve/main/2anime-QwenImage2.1.safetensors) | Turns a photo into anime, image to image. |
| **Hanfu Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-Dream2046]](https://civitai.com/models/3004075/qwen-image-21-hanfu-style?modelVersionId=3408146) | Traditional Chinese clothing style. |
| **Tattoo for Stencil** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-jblm]](https://civitai.com/models/2984373/tattoo-for-stencil-qwen21?modelVersionId=3400405) | Black-on-white tattoo-stencil style. |
| **GPT-Image-2 Anime Style** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-makimakimakima12138]](https://civitai.com/models/2736538/gpt-image-2-anime-illustration-style-lora-gptimage2-gpt-image-2-loragptimage2-gpt-image-2-loragptimage2?modelVersionId=3360462) | Anime illustration style. |
| **Night Vision P45** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-Exis10ce]](https://civitai.com/models/2999325/night-vision-photography-white-phosphor-p45-image-intensifier-qwen-21-krea-2?modelVersionId=3402058) | White-phosphor night-vision look. |
| **Night Vision P43** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-Exis10ce]](https://civitai.com/models/2946904/night-vision-photography-green-phosphor-p43-image-intensifier-krea-2-qwen-21?modelVersionId=3395186) | Green-phosphor night-vision look. |
| **Klein-9b Turn2Real** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-eznorb]](https://civitai.com/models/2406218/klein-9b-turn2real?modelVersionId=3404257) | Turn-to-realism restyle pass. |
| **HYY character** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image2.1-hyy-lora/resolve/main/hyy1_c1-st1000.safetensors) | A named character style behind the trigger `hyy`. |
| **KreLuminA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-henxpoi521]](https://civitai.com/models/2935404/krelumina?modelVersionId=3345973) | A half-realistic look with blown highlights; trained on 11 images, so expect overfitting. |
| **YUL-GILRS** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-wwwmanmoon103]](https://civitai.com/models/2994272/yul-girls-lora?modelVersionId=3395734) | A character LoRA; the trigger starts with `yul`. Only this character of the series has a Qwen 2.1 build. |
| **Star Trek TNG Type 2 Phaser** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.08 GB | [![][cv-lcarspicturesdatabase]](https://civitai.com/models/3025318/star-trek-tng-type-2-phaser?modelVersionId=3443413) | Object LoRA for the TNG Type 2 phaser prop; trigger `TNGTypeII2364`. |
| **Canon UltraReal** | ![Style][ltype-style] | ![f16][badge-f16] | 0.07 GB | [![][cv-Danrisi]](https://civitai.com/models/2783143/canon-ultrareal?modelVersionId=3352035) | The look of a full-frame Canon camera: rich colour, deep bokeh. |
| **GrainScape UltraReal** | ![Style][ltype-style] | ![f16][badge-f16] | 0.07 GB | [![][cv-Danrisi]](https://civitai.com/models/1332651/grainscape-ultrareal?modelVersionId=3352222) | Coarse film grain with deeper colour. |
| **NiceGirls UltraReal** | ![Style][ltype-style] | ![f16][badge-f16] | 0.07 GB | [![][cv-Danrisi]](https://civitai.com/models/1862761/nicegirls-ultrareal?modelVersionId=3352079) | Portrait realism for female subjects. |
| **RAW style photo** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-latent--space--dreams]](https://civitai.com/models/1751076/raw-style-photo?modelVersionId=3391625) | Converts a render toward a RAW-photograph look. |
| **Y2K digicam** | ![Style][ltype-style] | ![fp32][badge-fp32] | 0.07 GB | [![][gh-empero--ai]](https://huggingface.co/empero-ai/Homebrew-Qwen-Image-2.1-Y2K/resolve/main/pytorch_lora_weights.safetensors) | Early-2000s digicam look with direct-flash highlights and soft detail. |
| **Remix** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-NO8D]](https://civitai.com/models/2937570/remix?modelVersionId=3405269) | Experimental restyle pass. |
| **Lenovo UltraReal** | ![Style][ltype-style] | ![fp16][badge-f16] | 0.07 GB | [![][cv-Danrisi]](https://civitai.com/models/1662740/lenovo-ultrareal?modelVersionId=3349565) | Candid ultrareal portrait style. |
| **2000s Analog Core** | ![Style][ltype-style] | ![fp16][badge-fp16] | 0.07 GB | [![][cv-Danrisi]](https://civitai.com/models/1134895/2000s-analog-core?modelVersionId=3378081) | 2000s analog video look: scan lines, tape noise and faded colour. |
| **Triclops** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-2__as__1]](https://civitai.com/models/3013514/triclops-three-eyes-qwen-edit?modelVersionId=3420410) | Adds a third eye; trigger `add a third eye`. |
| **Tail - Qwen Edit** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-2__as__1]](https://civitai.com/models/3031262/tail-qwen-edit?modelVersionId=3443640) | Adds a tail; trigger `add a tail`. A red line drawn on the input guides its shape. |
| **Skeletor** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.02 GB | [![][cv-DialUpDialDown]](https://civitai.com/models/2835069/skeletor-masters-of-the-universe?modelVersionId=3345001) | Skeletor character from Masters of the Universe. |
|  |  |  |  |  |  |
| **Pokemon foil mask** | ![Utility][ltype-utility] | ![fp32][badge-fp32] | 0.34 GB | [![][gh-newHashMap]](https://huggingface.co/newHashMap/PokemonFoilMask-LoRA-Qwen-Image-2.1/resolve/main/pytorch_lora_weights.safetensors) | Predicts the grayscale foil mask a card shader needs. Scored on 820 held-out test cards. |
| **Normal2RGB** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-Aero--Ex]](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB/resolve/main/Normal2RGB_4000.safetensors) | Normal map → RGB render, 3 checkpoints. |
| **White model** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora-2103639501527670786/resolve/main/QI2.1Convert-to-white-model.safetensors) | Strips people down to grey mannequins while keeping posture and camera angle. |
| **Image Translator** | ![Utility][ltype-utility] | ![f16][badge-f16] | 0.16 GB | [![][gh-ysharma]](https://huggingface.co/ysharma/Qwen-Image-2.1-image-translator-LoRA/resolve/main/imtrans_full_2000_gate_up_split.safetensors) ┊ [![][gh-ysharma]](https://huggingface.co/ysharma/Qwen-Image-2.1-image-translator-LoRA/resolve/main/imtrans_full_000001500_gate_up_split.safetensors) | Rewrites text inside an image in another language, leaving the rest alone. |
| **Restore** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Restore-LoRA/resolve/main/qwen-image-2.1-restore.safetensors) ┊ [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Restore-LoRA/resolve/main/qwen-image-2.1-restore-1250.safetensors) | Softens the base model's own over-eager upscaling. |
| **Masked removal** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-GooHAI]](https://huggingface.co/GooHAI/Qwen-image-2.1_loras/resolve/main/Qwen2.1_%E4%B8%87%E7%89%A9%E7%A7%BB%E9%99%A4Remove_GH.safetensors) | Removes the green region you mark, with no pixel shift or colour cast. Strength 1.0. |
| **Font / poster text** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.08 GB | [![][gh-dx8152]](https://huggingface.co/dx8152/Qwen-Image-2.1-Font/resolve/main/Qwen2-1-Font.safetensors) | Text layout and poster design. |
| **High resolution** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.07 GB | [![][cv-NO8D]](https://civitai.com/models/2436859/highquality?modelVersionId=3396843) | Restores high-definition detail. |
| **Clothing Sheet** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.04 GB | [![][cv-ladaagency]](https://civitai.com/models/3000995/clothing-sheet-qwen-image-21-edit-lora?modelVersionId=3404186) | Flat-lay clothing sheet from one photo. |
| **Angles Sheet** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.04 GB | [![][cv-ladaagency]](https://civitai.com/models/3000994/angles-sheet-qwen-image-21-edit-lora?modelVersionId=3404185) | 2x2 angle reference sheet from one photo. |
| **Expressions Sheet** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.04 GB | [![][cv-ladaagency]](https://civitai.com/models/3000993/expressions-sheet-qwen-image-21-edit-lora?modelVersionId=3404184) | 3x2 expression grid from one photo. |
|  |  |  |  |  |  |
| **Uncensored Omni v0.1** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.67 GB | ⚠️ [![][gh-chatrlabs]](https://huggingface.co/chatrlabs/Qwen-Image-2.1-Uncensored-Omni-LoRA-v0.1/resolve/main/chatrlabs-omni-lora-v0.1-bf16.safetensors) | One adapter for both text-to-image and image editing. |
| **Hips / buttocks** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.47 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora/resolve/main/qwen2509%E8%87%80%E9%83%A8%E6%94%BE%E5%A4%A7.safetensors) | Body-shape slider. |
| **FUSAL Anime** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.42 GB | ⚠️ [![][gh-MicksHF]](https://huggingface.co/MicksHF/Qwen-Image-2.1-Anime-Fusal/resolve/main/loras/fusal-qwen-image-2.1-v2-exact-r80.safetensors) | Wide-domain anime style for the original Base; start at 0.7 strength with the `fusal style.` prefix. The Turbo E6 preview is a separate file for the Turbo base, and the repo bundles the matching Heretic text encoder. |
| **Asian mix** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.32 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen21-hina-qwenimage2.1-asianmix-lora-v3.0-rank64-lora/resolve/main/Qwen21-%E4%BA%9A%E6%B4%B2hina_QwenImage2.1_asianMix_Lora_v3.0-rank64.safetensors) | A portrait LoRA on a single named character. |
| **Realism transform** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.30 GB | ⚠️ [![][cv-sh1201]](https://civitai.com/models/2985682/realism-transform?modelVersionId=3384983) | Anime-to-photo edit that keeps the composition. Strength 0.8-1.2, under 2 MP. Non-commercial only. |
| **Belt Spanking QE21** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.30 GB | ⚠️ [![][cv-spanking__cutie]](https://civitai.com/models/1732301/belt-spanking-or-il-or-bdsm-or-spanking?modelVersionId=3451555) | Belt-spanking concept; trigger `make the spanker swing a leather belt`. The QE21 v1 build is a stepping stone to the upcoming v2. |
| **Dual anatomy** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.24 GB | ⚠️ [![][gh-glowleaf]](https://huggingface.co/glowleaf/qwen-image-2.1-futanari-lora/resolve/main/futa_qwen21_v1_000001000.safetensors) | Adds a dual anatomy to the subject. |
| **Breasts and hips** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora/resolve/main/Qwen%20Image%202.1%E5%A4%A7%E8%83%B8%E5%A4%A7%E8%87%80.safetensors) | Combined body-shape adapter from the same pack. |
| **RadianceChrome Voluptuous** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-AIImageStudio]](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0) | Character-style LoRA. |
| **NSFW LoRA** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.16 GB | ⚠️ [![][gh-Wickedlizerd]](https://huggingface.co/Wickedlizerd/NSFW-Qwen-Image-2.1-LoRA/resolve/main/nsfw_qwen_21.safetensors) | Adult-content adapter that wants a full sampler schedule. |
| **Bust resize** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-auxilia]](https://civitai.com/models/2182514/fake-breasts-bimbo-fake-tits?modelVersionId=3399192) | Adjusts breast size on generated bodies. |
| **Harness gear** | ![NSFW][ltype-nsfw] | ![fp16][badge-f16] | 0.15 GB | ⚠️ [![][cv-PromptHippo]](https://civitai.com/models/2997058/ponygirl-pony-play-gear-qwen-21?modelVersionId=3399235) | Head-harness and tail outfit style. |
| **Cosplay** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-mrkakapopoloch]](https://civitai.com/models/2791428/cosplay?modelVersionId=3400404) | Cosplay outfit style; also works for editing. |
| **Anime Flat Style** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-Agino]](https://civitai.com/models/1952560/anime-flat-style?modelVersionId=3361221) | Flat cel-shaded anime style. |
| **Natural Body Hair** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-Rimya]](https://civitai.com/models/2822722/natural-body-hair-qwen21-krea2?modelVersionId=3408091) | Adds natural body-hair detail. |
| **Rear reshape** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-auxilia]](https://civitai.com/models/2181576/fake-ass-silicone-bubble-butt?modelVersionId=3397263) | Adds a rounded, silicone-like rear shape. |
| **Chastity gear** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-Wimile]](https://civitai.com/models/3011844/chastity-belt-and-chastity-bra-for-qwen21?modelVersionId=3418236) | Chastity belt, bra and cuff set. |
| **Sitting upskirt** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-itwasfake]](https://civitai.com/models/3017870/sitting-upskirt?modelVersionId=3426120) | Rewrites a photo into a sitting-upskirt view, no mask needed. Best at 2048x2048, CFG 1-3. |
| **PHOTOSTYLE by Sean Archer** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-FASCIUM]](https://civitai.com/models/1632416/photostyle-by-sean-archer?modelVersionId=3402309) | Portrait-photographer photo style. |
| **Qwen 2.1 Emote V2** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-AirbagGuy]](https://civitai.com/models/3032955/qwen-21-emote-lora?modelVersionId=3450591) | 99 facial expressions and emotes, two of them adult; V2 works at full strength without flattening photoreal faces. |
| **RLY Thot Shot - Jada** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.15 GB | ⚠️ [![][cv-RaymondLuxuryYacht]](https://civitai.com/models/2558101/rly-thot-shot-jada-qwen21-krea-2-ideogram-4-zibzit-k9b?modelVersionId=3357572) | Female character LoRA; trigger `rlyjada` at strength 1.0. The author calls this Qwen 2.1 build a proof of concept. |
| **NSFW Image Edit** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.08 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora/resolve/main/Qwen-Image-2.1%20NSFW%20Image%20Edit.safetensors) | Adult-content editing adapter. |
| **Glamour Realism** | ![NSFW][ltype-nsfw] | ![f16][badge-f16] | 0.07 GB | ⚠️ [![][cv-TP--AI--63]](https://civitai.com/models/2986100/qwen-image-21-glamour-realism-lora-16-gb-tested?modelVersionId=3385496) | Gentle glamour-portrait polish with natural skin. |
| **Miniskirt style** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-mrkakapopoloch]](https://civitai.com/models/1498700/micro-skirt?modelVersionId=3400203) | Short-skirt outfit subject. |
| **Transform To photorealism** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-imagC]](https://civitai.com/models/3002555/transform-to-photorealism?modelVersionId=3412029) | Converts a render toward photorealism. |
| **Swimsuit tan lines** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-coachbate]](https://civitai.com/models/2197413/speedo-tan-line?modelVersionId=3413604) | Adds a swimsuit tan-line detail. |
| **Conjoined Two Heads** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-2__as__1]](https://civitai.com/models/3001603/conjoined-two-heads-qwen-edit?modelVersionId=3404953) | Two-headed body edit. |
| **Legs with stockings 5** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/3002465/nude-collection-westernwomanlegswithstockings5qwen-image-21?modelVersionId=3406061) | Stocking-leg subject LoRA. |
| **Legs with stockings 3** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/3001478/nude-collection-westernwomanlegswithstockings3qwen-image-21?modelVersionId=3404784) | Stocking-leg subject LoRA. |
| **Legs with stockings 6** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/3000817/nude-collection-westernwomanlegswithstockings6qwen-image-21?modelVersionId=3403960) | Stocking-leg subject LoRA. |
| **Legs with stockings 4** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/2999997/nude-collection-westernwomanlegswithstockings4qwen-image-21?modelVersionId=3402884) | Stocking-leg subject LoRA. |
| **Legs with stockings 1** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/2999253/nude-collection-westernwomanlegswithstockings1qwen-image-21?modelVersionId=3401970) | Stocking-leg subject LoRA. |
| **Nude figure 22** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/2998123/nude-collection-westernwomanfullnude22qwen-image-21?modelVersionId=3400844) | Nude-figure subject LoRA. |
| **Nude figure 11** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-dushuai]](https://civitai.com/models/2997556/nude-collection-westernwomanfullnude11qwen-image-21?modelVersionId=3399893) | Nude-figure subject LoRA. |
| **Anatomy correction** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-nil__sphere]](https://civitai.com/models/3012772/revulva?modelVersionId=3419430) | Corrects the base model's female anatomy. Trained on 40 stripped images. |
| **Milkmaid** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-mrkakapopoloch]](https://civitai.com/models/377718/hucow-milking?modelVersionId=3397155) | A milking theme; trigger `m1lk1ng`. Also works for editing. |
| **Showa shibari** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-wolfcatz]](https://civitai.com/models/136849/bondage-go-bdsm-lora-for-moss-qwen21-zib-f1-and-xl?modelVersionId=3422389) | Showa-era shibari; trigger `shibari` plus the era name. Weight 0.3-1.0. |
| **SamsungCam** | ![NSFW][ltype-nsfw] | ![f16][badge-f16] | 0.07 GB | ⚠️ [![][cv-Danrisi]](https://civitai.com/models/1551668/samsungcam-ultrareal?modelVersionId=3354434) | The S-series phone look; strength 0.7, retrained on S25 Ultra shots. |
| **AAS - Awesome Ankle Socks** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.07 GB | ⚠️ [![][cv-Mister__A]](https://civitai.com/models/3020499/aas-awesome-ankle-socks?modelVersionId=3448966) | Ankle-sock clothing LoRA for female subjects; trigger `aasocks`, weight 1.0-1.2. |
| **NSFW unlocked** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.05 GB | ⚠️ [![][cv-The__Midnight__Lab]](https://civitai.com/models/2063193/creative-generation-model?modelVersionId=3392016) | Fully uncensored; 4.5k steps, 200 images, CFG 3.5, weight 0.5-0.9. |
| **Metal breast clamp** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.02 GB | ⚠️ [![][cv-wolfcatz]](https://civitai.com/models/2290707/breasts-clampclamp2-for-qwen21-anima-and-zit?modelVersionId=3418159) | Clamps the breasts with metal bars and screws. |
| **Breasts Slider V1** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.003 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora/resolve/main/Pornmaster_QI2.1_Breasts_Slider_V1.safetensors) | Slider control. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used, the uncensored GGUF in particular. They carry the same **Qwen Research License** as the base model, and a research license is not a license to do whatever you want; you remain responsible for how you use them. Sizes are the LoRA file only; none of these merge a base model.

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

<p id="tools" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚙ Tools &amp; notebooks

| Name | Type | Links | Notes |
| :--- | :---: | :---: | :--- |
| **training assistant v1** | ![Training][ltype-training] | [![][gh-SimpleTuner]](https://huggingface.co/SimpleTuner/Qwen-Image-2.1-training-assistant-v1) | A working `pytorch_lora_weights.safetensors` + `training_details.json` you can inspect to learn a real SimpleTuner config. |
| **Qwen-Image-2.1-Skills** | ![Agent skill][ltype-skill] | [![][gh-iamvts]](https://huggingface.co/iamvts/Qwen-Image-2.1-Skills) | Turns a short scene into a structured prompt for believable casual phone photography. 22 example images. |
| **qwen-image-2.1-p150** | ![Port][ltype-port] | [![][gh-changh95]](https://huggingface.co/changh95/qwen-image-2.1-p150) | Tenstorrent Blackhole p150a. `tt-model pull --with-weights` then `tt-model serve`. |
| **Phrase Weights** | ![Node pack][ltype-nodes] | [![capitan01R](https://img.shields.io/badge/capitan01R-17a2b8?style=flat-square&logo=github&logoColor=white)](https://github.com/capitan01R/ComfyUI-qwen_img_2_1_enhancer) | Two native nodes the stock Qwen 2.1 graph lacks: **per-reference attention strength** (make one of three references dominate, default 1.2) and **`(phrase:weight)` prompt weighting** in the encoder. Despite the name it does not enhance, upscale or rewrite prompts; it only reweights attention, and weights add `log(w)` before softmax, so `2.0` is not twice the effect. Phrases must map to whole tokenizer pieces. Needs an attention backend with **additive masks**. MIT. |
| **TensorFold RTX loader** | ![Node pack][ltype-nodes] | [![jayleaton](https://img.shields.io/badge/jayleaton-17a2b8?style=flat-square&logo=github&logoColor=white)](https://github.com/jayleaton/qwen-image21-tensorfold-rtx) | ComfyUI node that replaces `Unet Loader (GGUF)` and runs the DiT through TensorFold's **NVFP4 and FP8** kernels, skipping the per-step Q6_K decode. Measured 4.0× faster at 1024×1024 on an RTX 5070 Ti. **RTX 50-series only**, since NVFP4 needs block-scaled FP4 tensor cores Blackwell has and Ada does not. The loader **refuses LoRA, ControlNet, weight patches and attention patches**. Takes the same GGUF you would otherwise load. Work in progress, Apache-2.0. |
| **ComfyUI-AlphaTrace** | ![Node pack][ltype-nodes] | [![Nynxz](https://img.shields.io/badge/Nynxz-17a2b8?style=flat-square&logo=github&logoColor=white)](https://github.com/Nynxz/ComfyUI-AlphaTrace) | Diagnostic sampler nodes for the native RGBA output: they emit the predicted image, the alpha map and per-step alpha statistics. Purely observational. For 2.1 pass **LTXVScheduler** as `sigmas` to match the official schedule. MIT, example workflow included. |
| **Qwen-Image-2.1 VRAM calculator** | ![Utility][ltype-utility] | [![][gh-modelvram]](https://modelvram.com/qwen-image-2-1-vram-calculator/) | Pick the DiT, text encoder and VAE files, where the encoder runs and the image size; it shows the peak VRAM range and which 8–32 GB GPUs fit. |
| **Image Studio** | ![Utility][ltype-utility] | [![][gh-tamimKTH]](https://github.com/tamimKTH/image-studio) | Local web app for Apple Silicon (64 GB+): text-to-image, edits with up to 10 reference images, transparent PNGs, and a node canvas that chains steps. Runs the Uncensored Q8_0 GGUF on ComfyUI. AGPL. |
| **Kokon** | ![Utility][ltype-utility] | [![][gh-GabrieleLozupone]](https://github.com/GabrieleLozupone/kokon) | Desktop app for Windows and macOS that runs the 8-step Turbo build on stable-diffusion.cpp, with no Python or ComfyUI. Text-to-image, instruction edits, up to 10 reference images, masked region edits, outpaint and upscale. Uses the Turbo Q4_K GGUF, so 12 GB of VRAM is enough; 16 GB+ runs edits natively at 2K. CUDA on NVIDIA, Vulkan and Metal in beta. Ships an MCP server and a local HTTP API. AGPL. |

**Gotchas**

1. `QwenImage21Pipeline` requires diffusers **`main`**, since release `0.40.0` does not have it. Install from git.
2. The CFG kwarg is **`true_cfg_scale`**, not `guidance_scale`, and it needs a `negative_prompt` to engage.
3. Width and height must be **multiples of 32**, max edge 2048.
4. With CPU offload, the generator should live on **`'cpu'`**.
5. The default sample is **40 steps with no CFG**. Adding CFG changes the look; don't assume more steps help.
6. Chinese prompts work natively. The PE models are what rewrite them into English.

---

<p id="contributing" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ✎ Contributing

This list is a **set of Markdown files, not one**. `README.md` is generated and
should never be edited directly, since your change will be overwritten on the next
build. Each section lives in its own file under [`blocks/`](blocks/):

| To change… | Edit this file |
| :--- | :--- |
| Title, badges, table of contents | [`blocks/00_header.md`](blocks/00_header.md) |
| Base model or official ComfyUI files | [`blocks/10_official_checkpoints.md`](blocks/10_official_checkpoints.md) |
| Prompt engines, rewriters, text encoders | [`blocks/20_text_encoders_prompt_engines.md`](blocks/20_text_encoders_prompt_engines.md) |
| GGUF, 4/8-bit, FP8, Nunchaku quants | [`blocks/30_quantizations.md`](blocks/30_quantizations.md) |
| Turbo and step-distilled models | [`blocks/40_turbo.md`](blocks/40_turbo.md) |
| LoRAs and adapters | [`blocks/50_loras.md`](blocks/50_loras.md) |
| Apple, MNN, AMD, VAE ports | [`blocks/60_platform_ports.md`](blocks/60_platform_ports.md) |
| Tools, notebooks, training scripts | [`blocks/70_tools.md`](blocks/70_tools.md) |
| Badge and link definitions | [`blocks/99_footer.md`](blocks/99_footer.md) |

The file prefix controls the order: the build concatenates `blocks/*.md` in
filename order, so `10_` always lands before `20_`. A new section just needs a
new number.

<div align="center">

**Awesome Qwen-Image 2.1**

</div>

<!-- OWNER BADGES -->
[gh-gguf-org]: https://img.shields.io/badge/gguf-org-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-19nbrown]: https://img.shields.io/badge/19nbrown-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-471Def]: https://img.shields.io/badge/471Def-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-AIImageStudio]: https://img.shields.io/badge/AIImageStudio-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Abiray]: https://img.shields.io/badge/Abiray-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-AcademiaSD]: https://img.shields.io/badge/AcademiaSD-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Aero--Ex]: https://img.shields.io/badge/Aero--Ex-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Airmongsity]: https://img.shields.io/badge/Airmongsity-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Alissonerdx]: https://img.shields.io/badge/Alissonerdx-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-BlazeMCworld]: https://img.shields.io/badge/BlazeMCworld-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Comfy--Org]: https://img.shields.io/badge/Comfy--Org-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-EliovpAI]: https://img.shields.io/badge/EliovpAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-FlagRelease]: https://img.shields.io/badge/FlagRelease-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Frosty40]: https://img.shields.io/badge/Frosty40-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-GabrieleLozupone]: https://img.shields.io/badge/GabrieleLozupone-lightgrey?style=flat-square&logo=github&logoColor=white
[gh-Gogodr]: https://img.shields.io/badge/Gogodr-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-GooHAI]: https://img.shields.io/badge/GooHAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-HangGlidersRule]: https://img.shields.io/badge/HangGlidersRule-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-HarleyWang]: https://img.shields.io/badge/HarleyWang-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Haverbex]: https://img.shields.io/badge/Haverbex-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-JoyFusionAI]: https://img.shields.io/badge/JoyFusionAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Karsus1997]: https://img.shields.io/badge/Karsus1997-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-LuffyTheFox]: https://img.shields.io/badge/LuffyTheFox-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-MIUProject]: https://img.shields.io/badge/MIUProject-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ML--Intern--lab]: https://img.shields.io/badge/ML--Intern--lab-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-MicksHF]: https://img.shields.io/badge/MicksHF-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ModelsLab]: https://img.shields.io/badge/ModelsLab-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-NicknickAI]: https://img.shields.io/badge/NicknickAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Noctaluna]: https://img.shields.io/badge/Noctaluna-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-PrunaAI]: https://img.shields.io/badge/PrunaAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Qwen]: https://img.shields.io/badge/Qwen-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-RunningHubAI]: https://img.shields.io/badge/RunningHubAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-SamuelTallet]: https://img.shields.io/badge/SamuelTallet-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-SimpleTuner]: https://img.shields.io/badge/SimpleTuner-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Sutoonq]: https://img.shields.io/badge/Sutoonq-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Viggle]: https://img.shields.io/badge/Viggle-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Wickedlizerd]: https://img.shields.io/badge/Wickedlizerd-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Zabin]: https://img.shields.io/badge/Zabin-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-abenzerps]: https://img.shields.io/badge/abenzerps-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-addlabsviral]: https://img.shields.io/badge/addlabsviral-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-akhaliq]: https://img.shields.io/badge/akhaliq-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-alibaba--pai]: https://img.shields.io/badge/alibaba--pai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ausboss]: https://img.shields.io/badge/ausboss-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-base11231]: https://img.shields.io/badge/base11231-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-catplusplus]: https://img.shields.io/badge/catplusplus-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-cgb]: https://img.shields.io/badge/cgb-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-changh95]: https://img.shields.io/badge/changh95-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-chatrlabs]: https://img.shields.io/badge/chatrlabs-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-chfm]: https://img.shields.io/badge/chfm-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-circulus]: https://img.shields.io/badge/circulus-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-congruency]: https://img.shields.io/badge/congruency-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-darrellbest]: https://img.shields.io/badge/darrellbest-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-devin--lai]: https://img.shields.io/badge/devin--lai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-dh123456789123]: https://img.shields.io/badge/dh123456789123-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-diffnamehard]: https://img.shields.io/badge/diffnamehard-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-dx8152]: https://img.shields.io/badge/dx8152-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-e--n--v--y]: https://img.shields.io/badge/e--n--v--y-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-empero--ai]: https://img.shields.io/badge/empero--ai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-evankuo]: https://img.shields.io/badge/evankuo-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-foofifoo]: https://img.shields.io/badge/foofifoo-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-foprc]: https://img.shields.io/badge/foprc-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-glowleaf]: https://img.shields.io/badge/glowleaf-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-gribbliest]: https://img.shields.io/badge/gribbliest-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-iamvts]: https://img.shields.io/badge/iamvts-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-jyohukuchan]: https://img.shields.io/badge/jyohukuchan-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-kkxao]: https://img.shields.io/badge/kkxao-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-lightx2v]: https://img.shields.io/badge/lightx2v-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-lilylilith]: https://img.shields.io/badge/lilylilith-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ling0322]: https://img.shields.io/badge/ling0322-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-linoyts]: https://img.shields.io/badge/linoyts-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-madebyollin]: https://img.shields.io/badge/madebyollin-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-mfxq2l]: https://img.shields.io/badge/mfxq2l-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-mingyi456]: https://img.shields.io/badge/mingyi456-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-modelvram]: https://img.shields.io/badge/modelvram.com-lightgrey?style=flat-square
[gh-myllmbox]: https://img.shields.io/badge/myllmbox-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-netdur]: https://img.shields.io/badge/netdur-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-netrunner--exe]: https://img.shields.io/badge/netrunner--exe-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-newHashMap]: https://img.shields.io/badge/newHashMap-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-og1936]: https://img.shields.io/badge/og1936-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ped4enko]: https://img.shields.io/badge/ped4enko-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-pottokao]: https://img.shields.io/badge/pottokao-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-prithivMLmods]: https://img.shields.io/badge/prithivMLmods-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-realrebelai]: https://img.shields.io/badge/realrebelai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-sarvamaigc]: https://img.shields.io/badge/sarvamaigc-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-t8star]: https://img.shields.io/badge/t8star-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-tamimKTH]: https://img.shields.io/badge/tamimKTH-lightgrey?style=flat-square&logo=github&logoColor=white
[gh-themindstudio]: https://img.shields.io/badge/themindstudio-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-trmz]: https://img.shields.io/badge/trmz-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-unsloth]: https://img.shields.io/badge/unsloth-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-wesjos]: https://img.shields.io/badge/wesjos-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-xingewh]: https://img.shields.io/badge/xingewh-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-yhzr2222]: https://img.shields.io/badge/yhzr2222-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ysharma]: https://img.shields.io/badge/ysharma-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-yunfengwang]: https://img.shields.io/badge/yunfengwang-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-zcf0508]: https://img.shields.io/badge/zcf0508-lightgrey?style=flat-square&logo=huggingface&logoColor=white

<!-- CIVITAI AUTHOR BADGES -->
[cv-2__as__1]: https://img.shields.io/badge/2__as__1-lightgrey?style=flat-square
[cv-AHEKOT]: https://img.shields.io/badge/AHEKOT-lightgrey?style=flat-square
[cv-AIGC--Singularity]: https://img.shields.io/badge/AIGC--Singularity-lightgrey?style=flat-square
[cv-Agino]: https://img.shields.io/badge/Agino-lightgrey?style=flat-square
[cv-AiMetatron]: https://img.shields.io/badge/AiMetatron-lightgrey?style=flat-square
[cv-AirbagGuy]: https://img.shields.io/badge/AirbagGuy-lightgrey?style=flat-square
[cv-AishaAI]: https://img.shields.io/badge/AishaAI-lightgrey?style=flat-square
[cv-ArsMachina]: https://img.shields.io/badge/ArsMachina-lightgrey?style=flat-square
[cv-Artfat]: https://img.shields.io/badge/Artfat-lightgrey?style=flat-square
[cv-Danrisi]: https://img.shields.io/badge/Danrisi-lightgrey?style=flat-square
[cv-Deno2026]: https://img.shields.io/badge/Deno2026-lightgrey?style=flat-square
[cv-DialUpDialDown]: https://img.shields.io/badge/DialUpDialDown-lightgrey?style=flat-square
[cv-Dream2046]: https://img.shields.io/badge/Dream2046-lightgrey?style=flat-square
[cv-Exis10ce]: https://img.shields.io/badge/Exis10ce-lightgrey?style=flat-square
[cv-FASCIUM]: https://img.shields.io/badge/FASCIUM-lightgrey?style=flat-square
[cv-Fenfel23]: https://img.shields.io/badge/Fenfel23-lightgrey?style=flat-square
[cv-Luntrix]: https://img.shields.io/badge/Luntrix-lightgrey?style=flat-square
[cv-Mister__A]: https://img.shields.io/badge/Mister__A-lightgrey?style=flat-square
[cv-NO8D]: https://img.shields.io/badge/NO8D-lightgrey?style=flat-square
[cv-NoahGreyWood]: https://img.shields.io/badge/NoahGreyWood-lightgrey?style=flat-square
[cv-PromptHippo]: https://img.shields.io/badge/PromptHippo-lightgrey?style=flat-square
[cv-RICK666]: https://img.shields.io/badge/RICK666-lightgrey?style=flat-square
[cv-Ray3780]: https://img.shields.io/badge/Ray3780-lightgrey?style=flat-square
[cv-RaymondLuxuryYacht]: https://img.shields.io/badge/RaymondLuxuryYacht-lightgrey?style=flat-square
[cv-Rimya]: https://img.shields.io/badge/Rimya-lightgrey?style=flat-square
[cv-TP--AI--63]: https://img.shields.io/badge/TP--AI--63-lightgrey?style=flat-square
[cv-The__Midnight__Lab]: https://img.shields.io/badge/The__Midnight__Lab-lightgrey?style=flat-square
[cv-VividMuse]: https://img.shields.io/badge/VividMuse-lightgrey?style=flat-square
[cv-Wimile]: https://img.shields.io/badge/Wimile-lightgrey?style=flat-square
[cv-Xabsurd]: https://img.shields.io/badge/Xabsurd-lightgrey?style=flat-square
[cv-Zarj]: https://img.shields.io/badge/Zarj-lightgrey?style=flat-square
[cv-__Envy__]: https://img.shields.io/badge/__Envy__-lightgrey?style=flat-square
[cv-aimann]: https://img.shields.io/badge/aimann-lightgrey?style=flat-square
[cv-auxilia]: https://img.shields.io/badge/auxilia-lightgrey?style=flat-square
[cv-cfgscale]: https://img.shields.io/badge/cfgscale-lightgrey?style=flat-square
[cv-chloeana]: https://img.shields.io/badge/chloeana-lightgrey?style=flat-square
[cv-coachbate]: https://img.shields.io/badge/coachbate-lightgrey?style=flat-square
[cv-dushuai]: https://img.shields.io/badge/dushuai-lightgrey?style=flat-square
[cv-eznorb]: https://img.shields.io/badge/eznorb-lightgrey?style=flat-square
[cv-fwaunstp733]: https://img.shields.io/badge/fwaunstp733-lightgrey?style=flat-square
[cv-henxpoi521]: https://img.shields.io/badge/henxpoi521-lightgrey?style=flat-square
[cv-imagC]: https://img.shields.io/badge/imagC-lightgrey?style=flat-square
[cv-itwasfake]: https://img.shields.io/badge/itwasfake-lightgrey?style=flat-square
[cv-jandordoe]: https://img.shields.io/badge/jandordoe-lightgrey?style=flat-square
[cv-jblm]: https://img.shields.io/badge/jblm-lightgrey?style=flat-square
[cv-l15625089091628]: https://img.shields.io/badge/l15625089091628-lightgrey?style=flat-square
[cv-ladaagency]: https://img.shields.io/badge/ladaagency-lightgrey?style=flat-square
[cv-latent--space--dreams]: https://img.shields.io/badge/latent--space--dreams-lightgrey?style=flat-square
[cv-lcarspicturesdatabase]: https://img.shields.io/badge/lcarspicturesdatabase-lightgrey?style=flat-square
[cv-lenML]: https://img.shields.io/badge/lenML-lightgrey?style=flat-square
[cv-makimakimakima12138]: https://img.shields.io/badge/makimakimakima12138-lightgrey?style=flat-square
[cv-mastermerlin]: https://img.shields.io/badge/mastermerlin-lightgrey?style=flat-square
[cv-mikkoph]: https://img.shields.io/badge/mikkoph-lightgrey?style=flat-square
[cv-mrkakapopoloch]: https://img.shields.io/badge/mrkakapopoloch-lightgrey?style=flat-square
[cv-nil__sphere]: https://img.shields.io/badge/nil__sphere-lightgrey?style=flat-square
[cv-qingfengy36939]: https://img.shields.io/badge/qingfengy36939-lightgrey?style=flat-square
[cv-rakamik]: https://img.shields.io/badge/rakamik-lightgrey?style=flat-square
[cv-saladin]: https://img.shields.io/badge/saladin-lightgrey?style=flat-square
[cv-sh1201]: https://img.shields.io/badge/sh1201-lightgrey?style=flat-square
[cv-spanking__cutie]: https://img.shields.io/badge/spanking__cutie-lightgrey?style=flat-square
[cv-thedivergentai]: https://img.shields.io/badge/thedivergentai-lightgrey?style=flat-square
[cv-updatemp]: https://img.shields.io/badge/updatemp-lightgrey?style=flat-square
[cv-wolfcatz]: https://img.shields.io/badge/wolfcatz-lightgrey?style=flat-square
[cv-wwwmanmoon103]: https://img.shields.io/badge/wwwmanmoon103-lightgrey?style=flat-square

<!-- HEADER BADGES -->
[hf-shield]: https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black
[hf-url]: https://huggingface.co/Qwen/Qwen-Image-2.1
[lic-shield]: https://img.shields.io/badge/model%20license-Qwen%20Research-yellow?style=for-the-badge
[lic-url]: https://huggingface.co/Qwen/Qwen-Image-2.1/blob/main/LICENSE
[commit-shield]: https://img.shields.io/badge/docs-auto--generated-blue?style=for-the-badge
[commit-url]: https://github.com/QwenLM/Qwen-Image-2.1
[prs-shield]: https://img.shields.io/badge/PRs-welcome-brightgreen?style=for-the-badge
[prs-url]: https://github.com/QwenLM/Qwen-Image-2.1/pulls

<!-- PRECISION BADGES -->
[badge-bf16]: https://img.shields.io/badge/bf16-0077cc?style=flat-square
[badge-fp32]: https://img.shields.io/badge/fp32-0077cc?style=flat-square
[badge-fp16]: https://img.shields.io/badge/fp16-0077cc?style=flat-square
[badge-f16]: https://img.shields.io/badge/F16-0077cc?style=flat-square
[badge-IQ4_XS]: https://img.shields.io/badge/IQ4__XS-17a2b8?style=flat-square
[badge-fp8]: https://img.shields.io/badge/fp8-28a745?style=flat-square
[badge-fp4]: https://img.shields.io/badge/fp4-20c997?style=flat-square
[badge-nvfp4]: https://img.shields.io/badge/nvfp4-6f42c1?style=flat-square
[badge-mxfp4]: https://img.shields.io/badge/mxfp4-6f42c1?style=flat-square
[badge-int8]: https://img.shields.io/badge/int8-17a2b8?style=flat-square
[badge-int4]: https://img.shields.io/badge/int4-ffc107?style=flat-square
[badge-w4a8]: https://img.shields.io/badge/w4a8-fe7d37?style=flat-square
[badge-w8a8]: https://img.shields.io/badge/w8a8-fe7d37?style=flat-square
[badge-exl3-5bit]: https://img.shields.io/badge/EXL3%205bit-6f42c1?style=flat-square
[badge-exl3-4bit]: https://img.shields.io/badge/EXL3%204bit-6f42c1?style=flat-square
[badge-exl3-3bit]: https://img.shields.io/badge/EXL3%203bit-6f42c1?style=flat-square
[badge-Q2_K]: https://img.shields.io/badge/Q2__K-e05d44?style=flat-square
[badge-q2k]: https://img.shields.io/badge/Q2__K-e05d44?style=flat-square
[badge-Q3_K_M]: https://img.shields.io/badge/Q3__K__M-fe7d37?style=flat-square
[badge-q3km]: https://img.shields.io/badge/Q3__K__M-fe7d37?style=flat-square
[badge-Q4_K]: https://img.shields.io/badge/Q4__K-dfb317?style=flat-square
[badge-Q4_K_M]: https://img.shields.io/badge/Q4__K__M-dfb317?style=flat-square
[badge-q4km]: https://img.shields.io/badge/Q4__K__M-dfb317?style=flat-square
[badge-Q4_K_S]: https://img.shields.io/badge/Q4__K__S-dfb317?style=flat-square
[badge-Q5_0]: https://img.shields.io/badge/Q5__0-97c00f?style=flat-square
[badge-Q5_K_M]: https://img.shields.io/badge/Q5__K__M-97c00f?style=flat-square
[badge-q5km]: https://img.shields.io/badge/Q5__K__M-97c00f?style=flat-square
[badge-Q6_K]: https://img.shields.io/badge/Q6__K-0077cc?style=flat-square
[badge-q6k]: https://img.shields.io/badge/Q6__K-0077cc?style=flat-square
[badge-Q8_0]: https://img.shields.io/badge/Q8__0-28a745?style=flat-square
[badge-q8]: https://img.shields.io/badge/Q8__0-28a745?style=flat-square

<!-- TASK BADGES -->
[task-t2i]: https://img.shields.io/badge/text%20%E2%86%92%20image-0077cc?style=flat-square
[task-i2i]: https://img.shields.io/badge/image%20%E2%86%92%20image-6f42c1?style=flat-square
[task-both]: https://img.shields.io/badge/both-17a2b8?style=flat-square

<!-- TYPE BADGES -->
[ltype-fix]: https://img.shields.io/badge/Fix-0077cc?style=flat-square
[ltype-style]: https://img.shields.io/badge/Style-6f42c1?style=flat-square
[ltype-control]: https://img.shields.io/badge/Control-17a2b8?style=flat-square
[ltype-utility]: https://img.shields.io/badge/Utility-28a745?style=flat-square
[ltype-nsfw]: https://img.shields.io/badge/NSFW-b02a37?style=flat-square
[ltype-training]: https://img.shields.io/badge/Training-6f42c1?style=flat-square
[ltype-notebook]: https://img.shields.io/badge/Notebook-0077cc?style=flat-square
[ltype-skill]: https://img.shields.io/badge/Agent%20Skill-fe7d37?style=flat-square
[ltype-port]: https://img.shields.io/badge/Port-17a2b8?style=flat-square
[ltype-nodes]: https://img.shields.io/badge/Node%20pack-fe7d37?style=flat-square
[ltype-pe]: https://img.shields.io/badge/PE-6f42c1?style=flat-square
[ltype-te]: https://img.shields.io/badge/TE-17a2b8?style=flat-square
