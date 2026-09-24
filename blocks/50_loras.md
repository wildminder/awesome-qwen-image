<p id="lora" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ◉ LoRA &amp; adapters

Style, control, and fix adapters. All target the base DiT unless noted. Grouped by type, then by size.

| Name | Type | Precision | Size | Links | Notes |
| :--- | :---: | :---: | :---: | :---: | :--- |
| **ControlNet-Union** | ![Control][ltype-control] | ![bf16][badge-bf16] | 7.55 GB | [![][gh-alibaba--pai]](https://huggingface.co/alibaba-pai/Qwen-Image-2.1-Fun-Controlnet-Union/resolve/main/Qwen-Image-2.1-Fun-Controlnet-Union.safetensors) | **Official Alibaba PAI / VideoX-Fun.** One checkpoint for 8 conditions (Canny, Depth, Grayscale, HED, Lineart, MLSD, Pose, Scribble) plus inpainting. Control branch only, 16 injection points, loaded `strict=False`. | 
| **Object Mover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Mover-Bbox-Preview) | Bbox object *moving*, 6 checkpoints. |
| **Object Remover Bbox Preview** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.50 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-Preview) | Bbox object removal, full-quality variant. |
| **Object Remover Bbox turbo** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Object-Remover-Bbox-turbo) | 4-step-compatible variant. |
| **Orbit Alpha** | ![Control][ltype-control] | ![bf16][badge-bf16] | 0.17 GB | [![][gh-ML--Intern--lab]](https://huggingface.co/ML-Intern-lab/Qwen-Image-2.1-camera-control/resolve/main/checkpoints/steps2000res768/orbit_alpha_lora_gate_up_split.safetensors) | **Official ML-Intern-lab.** One RGBA image in, the same object from a new viewpoint out. Rank 32, 2,000 steps at 768 px. Use the `_gate_up_split` file — the other checkpoint in the repo does not load correctly. `<orbit>` grammar, 40 steps, no CFG. |
|  |  |  |  |  |  |
| **Fix** | ![Fix][ltype-fix] | ![bf16][badge-bf16] | 0.11 GB | [![][gh-e--n--v--y]](https://huggingface.co/e-n-v-y/Qwen-Image-2.1-Fix/resolve/main/qwen-image-2.1-fix-1.0-comfy.safetensors) | The most-liked community LoRA. |
|  |  |  |  |  |  |
| **De-AI LoRA pack** | ![Style][ltype-style] | ![bf16][badge-bf16] | 2.45 GB | [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1ai-lora) | 8-file "remove the AI look" pack (CN filenames). |
| **Natural Exposure LoRA** | ![Style][ltype-style] | ![bf16][badge-bf16] | 0.42 GB | [![][gh-prithivMLmods]](https://huggingface.co/prithivMLmods/Qwen-Image-2.1-Natural-Exposure-LoRA) | Exposure correction, 5 checkpoints. |
| **Sts2 Cards Drawer** | ![Style][ltype-style] | ![fp16][badge-fp16] | 0.10 GB | [![][gh-Airmongsity]](https://huggingface.co/Airmongsity/Qwen-Image-2.1-Sts2-Cards-Drawer) | `deckbuilder_cardart_style_lora_v1_fp16`. |
|  |  |  |  |  |  |
| **Normal2RGB** | ![Utility][ltype-utility] | ![bf16][badge-bf16] | 0.25 GB | [![][gh-Aero--Ex]](https://huggingface.co/Aero-Ex/Qwen-Image2.1_Normal2RGB/resolve/main/Normal2RGB_4000.safetensors) | Normal map → RGB render, 3 checkpoints. |
|  |  |  |  |  |  |
| **RadianceChrome Voluptuous** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.17 GB | ⚠️ [![][gh-AIImageStudio]](https://huggingface.co/AIImageStudio/RadianceChromeVoluptuous_QwenImage2.1_v1.0) | Character-style LoRA. |
| **NSFW Image Edit** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.08 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-aio-nsfw-lora/resolve/main/Qwen-Image-2.1%20NSFW%20Image%20Edit.safetensors) | Editing LoRA, uploaded 2026-09-24. |
| **Breasts Slider V1** | ![NSFW][ltype-nsfw] | ![bf16][badge-bf16] | 0.003 GB | ⚠️ [![][gh-RunningHubAI]](https://huggingface.co/RunningHubAI/rh-qwen-image-2.1-breasts-slider-lora/resolve/main/Pornmaster_QI2.1_Breasts_Slider_V1.safetensors) | Slider control, 3 MB. |

> [!CAUTION]
> Repos marked ⚠️ are uncensored, abliterated, or NSFW. They are listed for completeness because they are widely used — the uncensored GGUF in particular is the most-downloaded repo in this ecosystem. They carry the same **Qwen Research License** as the base model; a research license is not a license to do whatever you want, and you remain responsible for how you use them. Sizes are the LoRA file only; none of these merge a base model.

