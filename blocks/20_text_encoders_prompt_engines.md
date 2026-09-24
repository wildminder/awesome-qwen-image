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

The INT8 ConvRot pack is the only single download covering both PE-T2I and PE-I2I.

