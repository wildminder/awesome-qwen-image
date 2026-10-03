<p id="tools" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ⚙ Tools &amp; notebooks

| Name | Type | Links | Notes |
| :--- | :---: | :---: | :--- |
| **training assistant v1** | ![Training][ltype-training] | [![][gh-SimpleTuner]](https://huggingface.co/SimpleTuner/Qwen-Image-2.1-training-assistant-v1) | A working `pytorch_lora_weights.safetensors` + `training_details.json` you can inspect to learn a real SimpleTuner config. |
| **qwen_image2.1_colab** | ![Notebook][ltype-notebook] | [![][gh-bluemorpholimited]](https://huggingface.co/bluemorpholimited/qwen_image2.1_colab) | 7-cell notebook with form UI and `enable_model_cpu_offload()`. Works on L4 22 GB; **T4 16 GB will OOM by design**. |
| **qwen_image2.1_molab** | ![Notebook][ltype-notebook] | [![][gh-bluemorpholimited]](https://huggingface.co/bluemorpholimited/qwen_image2.1_molab) | Script version of the above, tuned for Marimo and Blackwell. |
| **Qwen-Image-2.1-Skills** | ![Agent skill][ltype-skill] | [![][gh-iamvts]](https://huggingface.co/iamvts/Qwen-Image-2.1-Skills) | Turns a short scene into a structured prompt for believable casual phone photography. 22 example images. |
| **qwen-image-2.1-p150** | ![Port][ltype-port] | [![][gh-changh95]](https://huggingface.co/changh95/qwen-image-2.1-p150) | Tenstorrent Blackhole p150a. `tt-model pull --with-weights` then `tt-model serve`. |
| **TensorFold RTX loader** | ![Node pack][ltype-nodes] | [![jayleaton](https://img.shields.io/badge/jayleaton-17a2b8?style=flat-square&logo=github&logoColor=white)](https://github.com/jayleaton/qwen-image21-tensorfold-rtx) | ComfyUI node that replaces `Unet Loader (GGUF)` and runs the DiT through TensorFold's **NVFP4 and FP8** kernels, skipping the per-step Q6_K decode that costs about 65% of GPU time. Measured 4.0× faster at 1024×1024 and 8.4× at 480×608 on an RTX 5070 Ti. **RTX 50-series only** — NVFP4 needs block-scaled FP4 tensor cores that Blackwell has and Ada does not; 40-series would have to use the fp8 path, which is untested. The loader **refuses LoRA, ControlNet, weight patches and attention patches**, so it is no use in a ControlNet workflow. Takes the same GGUF you would otherwise load; conversion and calibration run once. Marked work in progress, Apache-2.0. |
| **ComfyUI-AlphaTrace** | ![Node pack][ltype-nodes] | [![Nynxz](https://img.shields.io/badge/Nynxz-17a2b8?style=flat-square&logo=github&logoColor=white)](https://github.com/Nynxz/ComfyUI-AlphaTrace) | Diagnostic sampler nodes for the native RGBA output: they emit the predicted image, the alpha map and per-step alpha statistics at every step. Purely observational. For 2.1 pass **LTXVScheduler** as `sigmas` (`max_shift` 0.69, `base_shift` 0.54, `stretch` on, `terminal` 0.02) to match the official schedule. MIT, no extra dependencies, example workflow included. |
| **Qwen-Image-2.1 VRAM calculator** | ![Utility][ltype-utility] | [![][gh-modelvram]](https://modelvram.com/qwen-image-2-1-vram-calculator/) | Pick the DiT, text encoder and VAE files (BF16, FP8, INT8, GGUF), where the encoder runs and the image size; shows the peak VRAM range and which 8–32 GB GPUs fit, next to peaks people measured in ComfyUI and diffusers. |
| **Image Studio** | ![Utility][ltype-utility] | [![][gh-tamimKTH]](https://github.com/tamimKTH/image-studio) | Local web app for Apple Silicon (64 GB+): text-to-image, edits with up to 10 reference images, transparent PNGs, background removal, and a node canvas that chains steps. Runs the Uncensored Q8_0 GGUF on ComfyUI, with bf16 copies of the int8 text encoders because MPS has no int8 matmul. AGPL. |

**Gotchas that will save you an afternoon**

1. `QwenImage21Pipeline` requires diffusers **`main`** — release `0.40.0` does not have it. Install from git.
2. The CFG kwarg is **`true_cfg_scale`**, not `guidance_scale`, and it needs a `negative_prompt` to engage.
3. Width and height must be **multiples of 32**, max edge 2048.
4. With CPU offload, the generator should live on **`'cpu'`**.
5. The default sample is **40 steps with no CFG**. Adding CFG changes the look; don't assume more steps help.
6. Chinese prompts work natively — the PE models are what rewrite them into English.

---

