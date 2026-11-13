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
| **Radiance IQ8R** | ![int8][badge-int8] | 17.32 GB | [![][gh-StillDeadcode]](https://huggingface.co/StillDeadcode/radiance-qwen-image-2.1-iq8r) | Full pipeline in one `.rad` container for the radiance engine on AMD RDNA4 / ROCm: INT8 W8A8 with Hadamard rotation, 40 steps. Needs a radiance build past 1.17.0. Qwen Research License, non-commercial. |
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
| **FineView STORY3** | ![int8][badge-int8] ![fp8][badge-fp8] | 6.76 / 6.63 GB | ⚠️ [![][cv-AiMeASOP]](https://civitai.com/models/452459/fineview-or-story3?modelVersionId=3370753) | Merged uncensored DiT fine-tune (REDQW21 v2) in INT8 ConvRot and FP8; the v1 version adds BF16, NVFP4 and W4A8 builds. |
| **Uncensored Omni (quantized)** | ![fp8][badge-fp8] ![int8][badge-int8] ![nvfp4][badge-nvfp4] ![Q4_K_M][badge-Q4_K_M] | 7.12 / 7.26 / 4.20 / 4.47 GB | ⚠️ [![][gh-chatrlabs]](https://huggingface.co/chatrlabs/Qwen-Image-2.1-Uncensored-Omni-Quantized/resolve/main/qwen-image-2.1-step90000-FP8-comfy.safetensors) ┊ [![][gh-chatrlabs]](https://huggingface.co/chatrlabs/Qwen-Image-2.1-Uncensored-Omni-Quantized/resolve/main/qwen-image-2.1-step90000-INT8-ConvRot-comfy.safetensors) ┊ [![][gh-chatrlabs]](https://huggingface.co/chatrlabs/Qwen-Image-2.1-Uncensored-Omni-Quantized/resolve/main/qwen-image-2.1-step90000-NVFP4-comfy.safetensors) ┊ [![][gh-chatrlabs]](https://huggingface.co/chatrlabs/Qwen-Image-2.1-Uncensored-Omni-Quantized/resolve/main/qwen-image-2.1-step90000-Q4_K_M.gguf) | Step 90,000 of the same run as the v0.1 adapter, merged into the base at strength 1.0 and then requantized, not a conversion of that file. Also ships a BF16-to-Q4_0 GGUF ladder and torchao/bnb diffusers folders; text encoder, processor and VAE come from the base repo. |
| **W4A4 NVFP4** | ![nvfp4][badge-nvfp4] | 4.88 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-nvfp4) | DiT only; near-identical to the INT4 build. |
| **W4A4 INT4** | ![int4][badge-int4] | 4.66 GB | [![][gh-ModelsLab]](https://huggingface.co/ModelsLab/Qwen-Image-2.1-W4A4-int4) | DiT only. |
| **Uncensored NVFP4** | ![nvfp4][badge-nvfp4] | 4.20 GB | ⚠️ [![][gh-abenzerps]](https://huggingface.co/abenzerps/Qwen-Image-2.1-Turbo-Quantized/resolve/main/qwen-image-2.1-turbo-NVFP4.safetensors) | DiT only, native NVFP4 rather than a GGUF conversion. |

<p id="quant-fp8" align="center">· · · · · · · · · · · · · ·</p>

### ▣ FP8 &amp; bf16

| Name | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :--- |
| **Darkstar ModelOpt FP8** | ![fp8][badge-fp8] | 26.33 GB | [![][gh-HangGlidersRule]](https://huggingface.co/HangGlidersRule/Darkstar-Qwen-Image-2.1-Base-ModelOpt-FP8) | NVIDIA ModelOpt-derived, full repo. |
| **TailSteer** | ![bf16][badge-bf16] | 33.12 GB | [![][gh-Lyricccco]](https://huggingface.co/Lyricccco/Qwen-Image-2.1-TailSteer) | Complete BF16 fine-tune, not a quant: cuts texture drift across repeated edits (+1.22 dB PSNR, 11.5% lower LPIPS at turn 10). Drop-in model-ID swap; the repo carries the diffusers shards and a single-file ComfyUI transformer. |
| **Uncensored BF16 SafeTensor** | ![bf16][badge-bf16] | 14.23 GB | ⚠️ [![][gh-dh123456789123]](https://huggingface.co/dh123456789123/Qwen-Image-2.1-Uncensored-BF16-SafeTensor/resolve/main/qwen-image-2.1-UC-BF16_bf16.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-bf16-unet/resolve/main/Qwen-Image-2.1-_bf16%E6%97%A0%E5%AE%A1%E6%9F%A5.safetensors) | Single 14.23 GB file, the full DiT in one piece, mirrored by both repos. |
| **Uncensored UC FP8 / Q4_K_M** | ![fp8][badge-fp8] ![Q4_K_M][badge-Q4_K_M] | 7.12 / 4.60 GB | ⚠️ [![][gh-og1936]](https://huggingface.co/og1936/qwen-image-2.1-uncensored-weights/resolve/main/qwen-image-2.1-UC-fp8.safetensors) ┊ [![][gh-og1936]](https://huggingface.co/og1936/qwen-image-2.1-uncensored-weights/resolve/main/qwen-image-2.1-UC-Q4_K_M.gguf) | Mirror of the abenzerps uncensored DiT removed on 2026-11-03. FP8 e4m3 DiT plus a Q4_K_M GGUF of the same weights; bring your own text encoder and VAE. |
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
