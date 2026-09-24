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

### Adding a checkpoint or tool

1. **Open the right block** (table above) and add one row to the matching table.
2. **Name it for what it is**, not for who uploaded it — drop the
   `Qwen-Image-2.1` prefix and the owner name. `4bit`, `MLX-4bit`,
   `INT4ConvRot-ComfyUI`; not `Rin247/Qwen-Image-2.1-INT4`.
3. **Use a badge for Precision and Type**, not a bare string. Reuse an existing
   definition from `99_footer.md` when one fits — `badge-int4`, `ltype-style`,
   `task-t2i` — and add a new `[badge-…]` / `[ltype-…]` line only if the value is
   genuinely new. Keep the reference prefix: `gh-` owner, `badge-` precision,
   `ltype-` type, `task-` task.
4. **Link to the repo, not to a file.** If the weights are split into shards,
   point at the repository page so the reader gets all of them. A link to
   `…/resolve/main/model-00001-of-00004.safetensors` is a bug, not a shortcut.
5. **Center what the eye scans.** Precision, Type, and Size columns use
   `:---:`; only prose columns use `:---`.
6. **Say what makes it useful** in Notes — the VRAM it needs, the backend it
   requires, the caveat that trips people up. A row that only restates the name
   is not worth a row.

A row looks like this — `gh-OWNER` is a stand-in, replace it with the real
reference name and add the matching `[gh-OWNER]:` line to `99_footer.md`:

```markdown
| **MLX-4bit** | ![int4][badge-int4] ![bf16][badge-bf16] | 7.12 GB | [![][gh-OWNER]](https://huggingface.co/OWNER/Qwen-Image-2.1-MLX-4bit) | Runs in unified memory; no CUDA. |
```

> [!TIP]
> Multiple downloads for the same thing go in one cell, split with `┊`:
> `[![][gh-A]](LINK_A) ┊ [![][gh-B]](LINK_B)`. Reach for a second row only when
> the entries are genuinely different artifacts, not different downloads of one.

### What earns a place here

A PR is much more likely to be merged if it adds something a reader would
actually stop for. That means: a real checkpoint, quant, LoRA, port, or tool for
Qwen-Image 2.1 — reachable, documented well enough to describe, and not already
listed. Mirrors of an existing entry, personal scratch dumps with no README, and
re-uploads of official weights do not qualify. If you are unsure, open the PR as
a question and say why it seems worth including.

Please include the **model card link** and, where the repo states them, the
parameter count, precision, and file size. Those are the fields the tables are
built from.

### Before you open the PR

- [ ] I edited a file in `blocks/`, **not** `README.md`
- [ ] I added the owner and any new badge to `blocks/99_footer.md`
- [ ] Download links point at the repository, not a single shard
- [ ] Size is measured, not guessed
- [ ] The entry is not already in the list under another name

Rebuild locally to confirm the document still renders:

```bash
python scripts/build_readme.py
```

That regenerates `README.md` from the blocks. Commit **both** the block change
and the rebuilt `README.md` so the rendered document on GitHub matches your
source. If you only have a Markdown editor and no Python, that is fine — open
the PR against the block file alone and say so; a maintainer will run the build.
