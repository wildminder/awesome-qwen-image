# Awesome Qwen-Image 2.1

A curated list of checkpoints, quants, prompt engines, LoRAs, and tooling for **Qwen-Image 2.1** — Alibaba's 7B unified text-to-image and image-editing model.

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
| Generate in 4 steps | **[Viggle Turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** | Official turbo distill + LoRA variants |
| On a Mac (Apple Silicon) | **[MLX-4bit](https://huggingface.co/themindstudio/Qwen-Image-2.1-MLX-4bit)** | MLX 4-bit, native unified memory |
| Improve prompt quality | **[PE-T2I](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I)** | Official prompt rewriter + aspect-ratio picker |

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

| Name | Precision | Layout | DiT | Text encoder | VAE | Links |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| **Qwen-Image-2.1** | ![bf16][badge-bf16] | diffusers | 14.23 GB | 17.53 GB (Qwen3-VL-8B) | 1.35 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1) |

The reference release, and the only repo you need for a standard Diffusers setup. The DiT and text encoder ship as numbered safetensors shards with an index, so pull the repo rather than a single file.

<p id="official-comfy" align="center">· · · · · · · · · · · · · ·</p>

### ▣ ComfyUI official

**[Comfy-Org](https://huggingface.co/Comfy-Org/Qwen-Image-2.1)** — the Day-0 ComfyUI repackage. Files land directly in `models/` with no renaming.

| Name | Precision | Size | Links |
| :--- | :---: | :---: | :---: |
| **Image Model** | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_bf16.safetensors) |
| **Image Model** | ![int8][badge-int8] | 7.26 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/diffusion_models/qwen_image_2.1_int8_convrot.safetensors) |
| **Text Encoder** | ![bf16][badge-bf16] | 17.53 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_bf16.safetensors) |
| **Text Encoder** | ![int8][badge-int8] | 9.35 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_int8_convrot.safetensors) |
| **Text Encoder** | ![w4a8][badge-w4a8] | 6.31 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3vl_8b_w4a8.safetensors) |
| **Prompt Engine T2I** | ![int8][badge-int8] | 9.47 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3.5_9b_qwen_image_2.1_pe_t2i.int8_convrot.safetensors) |
| **Prompt Engine I2I** | ![int8][badge-int8] | 9.47 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/text_encoders/qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors) |
| **VAE** | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Comfy--Org]](https://huggingface.co/Comfy-Org/Qwen-Image-2.1/resolve/main/vae/qwen_image_2.1_vae_bf16.safetensors) |

> [!TIP]
> `ConvRot` files are ComfyUI's native rotated-channel integer format. Use a recent ComfyUI build and load them with the standard diffusion-model and text-encoder loaders — no custom nodes required.
>
> Destination folders: **Image Model** → `models/diffusion_models/`, **Text Encoder** and both **Prompt Engine** rows → `models/text_encoders/`, **VAE** → `models/vae/`.

<p id="encoders" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚲ Text encoders &amp; prompt engines

Qwen-Image 2.1 conditions on **Qwen3-VL-8B**, which at BF16 is the largest file in the stack. It also ships dedicated **prompt-rewriting** models — separate text models that take a short request in any language and return a detailed English prompt plus a recommended aspect ratio. Neither is part of the diffusion path; both are optional, and you can run the base model without either.

<p id="pe" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Prompt rewriters

The official pair is fine-tuned Qwen3.5-VL 9B. The **Pocket** builds are fine-tuned from Qwen3.5 base models instead — roughly 5× smaller, text-to-image only.

| Name | Task | Params | Precision | Size | Links |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **PE-T2I** | ![text → image][task-t2i] | 9B | ![bf16][badge-bf16] | 18.82 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-T2I) |
| **PE-I2I** | ![image → image][task-i2i] | 9B | ![bf16][badge-bf16] | 18.82 GB | [![][gh-Qwen]](https://huggingface.co/Qwen/Qwen-Image-2.1-PE-I2I) |
| **PE-T2I Pocket 2B** | ![text → image][task-t2i] | 2B | ![bf16][badge-bf16] | 3.76 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-2B) |
| **PE-T2I Pocket 0.8B** | ![text → image][task-t2i] | 0.8B | ![Q8_0][badge-Q8_0] | 1.50 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-PE-T2I-Pocket-0.8B) |

The official rewriters ship a `system_prompt.txt` and work with `AutoModelForCausalLM` + `AutoTokenizer`. Output is JSON after a reasoning block, so split on the think-tag before parsing. The T2I rewriter also returns a recommended `wh_ratio` you can map straight to a size.

Each official repo splits into four `model-0000N.safetensors` shards plus a `model.safetensors.index.json`, so download the repo — there is no single-file build to link to. The 0.8B Pocket build also ships a `Q8_0` GGUF (0.81 GB) for llama.cpp.

