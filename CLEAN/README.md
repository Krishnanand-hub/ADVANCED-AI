# CLEAN — Notebooks, Report & How to Run

This directory holds the final, assessed deliverables for CSC8645: two Jupyter notebooks and the written report.

```
CLEAN/
├── README.md              ← this file
├── ADVAIT1.ipynb          ← Project 1: Food Image Semantic Segmentation (FoodSeg103)
├── ADVAIT2.ipynb          ← Project 2: Multimodal Idiomaticity Representation
└── CSC8645 Report.pdf     ← full written report and analysis
```

---

## What each notebook does

**`ADVAIT1.ipynb` — Food Segmentation.** 21 logical blocks: environment setup → data extraction → EDA → SLIC+KMeans baseline → SegFormer-b0 zero-shot → SegFormer-b2 fine-tuning → SegFormer-b4 fine-tuning → per-class IoU analysis → ablation study → bootstrap confidence intervals → error analysis → model persistence → live demo cell.

**`ADVAIT2.ipynb` — Multimodal Idiomaticity.** 22 logical blocks: environment setup → data discovery → EDA → caption-similarity baseline → CLIP zero-shot baseline → Flan-T5 data augmentation → DeBERTa-v3 cross-encoder fine-tuning → weighted ensemble → Logistic Regression meta-learner (stacking) → ablation study → bootstrap analysis → test predictions → live demo cell.

Both notebooks are designed to run **top to bottom, block by block**, in order.

---

## Environment

These notebooks were developed on **Google Colab Pro+ with an A100 GPU** and **Python 3.12**. They mount Google Drive and read the datasets from Drive — they are *not* self-contained scripts.

| Requirement | Detail |
|---|---|
| Platform | Google Colab (Pro+ recommended for A100 / high RAM) |
| GPU | CUDA GPU. SegFormer-b4 and DeBERTa-v3 fine-tuning need ~A100-class memory |
| Python | 3.12 |
| Key libraries | `torch`, `transformers`, `scikit-learn`, `scikit-image`, `sentence-transformers`, `pandas`, `numpy`, `matplotlib` |
| Reproducibility | Random seeds fixed at 42 (Python, NumPy, PyTorch); TF32 enabled on A100 |

> Block 1 of each notebook installs the required libraries. Run it once per session.

---

## How to run (clone → reproduce)

1. **Get the datasets.** The notebooks expect the datasets in Google Drive, not in this repo's working directory. See `../RAW/README.md` for download links and the exact folder layout. Upload the two dataset folders to your Drive (or use the public sources directly).

2. **Open in Colab.** Upload `ADVAIT1.ipynb` (or `ADVAIT2.ipynb`) to Google Colab. Select a GPU runtime (`Runtime → Change runtime type → GPU`, ideally A100 via Pro+).

3. **Run Block 1** to install dependencies, then **Block 3** to mount Drive and extract/locate the dataset.

4. **Set the paths.** Block 4 (Task 1) / Blocks 3–4 (Task 2) define dataset paths. Update these to match where you placed the data in your Drive. The notebooks verify every path exists before loading.

5. **Run all remaining blocks in order.** Baselines run first, then fine-tuning, then evaluation. Fine-tuning is the slow part (multiple epochs on GPU).

6. **(Optional) Demo cell.** The final block in each notebook loads a saved model from Drive and runs a single live prediction — useful for a quick sanity check without re-training.

---

## Things to consider & expected errors

- **Hardcoded Drive paths.** The biggest reproducibility gap: paths point to the original author's Drive. If you get `FileNotFoundError` / `path does not exist`, you have not updated the path-configuration block to your own Drive layout. Fix the paths first.
- **Out-of-memory (CUDA OOM).** SegFormer-b4 and DeBERTa-v3 are memory-hungry. On a smaller GPU (e.g. free-tier T4), reduce batch size, lower image resolution, or skip the b4 / augmented runs. The b2 model is the lighter option.
- **DeBERTa-v3 + mixed precision.** DeBERTa-v3's gradient-disentangled embeddings are **incompatible with FP16/AMP**. The notebook enforces FP32 for this model on purpose — do not switch it to mixed precision or training will break.
- **Library version drift.** Built against Python 3.12 library versions current at submission. If a `transformers` / `sentence-transformers` API has since changed, pin the versions from Block 1 rather than installing latest.
- **Long runtimes & session timeouts.** Full fine-tuning takes time; Colab can disconnect on idle. Keep the tab active, or save checkpoints to Drive (the persistence block does this).
- **Small Task 2 evaluation set.** Task 2's validation set is only 9 grouped samples, so metrics vary run-to-run. This is a property of the data, not a bug — interpret accordingly.

---

## Note on repository hygiene

For grading these notebooks ship alongside their data in `../RAW/`. For a public/portfolio version, datasets should **not** live in Git — add a `.gitignore`, remove the committed data, and have users download it from the sources in `../RAW/README.md`.
