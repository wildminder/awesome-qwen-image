<p id="turbo" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚡ Turbo &amp; step distillation

4-step generation. Useful for iteration and batch work; expect some quality loss versus 40 steps.

| Name | Steps | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Viggle Turbo** | 4 / 5 | ![bf16][badge-bf16] | 19.33 GB | [![][gh-Viggle]](https://huggingface.co/Viggle/Qwen-Image-2.1-viggle-turbo/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-lora-r64.safetensors) | Source repo. `4step-lora-r64` and `5step-lora-r256` PEFT adapters plus a full `transformer/`. |
| **Viggle Turbo GGUFs** | 4 | ![Q2_K][badge-Q2_K] ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 34.07 GB | [![][gh-realrebelai]](https://huggingface.co/realrebelai/Viggle_Qwen-Image-2.1-Turbo_GGUFs) | Turbo in GGUF, Q2_K → Q8_0. |
| **Viggle 4-step Turbo GGUF** | 4 | ![Q3_K_M][badge-Q3_K_M] ![Q4_K_M][badge-Q4_K_M] ![Q5_K_M][badge-Q5_K_M] ![Q6_K][badge-Q6_K] ![Q8_0][badge-Q8_0] | 29.91 GB | [![][gh-Abiray]](https://huggingface.co/Abiray/Qwen-Image-2.1-viggle-4-steps-turbo-GGUF) | Alternate turbo GGUF set. |
| **Viggle Turbo r64 ComfyUI** | 4 | ![bf16][badge-bf16] | 0.34 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-viggle-turbo-4step-r64-comfyui-t8-lora/resolve/main/Qwen-Image-2.1-viggle-turbo-4step-r64-comfyui-T8.safetensors) | Rank-64 Viggle turbo LoRA. |
| **turbo 4step lora** | 4 | ![bf16][badge-bf16] | — | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-turbo-4step-lora) **Empty** | Created 2026-09-23, no files yet. |

