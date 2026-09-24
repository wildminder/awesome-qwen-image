<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ▸ Turbo &amp; step distillation

Few-step students of the base model, distilled with Distribution Matching Distillation. They trade fidelity for speed: useful for iteration and batch work, weaker than 40 steps on multi-reference composition and identity-preserving edits.

**Current release: `v0.2.1` (2026-09-24), 6 steps.** It supersedes `v0.2` (5-step name, sampled at 6) and `v0.1` (4 steps). Sample with `sigmas=[1.0, 0.9375, 0.875, 0.75, 0.5, 0.25]`, CFG 1.0, empty negative prompt. Official source: **[Viggle/Qwen-Image-2.1-viggle-turbo](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)**.

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

Repo links: **[Viggle](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo)** · **[chfm](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo)** (v0.2 snapshot) · **[t8star](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy)** · **[RunningHubAI](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora)** · **[addlabsviral](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-bf16)** · **[cgb](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX)**

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **v0.2.1 LoRA r256** | 6 | ![bf16][badge-bf16] | 1.36 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r256.safetensors) | **Start here.** Sharpest and most faithful to the 40-step base; runs the demo Space. Load on the base transformer at runtime — do not merge. |
| **v0.2.1 LoRA r128** | 6 | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2.1-6step-lora-r128.safetensors) | Same adapter cut to rank 128; what the shipped ComfyUI workflows use. |
| **v0.2.1 adapter (peft)** | 6 | ![fp32][badge-fp32] | 2.72 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/peft_v0.2.1) | The v0.2.1 LoRA in PEFT key format, F32 as trained. |
| **v0.2 LoRA r256** | 5 / 6 | ![bf16][badge-bf16] | 1.36 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) ┊ [![][gh-chfm]](https://huggingface.co/chfm/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r256.safetensors) | Step 600 of the same run. The `5step` in the name is the launch schedule; sample at 6 like v0.2.1. |
| **v0.2 LoRA r128** | 5 / 6 | ![bf16][badge-bf16] | 0.68 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-v0.2-5step-lora-r128.safetensors) | Rank-128 cut of v0.2. |
| **v0.1 full fine-tune** | 4 | ![bf16][badge-bf16] | 14.23 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/tree/main/transformer) | Merged transformer, no LoRA needed. This is what the GGUF quants above were built from. |
| **v0.1 LoRA r64** | 4 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-lora-r64.safetensors) ┊ [![][gh-t8star]](https://huggingface.co/t8star/Qwen-Image-2.1-viggle-turbo-4step-r64-comfy/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) ┊ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) | Superseded — diversity collapsed to 0.75× base and the output was visibly softer. Kept for reproducibility. |
| **Turbo BF16 diffusers** | 4 | ![bf16][badge-bf16] | 32.44 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-bf16) | Full pipeline (TE + DiT + VAE), ready to load with `QwenImage21Pipeline`. DiT is byte-identical in size to the v0.1 transformer above, re-sharded 2-way. |
| **Turbo FP4 diffusers** | 4 | ![fp4][badge-fp4] | 11.41 GB | [![][gh-addlabsviral]](https://huggingface.co/addlabsviral/qwen-image2.1-turbo-fp4) | Same v0.1 pipeline with an FP4 DiT; the TE is the larger half at 6.73 GB. |
| **Turbo ONNX (browser)** | 4 | ![int4][badge-int4] | ~17.2 GB | [![][gh-cgb]](https://huggingface.co/cgb/Qwen-Image-2.1-Turbo-ONNX) | r64 LoRA merged into the denoiser, then Q4 MatMulNBits. WebGPU in-browser; needs the FreeGen pipeline and a desktop adapter. Experimental. |
