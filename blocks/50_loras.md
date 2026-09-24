<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, control, and fix adapters. All target the base DiT unless noted.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **Fix** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.11 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix/resolve/main/qwen-image-2.1-fix-1.0-comfy.safetensors) | The most-liked community LoRA. |
| **De-AI LoRA pack** | ![Style][ltype-style] | ![bf16][badge-bf16] | 2.45 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora) | 8-file "remove the AI look" pack (CN filenames). |
| **Object Mover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview) | Bbox object *moving*, 6 checkpoints. |
| **Object Remover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview) | Bbox object removal, full-quality variant. |
| **Natural Exposure LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA) | Exposure correction, 5 checkpoints. |
| **Object Remover Bbox turbo** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo) | 4-step-compatible variant. |
| **Normal2RGB** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-Aero--Ex]](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB/resolve/main/Normal2RGB_4000.safetensors) | Normal map → RGB render, 3 checkpoints. |
| **Sts2 Cards Drawer** | ![Style][ltype-style] | ![fp16][badge-fp16] | 0.10 GB | [![][gh-Airmongsity]](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer) | `deckbuilder_cardart_style_lora_v1_fp16`. |
| **RadianceChrome Voluptuous** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-AIImageStudio]](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0) | Character-style LoRA. |
| **lora** | ![General][ltype-general] | ![bf16][badge-bf16] | — | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-lora) **Empty** | No files uploaded. |
| **aio-nsfw-lora** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | — | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora) **Empty** | No files uploaded. |
| **breasts-slider-lora** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | — | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora) **Empty** | No files uploaded. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used — the uncensored GGUF in particular is the most-downloaded repo in this ecosystem. They carry the same **Qwen Research License** as the base model; a research license is not a license to do whatever you want, and you remain responsible for how you use them. Several are empty placeholders created on release day — check before planning around them.