<p id="heretic" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Heretic &amp; abliterated

> [!WARNING]
> These repos have the safety refusal direction **abliterated**. The model no longer refuses prompt content, which means it will happily rewrite anything you ask. Same Qwen Research License as the base weights — the license does not grant you additional rights.
>
> Applied to both halves of the text stack: the **text encoder** (the conditioning signal itself no longer refuses) and the **prompt rewriter**.

| Type | Name | Task | Precision | Size | Links |
| :---: | :--- | :---: | :---: | :---: | :---: |
| ![TE][ltype-te] | **Heretic TE GGUF** | | ![Q4_K_M][badge-Q4_K_M] ![fp8][badge-fp8] ![bf16][badge-bf16] | 33.07 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-Text-Encoder-Heretic-GGUF/resolve/main/qwen3vl_8b_heretic-Q4_K_M.gguf) |
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

The INT8 ConvRot pack covers both PE-T2I and PE-I2I in one download. The heretic int8 ConvRot pair does too, and unlike the tensorwise build it keeps the MTP head — 1,395 tensors against 829, one file per task at 9.96 GB.

<p id="quant" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◈ Quantizations

Community conversions of the base DiT. Sizes are total weight bytes per repo. Distilled turbo GGUFs live under [Turbo](#turbo) instead.

<p id="quant-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ GGUF

Transformer-only weights for llama.cpp, sorted from the highest quant down. **Q4_K_M** is the usual quality/size balance point. **[Unsloth](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** is the primary source — it carries the widest ladder, so it wins every quant it ships. Where two repos offer a quant Unsloth does not, both are linked in the same cell.

| Quant | Size | Download |
| :---: | :---: | :---: |
| ![F16][badge-bf16] | 14.23 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-F16.gguf) |
| ![BF16][badge-bf16] | 14.23 GB | [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF/resolve/main/qwen-image-2.1-UC-BF16.gguf) |
| ![Q8_0][badge-q8] | 7.64 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q8_0.gguf) |
| ![Q6_K_XL][badge-q6k] | 6.72 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q6_K_XL.gguf) |
| ![Q6_K][badge-q6k] | 6.27 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q6_K.gguf) |
| ![Q5_K_M][badge-q5km] | 5.39 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q5_K_M.gguf) |
| ![Q5_K_S][badge-q5km] | 4.50 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q5_K_S.gguf) |
| ![Q4 · HVQ3 (sd.cpp)][badge-q4km] | 5.96 GB | [![][gh-zcf0508]](https://huggingface.co/zcf0508/qwen-image-2.1-hqv3-sdcpp-fixed/resolve/main/Qwen-Image-2.1-Q4-sd.cpp.gguf) |
| ![NVFP4][badge-nvfp4] | 4.05 GB | [![][gh-gguf-org]](https://huggingface.co/gguf-org/qwen-image-2.1-gguf/resolve/main/qwen-image-2.1-nvfp4.gguf) ┊ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF/resolve/main/qwen-image-2.1-UC-NVFP4.gguf) |
| ![Q4_K_M][badge-q4km] | 4.20 GB | [![][gh-unsloth]](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF/resolve/main/qwen-image-2.1-Q4_K_M.gguf) |
| ![Q4_0][badge-q4km] | 4.15 GB | [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF/resolve/main/qwen-image-2.1-UC-Q4_0.gguf) ┊ [![][gh-ped4enko]](https://huggingface.co/ped4enko/Qwen-Image-2.1-Dessi/resolve/main/qwen-image-2.1-Q4_0.gguf) |
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
| **INT8** | ![int8][badge-int8] | 17.96 GB | [![][gh-Rin247]](https://huggingface.co/Rin247/Qwen-Image-2.1-INT8) | Full diffusers repo. |
| **DiT NVFP4 ComfyUI** | ![nvfp4][badge-nvfp4] | 13.82 GB | [![][gh-pottokao]](https://huggingface.co/pottokao/Qwen-Image-2.1-DiT-NVFP4-ComfyUI/resolve/main/qwen_image_2.1_nvfp4.safetensors) | Three ComfyUI cuts: `nvfp4`, `nvfp4_T2`, `nvfp4_T3`. |
| **FP4** | ![fp4][badge-fp4] | 11.74 GB | [![][gh-Rin247]](https://huggingface.co/Rin247/Qwen-Image-2.1-FP4/resolve/main/transformer/diffusion_pytorch_model.safetensors) | DiT 6.46 + TE 4.94 + VAE 0.34. |
| **INT4** | ![int4][badge-int4] | 11.08 GB | [![][gh-Rin247]](https://huggingface.co/Rin247/Qwen-Image-2.1-INT4) | Full diffusers repo. |
| **bnb 4bit** | ![int4][badge-int4] | 11.41 GB | [![][gh-circulus]](https://huggingface.co/circulus/Qwen-Image-2.1-bnb-4bit) | Standard bitsandbytes NF4. |
| **MXFP4 (Paiton)** | ![mxfp4][badge-mxfp4] | 9.31 GB | [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4) | Paiton backend, 57 shards. |
| **MXFP4 Paiton RDNA4** | ![mxfp4][badge-mxfp4] | 9.31 GB | [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-MXFP4-Paiton-RDNA4) | RDNA4-specific kernel variant, identical layout. |
| **Uncensored MXFP4 Paiton** | ![mxfp4][badge-mxfp4] | 9.31 GB | ⚠️ [![][gh-EliovpAI]](https://huggingface.co/EliovpAI/Qwen_Image-2.1-Uncensored-MXFP4-Paiton) | Uncensored, derived from the abenzerps GGUF. |
| **W4A4 NVFP4** | ![nvfp4][badge-nvfp4] | 4.88 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4) | DiT only; near-identical to the INT4 build. |
| **W4A4 INT4** | ![int4][badge-int4] | 4.66 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4) | DiT only. |

<p id="quant-fp8" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; bf16

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Darkstar ModelOpt FP8** | ![fp8][badge-fp8] | 26.33 GB | [![][gh-HangGlidersRule]](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8) | NVIDIA ModelOpt-derived, full repo. |
| **FP8** | ![fp8][badge-fp8] | 17.96 GB | [![][gh-Rin247]](https://huggingface.co/Rin247/Qwen-Image-2.1-FP8) | Closest thing to a drop-in smaller BF16. |
| **Uncensored BF16 SafeTensor** | ![bf16][badge-bf16] | 14.23 GB | ⚠️ [![][gh-dh123456789123]](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor/resolve/main/qwen-image-2.1-UC-BF16_bf16.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet/resolve/main/Qwen-Image-2.1-_bf16%E6%97%A0%E5%AE%A1%E6%9F%A5.safetensors) | Single 14.23 GB file — the full DiT in one piece, mirrored by both repos. |
| **DF11 ComfyUI** | ![bf16][badge-bf16] | 9.72 GB | [![][gh-mingyi456]](https://huggingface.co/mingyi456/Qwen-Image-2.1-DF11-ComfyUI/resolve/main/qwen_image_2.1_bf16-DF11.safetensors) | `qwen_image_2.1_bf16-DF11.safetensors`. |

<p id="quant-nunchaku" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Nunchaku (SVDQ)

SVDQ-based 4-bit for Nunchaku, which targets low-VRAM systems and 4090-class cards.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **nunchaku** | ![fp4][badge-fp4] | 8.75 GB | [![][gh-catplusplus]](https://huggingface.co/catplusplus/nunchaku-qwen-image-2.1/resolve/main/best_quality_fp4.safetensors) | Two builds: `best_quality_fp4` and `svdq-fp4_r32`. |
| **nunchaku lite int4** | ![int4][badge-int4] | 4.15 GB | [![][gh-BlazeMCworld]](https://huggingface.co/BlazeMCworld/Qwen-Image-2.1-nunchaku-lite-int4) | Half the size of the FP4 build. |

<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▸ Turbo &amp; step distillation

Few-step students of the base model, distilled with Distribution Matching Distillation. They trade fidelity for speed: useful for iteration and batch work, weaker than 40 steps on multi-reference composition and identity-preserving edits.

**Current Viggle release: `v0.2.1` (2026-09-24), 6 steps.** It supersedes `v0.2` (5-step name, sampled at 6) and `v0.1` (4 steps). Sample with `sigmas=[1.0, 0.9375, 0.875, 0.75, 0.5, 0.25]`, CFG 1.0, empty negative prompt. Official source: **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)**.

Three independent distillation lines are listed below, and their checkpoints are **not interchangeable** — pick one family. **Viggle** is the reference DMD line above. **Alibaba PAI** ships an official 4-step line via Parallel Decoding Distillation (PDD) in VideoX-Fun. **[Pruna](https://huggingface.co/Pruna-Qwen-Image-2.1)** is a third-party DMD line with 8-step and 5-step adapters.

The Pruna adapters are strict about sampling. Each has its own sigma schedule and they are **not** interchangeable, so load exactly one: 8-step wants `sigmas=[1.0, 14/15, 6/7, 10/13, 2/3, 6/11, 0.4, 2/9]`, 5-step wants `sigmas=[1.0, 0.94, 6/7, 2/3, 0.4]`. Both need `shift=1.0` with dynamic shifting **off** (otherwise the sigmas get shifted twice), no CFG, no negative prompt, and LoRA strength 1.0. Trained at 1K with up to 3 reference images; 2K runs but sits outside training coverage. Requires a pinned diffusers commit.

<p id="turbo-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Turbo GGUF

DiT-only quants for ComfyUI-GGUF, from the **v0.1 full fine-tune** (4-step). Both repos keep FP32 attention norms and BF16 patch embeddings, so do **not** stack a Viggle LoRA on top. Repos: **[realrebelai](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs)** (wider ladder) and **[Abiray](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF)** (adds `Q4_K_S`). Where both ship a quant, both are linked; sizes differ because the builds differ.

| Quant | Size | Download |
| :---: | :---: | :---: |
| ![Q8_0][badge-Q8_0] | 7.69 / 7.59 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q8_0.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q8_0.gguf) |
| ![Q6_K][badge-Q6_K] | 6.91 / 5.88 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q6_K.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q6_K.gguf) |
| ![Q5_K_M][badge-Q5_K_M] | 5.96 / 5.01 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q5_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q5_K_M.gguf) |
| ![Q4_K_M][badge-Q4_K_M] | 5.56 / 4.19 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q4_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_M.gguf) |
| ![Q4_K_S][badge-Q4_K_S] | 4.06 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q4_K_S.gguf) |
| ![Q3_K_M][badge-Q3_K_M] | 4.19 / 3.19 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q3_K_M.gguf) ┊ [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF/resolve/main/qwen_image_2.1_turbo_Q3_K_M.gguf) |
| ![Q2_K][badge-Q2_K] | 3.77 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs/resolve/main/Qwen-Image-2.1-viggle-turbo-Q2_K.gguf) |

<p id="turbo-models" align="center">· · · · · · · · · · · · · ·</p>

### ▣ Official &amp; converted

Repo links: **[Viggle](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** · **[alibaba-pai](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Acc-LoRAs)** · **[Pruna](https://huggingface.co/PrunaAI/Pruna-Qwen-Image-2.1)** · **[chfm](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo)** (v0.2 snapshot) · **[t8star](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy)** · **[xingewh](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy)** · **[RunningHubAI](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora)** · **[addlabsviral](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-bf16)** · **[cgb](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX)**

Two ComfyUI conversions of the v0.2.1 adapters exist and are not interchangeable. **[xingewh](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy)** re-keys the weights for ComfyUI and loads with the stock `Load LoRA` node. **[t8star](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy)** fuses gate/up for ComfyUI's merged MLP, which is why its files are larger; load those with the built-in `LoraLoaderBypassModelOnly` at strength 1.0 on a **BF16** base, because ordinary merging loaders lose adapter updates to rounding.

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **v0.2.1 LoRA r256** | 6 | ![bf16][badge-bf16] | 1.36 / 1.36 / 1.76 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r256.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r256_comfy.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/qwen_image_2.1_viggle_turbo_v0.2.1_r256_comfy.safetensors) | **Start here.** Sharpest and most faithful to the 40-step base; runs the demo Space. Load on the base transformer at runtime — do not merge. |
| **v0.2.1 LoRA r128** | 6 | ![bf16][badge-bf16] | 0.68 / 0.68 / 0.88 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128_comfy.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/qwen_image_2.1_viggle_turbo_v0.2.1_r128_comfy.safetensors) | Same adapter cut to rank 128; what the shipped ComfyUI workflows use. |
| **v0.2.1 adapter (peft)** | 6 | ![fp32][badge-fp32] | 2.72 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/peft_v0.2.1) | The v0.2.1 LoRA in PEFT key format, F32 as trained. |
| **v0.2 LoRA r256** | 5 / 6 | ![bf16][badge-bf16] | 1.36 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) ┊ [![][gh-chfm]](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) ┊ [![][gh-xingewh]](https://huggingface.co/xingewh/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256_comfy.safetensors) | Step 600 of the same run. The `5step` in the name is the launch schedule; sample at 6 like v0.2.1. |
| **v0.2 LoRA r128** | 5 / 6 | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r128.safetensors) | Rank-128 cut of v0.2. |
| **v0.1 full fine-tune** | 4 | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/transformer) | Merged transformer, no LoRA needed. This is what the GGUF quants above were built from. |
| **v0.1 LoRA r64** | 4 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-lora-r64.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) | Superseded — diversity collapsed to 0.75× base and the output was visibly softer. Kept for reproducibility. |
| **Fun-Acc 4Step (PDD)** | 4 | ![bf16][badge-bf16] | 0.35 GB | [![][gh-alibaba--pai]](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Acc-LoRAs/resolve/main/models/Qwen-Image-2.1-Fun-Acc-4Step.safetensors) | **Official Alibaba PAI line**, rank 64, via Parallel Decoding Distillation. Independent of Viggle — 4 NFE for both T2I and instruction editing. | 
| **Pruna 8Step** | 8 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-PrunaAI]](https://huggingface.co/PrunaAI/Pruna-Qwen-Image-2.1/resolve/main/p_qwen_image_2.1_8step_v0.1.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Pruna-Qwen-Image-2.1-Comfy/resolve/main/p_qwen_image_2.1_8step_v0.1-comfyui-T8.safetensors) | Third-party DMD line, rank 64 / alpha 128. Better of the two and the card's default, but v0.1 is marked work in progress. |
| **Pruna 5Step** | 5 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-PrunaAI]](https://huggingface.co/PrunaAI/Pruna-Qwen-Image-2.1/resolve/main/p_qwen_image_2.1_5step_v0.1.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Pruna-Qwen-Image-2.1-Comfy/resolve/main/p_qwen_image_2.1_5step_v0.1-comfyui-T8.safetensors) | Same line, faster and visibly weaker. One adapter at a time — see the schedule note above. |
| **UltraFast 8Step** | 8 | ![bf16][badge-bf16] | 0.17 GB | [![][gh-Haverbex]](https://huggingface.co/Haverbex/Qwen-Image-2.1-UltraFast/tree/main/adapter) | Editing-focused rather than T2I, trained on MagicBrush. Rank 32, but loaded by the repo's own `inject_dit_lora` script, **not** a stock PEFT or ComfyUI loader. |
| **Turbo BF16 diffusers** | 4 | ![bf16][badge-bf16] | 32.44 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-bf16) | Full pipeline (TE + DiT + VAE), ready to load with `QwenImage21Pipeline`. |
| **Turbo FP4 diffusers** | 4 | ![fp4][badge-fp4] | 11.41 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-fp4) | Same v0.1 pipeline with an FP4 DiT; the TE is the larger half at 6.73 GB. |
| **Turbo ONNX (browser)** | 4 | ![int4][badge-int4] | ~17.2 GB | [![][gh-cgb]](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX) | r64 LoRA merged into the denoiser, then Q4 MatMulNBits. WebGPU in-browser; needs the FreeGen pipeline and a desktop adapter. Experimental. |
| **UltraFast Q4_K** | 8 | ![Q4_K][badge-Q4_K] | 4.05 GB | [![][gh-Haverbex]](https://huggingface.co/Haverbex/Qwen-Image-2.1-UltraFast-GGUF/resolve/main/Qwen-Image-2.1-UltraFast-Q4_K.gguf) | DiT-only Q4_K with the UltraFast adapter **already merged** — do not stack the adapter on top. Still needs the text/vision encoders and the matching VAE. Quality and speed unbenchmarked. |
| **Pruna 8Step SDNQ** | 8 | ![int4][badge-int4] | 11.51 GB | [![][gh-SamuelTallet]](https://huggingface.co/SamuelTallet/Pruna-Qwen-Image-2.1-8steps-SDNQ-4bit-dynamic-hadamard256) | The Pruna 8-step adapter **merged** into the base and re-quantized with [SDNQ](https://github.com/Disty0/sdnq) — UINT4 dynamic, Hadamard rotation at group size 256. Full diffusers pipeline (DiT 4.10 + TE 6.74 + VAE 0.68), so the text encoder is quantized too. Needs SDNQ 0.2.0+, Triton and diffusers from git. Follows the Pruna sigma schedule above. |

<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, control, and fix adapters. All target the base DiT unless noted. Grouped by type, then by size.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **ControlNet-Union** | ![Control][ltype-control] | ![bf16][badge-bf16] | 7.55 GB | [![][gh-alibaba--pai]](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Controlnet-Union/resolve/main/Qwen-Image-2.1-Fun-Controlnet-Union.safetensors) | **Official Alibaba PAI / VideoX-Fun.** One checkpoint for 8 conditions (Canny, Depth, Grayscale, HED, Lineart, MLSD, Pose, Scribble) plus inpainting. Control branch only, 16 injection points, loaded `strict=False`. | 
| **Object Mover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview) | Bbox object *moving*, 6 checkpoints. |
| **Object Remover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview) | Bbox object removal, full-quality variant. |
| **Object Remover Bbox turbo** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo) | 4-step-compatible variant. |
| **BFS Head Swap v1** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.32 GB | [![][gh-Alissonerdx]](https://huggingface.co/Alissonerdx/BFS-Best-Face-Swap/resolve/main/bfs_head_v1_qwen_2.1.safetensors) | Head replacement, **not** a whole-face blend: identity, hair, eye colour and nose come from image 2 while gaze direction, head rotation and expression stay with image 1. Rank 64, 5,000 steps, MIT. **Image order is load-bearing** — swapping the two inputs swaps who is retargeted. Trigger with the `head_swap:` prefix. The repo's other 17 adapters target Qwen Image Edit 2509/2511, Flux 2 Klein, Krea 2 and LTX-2, not this base. |
| **Orbit Alpha** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-viewpoint-orbit-LoRA/resolve/main/checkpoints/steps2000res768/orbit_alpha_lora_gate_up_split.safetensors) | **Official ML-Intern-lab.** One RGBA image in, the same object from a new viewpoint out. Rank 32, 2,000 steps at 768 px. Use the `_gate_up_split` file — the other checkpoint in the repo does not load correctly. `<orbit>` grammar, 40 steps, no CFG. |
| **Outpaint v2** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Outpaint-LoRA/resolve/main/qwen-image-2.1-outpaint-v2.safetensors) | Pad the picture with flat `#808080`, hand the padded canvas to the model as the reference; the adapter fills the gray and keeps the original pixel-registered. One side, a corner, or all four. Rank 32, ComfyUI keys, 2,000 steps. 25 steps, CFG 1, `resolution` 0, target 1–2 MP. Do **not** pin the known area with a latent noise mask — on this model it draws a visible rectangle at the seam. |
| **Outpaint v1** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.16 GB | [![][gh-ausboss]](https://huggingface.co/ausboss/Qwen-Image-2.1-Outpaint-LoRA/resolve/main/qwen-image-2.1-outpaint.safetensors) | Same adapter one step earlier, trained at ≤1 MP over more extreme zoom-outs. The better of the two on very large extensions; the two are within noise on ordinary crops. The repo also keeps the step-500 and step-1250 intermediates. |
|  |  |  |  |  |  |
| **Fix** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.11 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix/resolve/main/qwen-image-2.1-fix-1.0-comfy.safetensors) | The most-liked community LoRA. |
|  |  |  |  |  |  |
| **De-AI LoRA pack** | ![Style][ltype-style] | ![bf16][badge-bf16] | 2.45 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora) | 8-file "remove the AI look" pack (CN filenames). |
| **Natural Exposure LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA) | Exposure correction, 5 checkpoints. |
| **De-AI + lighting v5** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora-2071763057486946305/resolve/main/Qwen%20Image%202.1%E5%8E%BB%E9%99%A4ai%2B%E5%85%89%E5%BD%B1%E4%BC%98%E5%8C%96v5.safetensors) | Two revisions of the same de-AI + lighting adapter; v5 is the newer. The repo also has two lighting-only adapters, 0.24 and 0.09 GB. CN filenames. |
| **Sts2 Cards Drawer** | ![Style][ltype-style] | ![fp16][badge-fp16] | 0.10 GB | [![][gh-Airmongsity]](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer) | `deckbuilder_cardart_style_lora_v1_fp16`. |
|  |  |  |  |  |  |
| **Normal2RGB** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-Aero--Ex]](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB/resolve/main/Normal2RGB_4000.safetensors) | Normal map → RGB render, 3 checkpoints. |
|  |  |  |  |  |  |
| **Hips / buttocks** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.47 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora/resolve/main/qwen2509%E8%87%80%E9%83%A8%E6%94%BE%E5%A4%A7.safetensors) | Body-shape slider, ported from a Qwen-2.5-09 derivative. CN filename. |
| **Breasts and hips** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora/resolve/main/Qwen%20Image%202.1%E5%A4%A7%E8%83%B8%E5%A4%A7%E8%87%80.safetensors) | Combined body-shape adapter from the same pack. CN filename. |
| **RadianceChrome Voluptuous** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-AIImageStudio]](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0) | Character-style LoRA. |
| **NSFW LoRA** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.16 GB | ⚠️ [![][gh-Wickedlizerd]](https://huggingface.co/Wickedlizerd/NSFW-Qwen-Image-2.1-LoRA/resolve/main/nsfw_qwen_21.safetensors) | Civitai original by TheseAlpacas, mirrored unmodified. Rank 32, 192 targets. The author asks for **25+ steps**, `er_sde` and the `beta` scheduler on an INT8 ConvRot base — it is not a few-step adapter. |
| **NSFW Image Edit** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.08 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora/resolve/main/Qwen-Image-2.1%20NSFW%20Image%20Edit.safetensors) | Editing LoRA, uploaded 2026-09-24. |
| **Breasts Slider V1** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.003 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora/resolve/main/Pornmaster_QI2.1_Breasts_Slider_V1.safetensors) | Slider control, 3 MB. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used — the uncensored GGUF in particular is the most-downloaded repo in this ecosystem. They carry the same **Qwen Research License** as the base model; a research license is not a license to do whatever you want, and you remain responsible for how you use them. Sizes are the LoRA file only; none of these merge a base model.

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
| **Uncensored MLX** | MLX (4/6/8-bit) | ![int4][badge-int4] ![int8][badge-int8] | 7.56 GB | ⚠️ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-GGUF) | The uncensored line's Apple build, from the GGUF repo. **DiT only** — 7.56 GB at 8-bit, 5.78 at 6-bit, 4.00 at 4-bit, and the repo ships no MLX text encoder or config, so pair it with one of the two repos above. |

