# RAW — Datasets & Coursework Brief

This directory holds the input data for both projects plus the official assignment instructions. The notebooks in `../CLEAN/` read from these datasets (via Google Drive at runtime — see `../CLEAN/README.md`).

```
RAW/
├── README.md                          ← this file
├── CSC8645-Assignment-S26.pdf         ← official coursework instructions (Newcastle University)
├── Task 1 Food Segmentation Dataset/  ← FoodSeg103 (public)
└── Task+2+NLP+Dataset/                ← AdMIRe-derived coursework subset (provided by module)
```

---

## Task 1 — FoodSeg103 (PUBLIC)

A large-scale benchmark for ingredient-level food image segmentation, curated from the Recipe1M recipe dataset and human-annotated with pixel-wise masks.

- **Contents:** 7,118 images, 104 classes (103 food ingredients + background), split 4,983 train / 2,135 test.
- **License:** Apache-2.0.
- **Publicly available from:**
  - Hugging Face: `EduardoPacheco/FoodSeg103` — https://huggingface.co/datasets/EduardoPacheco/FoodSeg103
  - Official benchmark repo (MM'21): `LARC-CMU-SMU/FoodSeg103-Benchmark-v1` — https://github.com/LARC-CMU-SMU/FoodSeg103-Benchmark-v1
- **Source paper:** Wu et al., *A Large-Scale Benchmark for Food Image Segmentation*, ACM MM 2021.

**Folder structure:**

```
Task 1 Food Segmentation Dataset/
└── FoodSeg103/
    ├── category_id.txt              ← 104 class id → name mappings (0 = background)
    ├── Readme.txt                   ← original dataset readme
    ├── ImageSets/
    │   ├── train.txt                ← 4,983 training image ids
    │   └── test.txt                 ← 2,135 test image ids
    └── Images/
        ├── img_dir/
        │   ├── train/               ← 4,983 RGB food images
        │   └── test/                ← 2,135 RGB food images
        └── ann_dir/
            ├── train/               ← training segmentation masks
            └── test/                ← test segmentation masks
```

---

## Task 2 — Multimodal Idiomaticity (coursework subset, derived from AdMIRe)

The underlying dataset is **AdMIRe — SemEval-2025 Task 1: Advancing Multimodal Idiomaticity Representation**, which is publicly available:

- **License:** CC-BY-4.0.
- **Source (University of Sheffield Figshare):** DOI `10.15131/shef.data.28436600.v1` — https://orda.shef.ac.uk/articles/dataset/28436600
- **Task website:** https://semeval2025-task1.github.io/
- **Source paper:** Pickard, Villavicencio, Mi et al., *SemEval-2025 Task 1: AdMIRe — Advancing Multimodal Idiomaticity Representation*, SemEval-2025.

**Important — this repo's version is a modified coursework subset, not the raw public release.** The version here was provided by the CSC8645 module at Newcastle University (see `CSC8645-Assignment-S26.pdf`). It differs from the original AdMIRe release in two ways:

1. **Three candidate images per item** (the original AdMIRe Subtask A uses five), and
2. **A flattened CSV layout** with 231 train / 27 val / 27 test rows — i.e. **77 / 9 / 9 grouped samples** (3 rows per sample).

If you want the full original data, use the Figshare/Task-website links above. To reproduce *this notebook's results*, use the subset shipped here.

**Folder structure:**

```
Task+2+NLP+Dataset/
├── train/
│   ├── train.csv                    ← 231 rows (77 samples × 3 candidates)
│   └── images/                      ← 231 candidate images
├── val/
│   ├── val.csv                      ← 27 rows (9 samples × 3 candidates)
│   └── images/                      ← 27 candidate images
└── test/
    ├── test.csv                     ← 27 rows (9 samples × 3 candidates)
    └── images/                      ← 27 candidate images
```

**CSV schema** (`compound, sentence_type, sentence, image_name, image_caption, label`):

| Column | Meaning |
|---|---|
| `compound` | the nominal compound, e.g. *"bad apple"* |
| `sentence_type` | gold idiomaticity label: `idiomatic` or `literal` |
| `sentence` | context sentence in which the compound appears |
| `image_name` | filename of the candidate image (in the matching `images/` folder) |
| `image_caption` | text caption describing the candidate image |
| `label` | whether this candidate is the correct match |

---

## Coursework brief

`CSC8645-Assignment-S26.pdf` contains the official assignment instructions for both tasks (problem definitions, deliverables, and the compulsory assessed demo-day requirement) as set by the module at Newcastle University.

---

## Note on storing data in Git

Large image datasets normally should **not** be committed to a Git repository. For a public/portfolio version, exclude this directory via `.gitignore` and have users download the data from the public sources linked above (FoodSeg103) or request the coursework subset (Task 2). Keeping the data out of Git keeps the repository small, clonable, and recruiter-friendly.
