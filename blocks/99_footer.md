<a id="contributing"></a>

## ⌘ Contributing

This list is assembled from a raw link pool and verified against the Hugging Face API. Contributions are welcome.

**What makes a good entry**

* A real, working Qwen-Image 2.1 artifact — not a placeholder, not a name-only repo.
* A model card explaining what the conversion actually is (base model, precision, method).
* Files actually uploaded and downloadable.

**How to submit**

Open a pull request adding the repo to the right section of the matching `blocks/*.md` file. **Do not edit `README.md` directly** — it is generated.

```bash
# after editing blocks/*.md
python scripts/build_readme.py
```

Include the repo name as a link, plus a one-line note saying what makes it different from the alternatives in that table. Tables are sorted by usefulness, not alphabetically, so a new entry may need to slot into a specific position.

**Keeping data fresh**

The download counts, sizes, and file listings in this document were read from the HF API at build time and will drift. To refresh them:

```bash
python scripts/fetch_meta.py    # base metadata for every repo
python scripts/fetch_cards.py   # model cards (paced, avoids rate limits)
python scripts/fetch_sizes.py   # per-file byte sizes
python scripts/classify.py      # regenerate docs/classified.json
```

---

<a id="license"></a>

## ⚖ License

This list itself is released under [CC0-1.0](https://creativecommons.org/publicdomain/zero/1.0/) — the facts and links are not copyrightable, and the tables are generated from public Hugging Face metadata.

**The weights are not.** Every model linked here is a derivative of `Qwen/Qwen-Image-2.1` and is distributed under the **Qwen Research License**. Some entries additionally carry `Apache-2.0` metadata (the Paiton/MXFP4 builds, the Heretic text encoder, the Colab notebooks), which covers the conversion code and packaging, not the underlying weights.

Read the license before you deploy anything commercially. The research license is not a commercial license.

> [!IMPORTANT]
> Uncensored and abliterated repos in this list are still bound by the same Qwen Research License. Removing a refusal direction from a model does not create new legal rights, and redistributing such weights can violate the terms of the base model's license.

---

<div align="center">

**Awesome Qwen-Image 2.1** · curated from 95 unique repositories

<sub>Model facts verified against the Hugging Face API on 2026-09-24.</sub>

</div>

<!-- MARKDOWN LINK REFERENCES -->
[hf-shield]: https://img.shields.io/badge/Hugging%20Face-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black
[hf-url]: https://huggingface.co/Qwen/Qwen-Image-2.1
[lic-shield]: https://img.shields.io/badge/model%20license-Qwen%20Research-yellow?style=for-the-badge
[lic-url]: https://huggingface.co/Qwen/Qwen-Image-2.1/blob/main/LICENSE
[commit-shield]: https://img.shields.io/badge/docs-auto--generated-blue?style=for-the-badge
[commit-url]: https://github.com/QwenLM/Qwen-Image-2.1
[prs-shield]: https://img.shields.io/badge/PRs-welcome-brightgreen?style=for-the-badge
[prs-url]: https://github.com/QwenLM/Qwen-Image-2.1/pulls
