# CSC8645 — Advanced AI: Food Segmentation & Multimodal Idiom Disambiguation

Two deep-learning pipelines built for the **CSC8645 Advanced AI** module at Newcastle University (MSc Data Science & AI). Each tackles a distinct, modern problem — one in computer vision, one in multimodal NLP — and each progresses deliberately from a non-learning baseline to fine-tuned transformer models, with ablation studies and statistical-reliability analysis.

> **Author:** Krishnanand Sagar · **Module:** CSC8645 (Advanced AI)

---

## Project 1 — Food Image Semantic Segmentation (FoodSeg103)

**Aim.** Segment food images at the ingredient level across 104 classes (103 ingredients + background), assigning every pixel to a food category.

**Abstract.** Starting from a training-free computer-vision baseline (SLIC superpixels + KMeans colour clustering) and a zero-shot deep baseline (SegFormer-b0 pretrained on ADE20K), the pipeline fine-tunes transformer segmentation models (SegFormer-b2 and the higher-capacity SegFormer-b4) on FoodSeg103 by replacing the segmentation head with a 104-class output layer. Training uses AdamW with a cosine warmup schedule, mixed precision, gradient clipping, and data augmentation. The work isolates the contribution of each design decision through an ablation study and reports bootstrap confidence intervals to communicate result reliability honestly.

**What was achieved (verified from notebook outputs):**

| Method | Mean per-image mIoU | Pixel Accuracy |
|---|---|---|
| SLIC + KMeans (traditional baseline) | 0.0019 | — |
| SegFormer-b0 zero-shot (ADE20K) | 0.0247 | — |
| SegFormer-b2 fine-tuned | 0.353 | 0.727 |
| **SegFormer-b4 fine-tuned** | **0.383** | **0.741** |

> **Metric note (important for honest reporting):** the *per-image mean* mIoU above is the per-sample-averaged figure. The same models report a lower *dataset-aggregated* mIoU during training (≈0.142 for b2, ≈0.164 for b4). These differ only by aggregation method — always state which one you mean. Fine-tuning improves mIoU by roughly **two orders of magnitude** over the traditional baseline, which is the headline takeaway.

---

## Project 2 — Multimodal Idiomaticity Representation

**Aim.** Given a nominal compound (e.g. *"bad apple"*), a context sentence, and three candidate images, predict which image best depicts the compound's intended sense (idiomatic vs. literal).

**Abstract.** The system fuses three complementary signals: text-only caption similarity (all-mpnet-base-v2), vision-language grounding (CLIP-ViT-Large zero-shot), and a fine-tuned DeBERTa-v3 cross-encoder that scores (query, caption) pairs jointly. Flan-T5-Large generates label-conditioned paraphrases to augment training data. A Logistic Regression **meta-learner** then stacks the three base signals into a single prediction. The pipeline includes EDA, an ablation study, and a bootstrap confidence-interval analysis that explicitly accounts for the small evaluation set.

**What was achieved (verified from notebook outputs):**

| Method | Accuracy | Macro F1 |
|---|---|---|
| Caption similarity (text-only baseline) | 0.111 | 0.074 |
| CLIP-Large zero-shot | 0.111 | — |
| DeBERTa-v3 fine-tuned | 0.667 | 0.641 |
| **Meta-learner (stacking ensemble)** | **0.667** | **0.667** |

> **Statistical honesty (read before quoting these numbers):** the validation set contains only **9 grouped samples**. Bootstrap resampling (1,000 iterations) produces a 95% confidence interval of roughly **[0.33, 1.00]**. These results are **illustrative, not statistically significant** — a substantially larger evaluation set would be required for robust conclusions. This limitation is analysed directly in the notebook.

---

## Repository Structure

```
ADVANCED-AI/
├── README.md                          ← this file
├── CLEAN/                             ← deliverables (notebooks + report)
│   ├── README.md                      ← how to run, environment, troubleshooting
│   ├── ADVAIT1.ipynb                  ← Project 1: Food Segmentation
│   ├── ADVAIT2.ipynb                  ← Project 2: Multimodal Idiomaticity
│   └── CSC8645 Report.pdf             ← written report / analysis
└── RAW/                              ← datasets + assignment brief
    ├── README.md                      ← dataset sources, licenses, structure
    ├── CSC8645-Assignment-S26.pdf     ← official coursework instructions
    ├── Task 1 Food Segmentation Dataset/   ← FoodSeg103 (public, Apache-2.0)
    └── Task+2+NLP+Dataset/                 ← AdMIRe-derived coursework subset
```

## Tech Stack

- **Frameworks:** PyTorch, Hugging Face Transformers, scikit-learn
- **Vision models:** SegFormer-b0/b2/b4, CLIP-ViT-Large
- **Language models:** DeBERTa-v3, all-mpnet-base-v2, Flan-T5-Large
- **Classical CV/ML:** SLIC superpixels, KMeans, Logistic Regression (meta-learner)
- **Environment:** Google Colab Pro+ (A100 GPU), Python 3.12

See `CLEAN/README.md` to run the notebooks and `RAW/README.md` for dataset access.
