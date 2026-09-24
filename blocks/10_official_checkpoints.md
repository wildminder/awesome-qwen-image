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

