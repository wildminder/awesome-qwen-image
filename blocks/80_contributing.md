<p id="contributing" align="center">◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆◇◆</p>

## ✎ Contributing

This list is a **set of Markdown files, not one**. `README.md` is generated and
should never be edited directly — your change will be overwritten on the next
build. Each section lives in its own file under [`blocks/`](blocks/):

| To change… | Edit this file |
| :--- | :--- |
| Title, badges, table of contents | [`blocks/00_header.md`](blocks/00_header.md) |
| Base model or official ComfyUI files | [`blocks/10_official_checkpoints.md`](blocks/10_official_checkpoints.md) |
| Prompt engines, rewriters, text encoders | [`blocks/20_text_encoders_prompt_engines.md`](blocks/20_text_encoders_prompt_engines.md) |
| GGUF, 4/8-bit, FP8, Nunchaku quants | [`blocks/30_quantizations.md`](blocks/30_quantizations.md) |
| Turbo and step-distilled models | [`blocks/40_turbo.md`](blocks/40_turbo.md) |
| LoRAs and adapters | [`blocks/50_loras.md`](blocks/50_loras.md) |
| Apple, MNN, AMD, VAE ports | [`blocks/60_platform_ports.md`](blocks/60_platform_ports.md) |
| Tools, notebooks, training scripts | [`blocks/70_tools.md`](blocks/70_tools.md) |
| Badge and link definitions | [`blocks/99_footer.md`](blocks/99_footer.md) |

The file prefix controls the order — the build concatenates `blocks/*.md` in
filename order, so `10_` always lands before `20_`. A new section just needs a
new number.

