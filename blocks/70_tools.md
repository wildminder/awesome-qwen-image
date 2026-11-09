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
| **sketchbook-style** | ![Agent skill][ltype-skill] | [![][gh-Keith--CY]](https://github.com/Keith-CY/sketchbook-style) | Agent Skill for Claude Code that draws any object as a black-ink industrial-design sketchbook page. Renders on **Qwen-Image 2.1 in ComfyUI** by default (FLUX.2 klein or Z-Image locally on Apple Silicon). Four looks (pen, marker, ink, contrast), five page types, prompt templates that keep every "no …" in the positive prompt at **CFG 1**, an ink post-process to pure one-colour line art, and a script that measures line width and ink coverage. 52 sample pages with prompts and seeds. No API key. Apache-2.0. |

**Gotchas**

1. `QwenImage21Pipeline` requires diffusers **`main`**, since release `0.40.0` does not have it. Install from git.
2. The CFG kwarg is **`true_cfg_scale`**, not `guidance_scale`, and it needs a `negative_prompt` to engage.
3. Width and height must be **multiples of 32**, max edge 2048.
4. With CPU offload, the generator should live on **`'cpu'`**.
5. The default sample is **40 steps with no CFG**. Adding CFG changes the look; don't assume more steps help.
6. Chinese prompts work natively. The PE models are what rewrite them into English.

---

