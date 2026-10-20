<p id="quant" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◈ Quantizations

Community conversions of the base DiT. Sizes are total weight bytes per repo. Distilled turbo GGUFs live under [Turbo](#turbo) instead.

<p id="quant-gguf" align="center">· · · · · · · · · · · · · ·</p>

### ▣ GGUF

Transformer-only weights for llama.cpp, sorted from the highest quant down. **Q4_K_M** is the usual quality/size balance point. **[Unsloth](https://huggingface.co/unsloth/Qwen-Image-2.1-GGUF)** carries the widest ladder; where two repos offer a quant Unsloth does not, both are linked in the same cell.

<!--GGUF_ROWS-->

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
| **Pipi** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-pipi-qwen-image-2.1-int8-01-unet) | Merged photorealistic DiT in int8 ConvRot, single file for `models/diffusion_models/`, no LoRA needed. Trigger `photorealistic`, Euler, 40 steps, CFG 1.0, and no LoRA stacked on it. Uncensored. |
| **Noct Q** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-Noctaluna]](https://huggingface.co/Noctaluna/Noct-Q-Uncensored-Qwen-Image-2.1) | The photorealistic line, parent of the Anime row below. Merged int8 ConvRot DiT, single file for `models/diffusion_models/`. **Take V4**: the card measures it rendering explicit scenes as asked three times as often as V3. 25 steps, `euler`/`simple`, CFG 3 with a negative prompt. The wider int4/fp8/bf16 ladder and prompt gallery live on Civitai. |
| **Noct Q Anime** | ![int8][badge-int8] | 7.26 GB | ⚠️ [![][gh-Noctaluna]](https://huggingface.co/Noctaluna/Noct-Q-Anime-Uncensored-Qwen-Image-2.1) | Merged anime fine-tune of the DiT, single file, int8 ConvRot, fits 8–12 GB cards. Without the word "anime" in the prompt you get a photo. 25 steps, `euler`/`simple`, CFG 3. Uncensored. |
| **Fwaun Tune** | ![int8][badge-int8] ![bf16][badge-bf16] | 6.76 / 13.25 GB | [![][cv-fwaunstp733]](https://civitai.com/models/2846802/fwaun-tune?modelVersionId=3373014) | Merged fine-tune for flat colour, no lineart, deformed character art. |
| **Ray Qwen2.1 edit-enhanced** | ![int8][badge-int8] | 6.76 GB | ⚠️ [![][cv-Ray3780]](https://civitai.com/models/2972899/ray-qwen21?modelVersionId=3369126) | Custom editing merged into the base DiT; keeps base step counts. |
| **Ray Qwen2.1 turbo v1.3** | ![int8][badge-int8] | 6.70 GB | ⚠️ [![][cv-Ray3780]](https://civitai.com/models/2972899/ray-qwen21?modelVersionId=3395380) | The same editing work folded into the official Turbo checkpoint. |
| **W4A4 NVFP4** | ![nvfp4][badge-nvfp4] | 4.88 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4) | DiT only; near-identical to the INT4 build. |
| **W4A4 INT4** | ![int4][badge-int4] | 4.66 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4) | DiT only. |
| **Uncensored NVFP4** | ![nvfp4][badge-nvfp4] | 4.20 GB | ⚠️ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Uncensored-Quantized/resolve/main/qwen-image-2.1-UC-NVFP4.safetensors) | DiT only, native NVFP4 rather than a GGUF conversion. |

<p id="quant-fp8" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; bf16

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Darkstar ModelOpt FP8** | ![fp8][badge-fp8] | 26.33 GB | [![][gh-HangGlidersRule]](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8) | NVIDIA ModelOpt-derived, full repo. |
| **FP8** | ![fp8][badge-fp8] | 17.96 GB | [![][gh-Rin247]](https://huggingface.co/Rin247/Qwen-Image-2.1-FP8) | Closest thing to a drop-in smaller BF16. |
| **Uncensored BF16 SafeTensor** | ![bf16][badge-bf16] | 14.23 GB | ⚠️ [![][gh-dh123456789123]](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor/resolve/main/qwen-image-2.1-UC-BF16_bf16.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet/resolve/main/Qwen-Image-2.1-_bf16%E6%97%A0%E5%AE%A1%E6%9F%A5.safetensors) | Single 14.23 GB file, the full DiT in one piece, mirrored by both repos. |
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