The repo is self-contained: alongside the two packs it now ships the Qwen3-VL-8B text encoder (4 shards, 17.53 GB), the VAE (1.35 GB) and the processor files, so the support download is no longer needed. Needs macOS 14+. 1024×1024 works; 2048×2048 does not yet.

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

**Intel Arc (SYCL).** **[Frosty40](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF)** is a serving package for the `sd.cpp` SYCL backend, not a fine-tune — "turbo" refers to the recipe, not to step distillation. Despite the name the weights are Qwen's own, quantized from a BF16 master.

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **SYCL quality** | ![q5_0][badge-Q5_0] | 7.89 GB | [![][gh-Frosty40]](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF) | Attention qkv/o kept at F16; 73 BF16 + 128 F16 + 96 Q5_0. |
| **SYCL lean** | ![q5_0][badge-Q5_0] | 5.07 GB | [![][gh-Frosty40]](https://huggingface.co/Frosty40/Qwen-Image-2.1-SYCL-Turbo-GGUF) | All-exception Q5_0, 73 BF16 + 224 Q5_0. |

Both ship with a pinned Qwen3-VL-8B-Instruct Q8_0 text encoder (8.71 GB) and the BF16 VAE (0.68 GB). Two non-obvious requirements: `--vae-tiling` is mandatory at 1024² or the SYCL VAE overflows int32, and **one `sd-cli` per GPU** — a second concurrent run wedges the xe driver and needs a root-only reset. The 3.84× speedup comes from `--eager-load --cache-mode easycache`, not from fewer steps.

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

<p id="tools" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚙ Tools &amp; notebooks

| Name | Type | Links | Notes |
| :--- | :---: | :---: | :--- |
| **training assistant v1** | ![Training][ltype-training] | [![][gh-SimpleTuner]](https://huggingface.co/SimpleTuner/Qwen-Image-2.1-training-assistant-v1) | A working `pytorch_lora_weights.safetensors` + `training_details.json` you can inspect to learn a real SimpleTuner config. |
| **qwen_image2.1_colab** | ![Notebook][ltype-notebook] | [![][gh-bluemorpholimited]](https://huggingface.co/bluemorpholimited/qwen_image2.1_colab) | 7-cell notebook with form UI and `enable_model_cpu_offload()`. Works on L4 22 GB; **T4 16 GB will OOM by design**. |
| **qwen_image2.1_molab** | ![Notebook][ltype-notebook] | [![][gh-bluemorpholimited]](https://huggingface.co/bluemorpholimited/qwen_image2.1_molab) | Script version of the above, tuned for Marimo and Blackwell. |
| **Qwen-Image-2.1-Skills** | ![Agent skill][ltype-skill] | [![][gh-iamvts]](https://huggingface.co/iamvts/Qwen-Image-2.1-Skills) | Turns a short scene into a structured prompt for believable casual phone photography. 22 example images. |
| **qwen-image-2.1-p150** | ![Port][ltype-port] | [![][gh-changh95]](https://huggingface.co/changh95/qwen-image-2.1-p150) | Tenstorrent Blackhole p150a. `tt-model pull --with-weights` then `tt-model serve`. |

**Gotchas that will save you an afternoon**

1. `QwenImage21Pipeline` requires diffusers **`main`** — release `0.40.0` does not have it. Install from git.
2. The CFG kwarg is **`true_cfg_scale`**, not `guidance_scale`, and it needs a `negative_prompt` to engage.
3. Width and height must be **multiples of 32**, max edge 2048.
4. With CPU offload, the generator should live on **`'cpu'`**.
5. The default sample is **40 steps with no CFG**. Adding CFG changes the look; don't assume more steps help.
6. Chinese prompts work natively — the PE models are what rewrite them into English.

---

<p id="contributing" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ✎ Contributing

This list is a **set of Markdown files, not one**. `README.md` is generated and
should never be edited directly — your change will be overwritten on the next
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

The file prefix controls the order — the build concatenates `blocks/*.md` in
filename order, so `10_` always lands before `20_`. A new section just needs a
new number.

<div align="center">

**Awesome Qwen-Image 2.1**

</div>

<!-- OWNER BADGES -->
[gh-gguf-org]: https://img.shields.io/badge/gguf-org-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-471Def]: https://img.shields.io/badge/471Def-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-AIImageStudio]: https://img.shields.io/badge/AIImageStudio-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Alissonerdx]: https://img.shields.io/badge/Alissonerdx-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Abiray]: https://img.shields.io/badge/Abiray-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Aero--Ex]: https://img.shields.io/badge/Aero--Ex-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-addlabsviral]: https://img.shields.io/badge/addlabsviral-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-alibaba--pai]: https://img.shields.io/badge/alibaba--pai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Airmongsity]: https://img.shields.io/badge/Airmongsity-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-BlazeMCworld]: https://img.shields.io/badge/BlazeMCworld-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Comfy--Org]: https://img.shields.io/badge/Comfy--Org-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-EliovpAI]: https://img.shields.io/badge/EliovpAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-FlagRelease]: https://img.shields.io/badge/FlagRelease-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Frosty40]: https://img.shields.io/badge/Frosty40-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-HangGlidersRule]: https://img.shields.io/badge/HangGlidersRule-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-HarleyWang]: https://img.shields.io/badge/HarleyWang-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Haverbex]: https://img.shields.io/badge/Haverbex-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-JoyFusionAI]: https://img.shields.io/badge/JoyFusionAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Karsus1997]: https://img.shields.io/badge/Karsus1997-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ML--Intern--lab]: https://img.shields.io/badge/ML--Intern--lab-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ModelsLab]: https://img.shields.io/badge/ModelsLab-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Qwen]: https://img.shields.io/badge/Qwen-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Rin247]: https://img.shields.io/badge/Rin247-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-RunningHubAI]: https://img.shields.io/badge/RunningHubAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-SamuelTallet]: https://img.shields.io/badge/SamuelTallet-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-SimpleTuner]: https://img.shields.io/badge/SimpleTuner-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Viggle]: https://img.shields.io/badge/Viggle-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-abenzerps]: https://img.shields.io/badge/abenzerps-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ausboss]: https://img.shields.io/badge/ausboss-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-base11231]: https://img.shields.io/badge/base11231-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-bluemorpholimited]: https://img.shields.io/badge/bluemorpholimited-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-catplusplus]: https://img.shields.io/badge/catplusplus-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-cgb]: https://img.shields.io/badge/cgb-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-changh95]: https://img.shields.io/badge/changh95-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-chfm]: https://img.shields.io/badge/chfm-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-circulus]: https://img.shields.io/badge/circulus-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-darrellbest]: https://img.shields.io/badge/darrellbest-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-devin--lai]: https://img.shields.io/badge/devin--lai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-dh123456789123]: https://img.shields.io/badge/dh123456789123-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-diffnamehard]: https://img.shields.io/badge/diffnamehard-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-e--n--v--y]: https://img.shields.io/badge/e--n--v--y-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-evankuo]: https://img.shields.io/badge/evankuo-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-foofifoo]: https://img.shields.io/badge/foofifoo-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-iamvts]: https://img.shields.io/badge/iamvts-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-kkxao]: https://img.shields.io/badge/kkxao-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ling0322]: https://img.shields.io/badge/ling0322-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-mingyi456]: https://img.shields.io/badge/mingyi456-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-netdur]: https://img.shields.io/badge/netdur-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-netrunner--exe]: https://img.shields.io/badge/netrunner--exe-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-ped4enko]: https://img.shields.io/badge/ped4enko-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-pottokao]: https://img.shields.io/badge/pottokao-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-prithivMLmods]: https://img.shields.io/badge/prithivMLmods-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-PrunaAI]: https://img.shields.io/badge/PrunaAI-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-realrebelai]: https://img.shields.io/badge/realrebelai-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-t8star]: https://img.shields.io/badge/t8star-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-themindstudio]: https://img.shields.io/badge/themindstudio-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-unsloth]: https://img.shields.io/badge/unsloth-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-Wickedlizerd]: https://img.shields.io/badge/Wickedlizerd-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-xingewh]: https://img.shields.io/badge/xingewh-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-yunfengwang]: https://img.shields.io/badge/yunfengwang-lightgrey?style=flat-square&logo=huggingface&logoColor=white
[gh-zcf0508]: https://img.shields.io/badge/zcf0508-lightgrey?style=flat-square&logo=huggingface&logoColor=white

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
[badge-fp8]: https://img.shields.io/badge/fp8-28a745?style=flat-square
[badge-fp4]: https://img.shields.io/badge/fp4-20c997?style=flat-square
[badge-nvfp4]: https://img.shields.io/badge/nvfp4-6f42c1?style=flat-square
[badge-mxfp4]: https://img.shields.io/badge/mxfp4-6f42c1?style=flat-square
[badge-int8]: https://img.shields.io/badge/int8-17a2b8?style=flat-square
[badge-int4]: https://img.shields.io/badge/int4-ffc107?style=flat-square
[badge-w4a8]: https://img.shields.io/badge/w4a8-fe7d37?style=flat-square
[badge-w8a8]: https://img.shields.io/badge/w8a8-fe7d37?style=flat-square
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
[ltype-pe]: https://img.shields.io/badge/PE-6f42c1?style=flat-square
[ltype-te]: https://img.shields.io/badge/TE-17a2b8?style=flat-square
