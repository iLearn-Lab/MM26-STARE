<div align="center">

# STARE

### Spatially-Calibrated Token-to-Region Representation for Language-Guided Drone Geolocalization

**ACM Multimedia 2026 (MM '26)**

Rongqiang Liu · Xiang Deng<sup>*</sup> · Jianqiang Xiao · Weili Guan<sup>*</sup>

Harbin Institute of Technology (Shenzhen)

<sup>*</sup> Corresponding authors

[Paper](https://doi.org/10.1145/3767308.3836505) · [Results](#main-results) · [Citation](#citation) · [Contact](#contact)

</div>

Official repository for **Spatially-Calibrated Token-to-Region Representation for Language-Guided Drone Geolocalization**.

> **Code coming soon.** This release provides the paper introduction, experimental results, and citation. Source code, installation instructions, data preparation scripts, and model checkpoints will be added with the implementation release.

## Overview

Natural language-guided drone geolocalization identifies the location depicted in a drone-view image from a free-form textual description. Descriptions often mention several objects, attributes, and spatial relationships, which require fine-grained correspondences between textual elements and visual regions.

**STARE** (**S**pa**T**ially-calibr**A**ted token-to-**RE**gion representation) constructs a compact retrieval embedding from multiple text-grounded visual features. It contains two core components:

- **Token-to-Region Grounding (TRG):** derives multiple aggregated tokens from the description, lets them independently attend to image patches, and composes their region-specific visual evidence into a text-conditioned retrieval representation.
- **Region-Anchored Spatial Calibration (RASC):** uses bounding-box annotations as explicit spatial anchors during training. It aligns predicted grounded features with region-level visual references, calibrating the same features that form the final retrieval embedding.

The visual encoder uses a frozen **DINOv3 ViT-B/16** backbone followed by two trainable Transformer blocks; the text encoder is based on **CLIP**. At inference, TRG produces a text-conditioned image embedding for each image-text pair, which is scored against the corresponding global text embedding using cosine similarity.

## Highlights

- **Fine-grained grounding:** multiple textual cues retrieve visual evidence from different image regions.
- **Direct spatial supervision:** region annotations calibrate the grounded features used for retrieval.
- **Drone geolocalization:** 30.2% I2T R@1 and 14.7% T2I R@1 on GeoText-1652, with 165M parameters.
- **Efficiency:** the paper reports 16× faster training and 11× faster inference than the GeoText-1652 baseline under identical hardware conditions.
- **General retrieval:** evaluation includes standard, fine-grained, and long-text zero-shot retrieval under separate 3M and 12M pretraining settings.

## Main Results

All results below are transcribed from the paper's LaTeX tables; they are paper-reported results rather than a new reproduction run. **T2I** means text-to-image retrieval, **I2T** means image-to-text retrieval, and Recall values are percentages. A dash (`-`) indicates a result not reported in the source table.

### 1. Drone geolocalization on GeoText-1652

The following comparison preserves the zero-shot and fine-tuned settings from Table 1. The STARE row belongs to the **fine-tuned** setting.

| Setting | Method | Params | T2I R@1 | T2I R@5 | T2I R@10 | I2T R@1 | I2T R@5 | I2T R@10 |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| Zero-shot | UNITER | 300M | 0.9 | 2.7 | 4.2 | 2.5 | 7.4 | 11.8 |
| Zero-shot | METER-Swin | 380M | 1.3 | 3.9 | 5.8 | 2.7 | 8.0 | 12.2 |
| Zero-shot | ALBEF | 210M | 1.1 | 3.5 | 5.3 | 3.0 | 9.1 | 14.2 |
| Zero-shot | XVLM | 216M | 4.5 | 9.9 | 13.4 | 5.0 | 14.4 | 21.4 |
| Fine-tuned | UNITER | 300M | 10.6 | 20.4 | 26.1 | 21.4 | 43.3 | 59.5 |
| Fine-tuned | METER-Swin | 380M | 11.3 | 21.5 | 27.3 | 22.7 | 46.3 | 60.7 |
| Fine-tuned | ALBEF | 210M | 12.5 | 22.8 | 28.5 | 23.2 | 49.7 | 62.4 |
| Fine-tuned | XVLM | 216M | 13.2 | 23.7 | 29.6 | 25.0 | 52.3 | 65.1 |
| Fine-tuned | GeoText-1652 | 217M | 13.6 | 24.6 | 31.2 | 26.3 | 53.7 | 66.9 |
| Fine-tuned | HCCM | 216M | 14.7 | 26.0 | 32.5 | 28.8 | 57.3 | 69.9 |
| Fine-tuned | **STARE (Ours)** | 165M | 14.7 | 26.4 | 33.4 | 30.2 | 59.1 | 71.6 |

STARE improves I2T R@1 over GeoText-1652 by **3.9 percentage points** and T2I R@1 by **1.1 percentage points**. Its T2I R@1 is tied with HCCM at 14.7%.

### 2. Track 4 text-to-image retrieval challenge

Results from Table 2. Both Phase I and Phase II report **T2I** retrieval; the two column groups are competition phases.

| Method | Phase I T2I R@1 | Phase I T2I R@5 | Phase I T2I R@10 | Phase II T2I R@1 | Phase II T2I R@5 | Phase II T2I R@10 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| GeoText-1652 | 29.38 | 45.78 | 54.0 | 26.2 | 41.4 | 49.65 |
| **STARE (Ours)** | 30.64 | 52.81 | 60.25 | 28.21 | 45.17 | 52.37 |

### 3. Standard and fine-grained zero-shot retrieval

Results from Table 3. The two pretraining settings use **CC3M-recap** and **CC12M-recap**, respectively. These models are evaluated directly without task-specific fine-tuning on the retrieval benchmarks.

| Pretraining data | Benchmark | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | --- | ---: | ---: | ---: | ---: |
| CC3M-recap | MSCOCO | 46.5 | 72.3 | 59.8 | 82.5 |
| CC3M-recap | Flickr30k | 74.7 | 92.1 | 89.5 | 98.4 |
| CC3M-recap | DOCCI-FG | 20.9 | 39.2 | 46.9 | 74.4 |
| CC3M-recap | IIW-FG | 37.7 | 59.4 | 81.9 | 96.1 |
| CC12M-recap | MSCOCO | 50.6 | 75.9 | 64.3 | 85.9 |
| CC12M-recap | Flickr30k | 77.6 | 94.0 | 90.0 | 98.5 |
| CC12M-recap | DOCCI-FG | 23.6 | 42.3 | 53.0 | 79.5 |
| CC12M-recap | IIW-FG | 40.1 | 62.4 | 86.5 | 98.4 |

<details>
<summary>Full baseline comparisons from Table 3</summary>

The CC3M-recap and CC12M-recap groups contain the paper's comparisons under identical training configurations. The **External comparison** group reproduces the source table's separate “SOTA Comparison” group; data scales in parentheses belong to the individual comparison models. STARE (Ours) (12M) repeats the CC12M-recap model in that group.


#### MSCOCO

| Setting | Method | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | --- | ---: | ---: | ---: | ---: |
| CC3M-recap | CLIP | 27.0 | 52.6 | 38.9 | 66.1 |
| CC3M-recap | SigLIP | 28.3 | 54.4 | 40.1 | 67.5 |
| CC3M-recap | DreamLIP | 29.8 | 55.4 | 40.8 | 68.4 |
| CC3M-recap | FLAIR | 37.7 | 64.5 | 51.6 | 77.2 |
| CC3M-recap | COSMOS | 40.1 | 66.8 | 53.1 | 78.3 |
| CC3M-recap | **STARE (Ours)** | 46.5 | 72.3 | 59.8 | 82.5 |
| CC12M-recap | CLIP | 39.8 | 66.4 | 56.2 | 80.5 |
| CC12M-recap | SigLIP | 40.4 | 67.0 | 55.3 | 79.7 |
| CC12M-recap | DreamLIP | 40.6 | 66.5 | 54.0 | 78.3 |
| CC12M-recap | SAIL-B-GTE | 39.3 | - | 50.4 | - |
| CC12M-recap | SAIL-B-NV2 | 45.3 | - | 57.3 | - |
| CC12M-recap | FLAIR | 47.8 | 73.5 | 64.1 | 85.0 |
| CC12M-recap | COSMOS | 48.9 | 74.3 | 63.2 | 85.5 |
| CC12M-recap | **STARE (Ours)** | 50.6 | 75.9 | 64.3 | 85.9 |
| External comparison | OpenCLIP(2B) | 41.7 | 67.1 | 59.3 | 82.4 |
| External comparison | SigLIP(10B) | 47.2 | 72.1 | 65.5 | 86.2 |
| External comparison | MetaCLIP(2.5B) | 41.4 | 67.2 | 59.4 | 80.6 |
| External comparison | Llip(2.5B) | 45.6 | 70.8 | 63.4 | 84.3 |
| External comparison | DreamLIP(30M) | 44.8 | 69.8 | 62.3 | 84.5 |
| External comparison | FG-CLIP(1.6B) | 45.4 | - | 64.1 | - |
| External comparison | **STARE (Ours) (12M)** | 50.6 | 75.9 | 64.3 | 85.9 |


#### Flickr30k

| Setting | Method | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | --- | ---: | ---: | ---: | ---: |
| CC3M-recap | CLIP | 49.9 | 77.0 | 67.8 | 88.5 |
| CC3M-recap | SigLIP | 53.2 | 78.5 | 69.9 | 90.4 |
| CC3M-recap | DreamLIP | 53.6 | 78.4 | 69.2 | 91.5 |
| CC3M-recap | FLAIR | 65.7 | 86.8 | 78.7 | 95.2 |
| CC3M-recap | COSMOS | 68.6 | 88.7 | 84.1 | 96.2 |
| CC3M-recap | **STARE (Ours)** | 74.7 | 92.1 | 89.5 | 98.4 |
| CC12M-recap | CLIP | 67.0 | 87.7 | 81.7 | 96.5 |
| CC12M-recap | SigLIP | 77.7 | 88.1 | 82.5 | 96.1 |
| CC12M-recap | DreamLIP | 68.3 | 89.3 | 84.1 | 97.8 |
| CC12M-recap | SAIL-B-GTE | 68.9 | - | 80.8 | - |
| CC12M-recap | SAIL-B-NV2 | 73.0 | - | 84.9 | - |
| CC12M-recap | FLAIR | 75.4 | 92.2 | 90.8 | 98.4 |
| CC12M-recap | COSMOS | 77.2 | 92.8 | 91.4 | 98.2 |
| CC12M-recap | **STARE (Ours)** | 77.6 | 94.0 | 90.0 | 98.5 |
| External comparison | OpenCLIP(2B) | 71.9 | 90.4 | 87.5 | 97.7 |
| External comparison | SigLIP(10B) | 75.6 | 92.8 | 89.1 | 98.6 |
| External comparison | MetaCLIP(2.5B) | 76.2 | 90.7 | 85.9 | 97.3 |
| External comparison | Llip(2.5B) | 75.1 | 92.8 | 90.1 | 98.5 |
| External comparison | DreamLIP(30M) | 73.3 | 91.8 | 89.9 | 99.0 |
| External comparison | FG-CLIP(1.6B) | 76.4 | - | 90.7 | - |
| External comparison | **STARE (Ours) (12M)** | 77.6 | 94.0 | 90.0 | 98.5 |


#### DOCCI-FG

| Setting | Method | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | --- | ---: | ---: | ---: | ---: |
| CC3M-recap | CLIP | 10.3 | 23.4 | 25.0 | 50.9 |
| CC3M-recap | SigLIP | 10.4 | 23.8 | 24.9 | 50.6 |
| CC3M-recap | DreamLIP | 10.3 | 22.8 | 23.3 | 47.5 |
| CC3M-recap | FLAIR | 15.1 | 30.9 | 35.7 | 63.5 |
| CC3M-recap | COSMOS | - | - | - | - |
| CC3M-recap | **STARE (Ours)** | 20.9 | 39.2 | 46.9 | 74.4 |
| CC12M-recap | CLIP | 16.0 | 31.6 | 39.5 | 66.5 |
| CC12M-recap | SigLIP | 16.2 | 31.9 | 40.0 | 66.8 |
| CC12M-recap | DreamLIP | 17.2 | 33.0 | 41.6 | 68.5 |
| CC12M-recap | SAIL-B-GTE | - | - | - | - |
| CC12M-recap | SAIL-B-NV2 | - | - | - | - |
| CC12M-recap | FLAIR | 21.4 | 38.8 | 50.4 | 76.7 |
| CC12M-recap | COSMOS | - | - | - | - |
| CC12M-recap | **STARE (Ours)** | 23.6 | 42.3 | 53.0 | 79.5 |
| External comparison | OpenCLIP(2B) | 17.4 | 31.9 | 49.7 | 75.9 |
| External comparison | SigLIP(10B) | 20.6 | 35.9 | 57.5 | 82.1 |
| External comparison | MetaCLIP(2.5B) | - | - | - | - |
| External comparison | Llip(2.5B) | - | - | - | - |
| External comparison | DreamLIP(30M) | 21.6 | 39.3 | 51.2 | 78.3 |
| External comparison | FG-CLIP(1.6B) | - | - | - | - |
| External comparison | **STARE (Ours) (12M)** | 23.6 | 42.3 | 53.0 | 79.5 |


#### IIW-FG

| Setting | Method | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | --- | ---: | ---: | ---: | ---: |
| CC3M-recap | CLIP | 24.4 | 45.0 | 61.1 | 95.6 |
| CC3M-recap | SigLIP | 24.7 | 45.1 | 62.6 | 86.9 |
| CC3M-recap | DreamLIP | 22.7 | 41.6 | 59.2 | 83.3 |
| CC3M-recap | FLAIR | 30.5 | 52.3 | 70.6 | 90.9 |
| CC3M-recap | COSMOS | - | - | - | - |
| CC3M-recap | **STARE (Ours)** | 37.7 | 59.4 | 81.9 | 96.1 |
| CC12M-recap | CLIP | 31.8 | 52.9 | 76.4 | 93.6 |
| CC12M-recap | SigLIP | 31.9 | 53.2 | 78.4 | 94.3 |
| CC12M-recap | DreamLIP | 31.9 | 52.1 | 77.8 | 94.9 |
| CC12M-recap | SAIL-B-GTE | - | - | - | - |
| CC12M-recap | SAIL-B-NV2 | - | - | - | - |
| CC12M-recap | FLAIR | 38.7 | 59.9 | 83.8 | 96.9 |
| CC12M-recap | COSMOS | - | - | - | - |
| CC12M-recap | **STARE (Ours)** | 40.1 | 62.4 | 86.5 | 98.4 |
| External comparison | OpenCLIP(2B) | 30.6 | 48.4 | 84.1 | 95.4 |
| External comparison | SigLIP(10B) | 33.8 | 53.0 | 83.7 | 97.7 |
| External comparison | MetaCLIP(2.5B) | - | - | - | - |
| External comparison | Llip(2.5B) | - | - | - | - |
| External comparison | DreamLIP(30M) | 37.5 | 58.6 | 85.3 | 97.4 |
| External comparison | FG-CLIP(1.6B) | - | - | - | - |
| External comparison | **STARE (Ours) (12M)** | 40.1 | 62.4 | 86.5 | 98.4 |


</details>

### 4. Long-text zero-shot retrieval

Results from Table 4. **Every value in this table is R@1.** Within each benchmark, columns follow the source order: **I2T first, T2I second**. SV-1k and SV-10k denote the 1k and 10k subsets of ShareGPT-4V. The pretraining data column retains the sizes reported in the paper.

| Method | Pretraining data | DCI I2T | DCI T2I | SV-1k I2T | SV-1k T2I | SV-10k I2T | SV-10k T2I | Urban-1k I2T | Urban-1k T2I |
| --- | --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| OpenCLIP | 2B | 56.0 | 55.4 | 90.3 | 87.7 | 69.6 | 66.8 | 69.5 | 65.8 |
| LIP | 100M | 41.7 | 40.9 | 86.0 | 80.0 | 61.4 | 50.6 | - | - |
| ALIGN | 700M | 56.5 | 57.4 | 86.3 | 85.3 | 65.1 | 62.7 | - | - |
| SigLIP | 10B | 57.7 | 56.2 | 85.8 | 83.4 | 83.4 | 63.0 | 62.7 | 62.1 |
| Long-CLIP | 400M | 47.4 | 44.1 | 90.6 | 87.4 | 73.1 | 62.0 | 78.9 | 79.5 |
| LoTLIP | 100M | 62.1 | 61.0 | 95.5 | 86.8 | 81.4 | 83.7 | - | - |
| FG-CLIP | 1.6B | 61.8 | 60.6 | 96.7 | 94.9 | - | - | - | - |
| FLAIR | 3M | 47.3 | 50.5 | 91.0 | 89.7 | 72.0 | 70.6 | 63.5 | 69.5 |
| **STARE (Ours)** | 3M | 56.9 | 61.4 | 94.0 | 92.6 | 78.7 | 77.6 | 72.8 | 77.7 |
| FLAIR | 12M | 55.5 | 60.8 | 96.1 | 95.1 | 85.0 | 83.4 | 74.6 | 80.6 |
| **STARE (Ours)** | 12M | 60.0 | 63.0 | 96.1 | 95.8 | 86.1 | 85.4 | 75.6 | 82.0 |

At 12M, STARE and FLAIR tie on SV-1k I2T R@1 (96.1%); STARE improves T2I R@1 on that benchmark from 95.1% to 95.8%.

### 5. Component ablations on GeoText-1652

Results from Table 5. The two backbone groups are separate ablation sequences. TRG and RASC are added incrementally within each group.

| Vision backbone | TRG | RASC | T2I R@1 | T2I R@5 | T2I R@10 | I2T R@1 | I2T R@5 | I2T R@10 |
| --- | --- | --- | ---: | ---: | ---: | ---: | ---: | ---: |
| OpenCLIP ViT-B/16 | No | No | 12.8 | 22.7 | 28.5 | 25.1 | 52.7 | 65.9 |
| OpenCLIP ViT-B/16 | Yes | No | 14.1 | 24.5 | 30.9 | 28.4 | 56.9 | 69.8 |
| OpenCLIP ViT-B/16 | Yes | Yes | 15.2 | 25.8 | 31.9 | 29.5 | 58.4 | 71.2 |
| DINOv3 ViT-B/16 | No | No | 13.8 | 25.4 | 32.5 | 27.9 | 55.9 | 68.5 |
| DINOv3 ViT-B/16 | Yes | No | 14.4 | 26.3 | 33.6 | 29.0 | 57.9 | 70.7 |
| DINOv3 ViT-B/16 | Yes | Yes | 14.7 | 26.4 | 33.4 | 30.2 | 59.1 | 71.6 |

## Release Scope

| Artifact | Status |
| --- | --- |
| Paper introduction and experimental results | Available |
| Bibliographic information | Available in [citation.bib](citation.bib) |
| Source code and configuration files | Coming soon |
| Environment and installation instructions | Coming soon |
| Data preparation scripts | Coming soon |
| Training, evaluation, and inference commands | Coming soon |
| Pretrained and fine-tuned checkpoints | Coming soon |

## Environment Installation

Coming soon. Dependency versions and installation commands will accompany the implementation release.

<details>
<summary>Implementation settings reported in the paper</summary>

| Setting | Value |
| --- | --- |
| Vision backbone | Frozen DINOv3 ViT-B/16 |
| Visual adaptation | Two trainable Transformer blocks |
| Text encoder | OpenAI CLIP text encoder |
| Input image resolution | 224 × 224 for training and inference |
| Aggregated text tokens (m) | 8 |
| Learnable pooling queries (Nq) | 4 |
| Learning rate | 1 × 10<sup>-4</sup> |
| Batch size | 1,024 |
| Training hardware | A800 GPUs |

These settings are a summary of the paper and do not constitute a complete installation or reproduction configuration.

</details>

## Data Preparation

- **Pretraining:** [DreamLIP](https://github.com/ant-research/DreamLIP) re-captioned datasets, CC3M-recap and CC12M-recap, used in separate experimental settings.
- **Drone geolocalization fine-tuning:** [GeoText-1652](https://github.com/MultimodalGeo/GeoText-1652), which augments University-1652 with region-level bounding boxes and textual descriptions. The paper follows the official splits and evaluation protocols.
- **Standard zero-shot evaluation:** MSCOCO and Flickr30k.
- **Fine-grained zero-shot evaluation:** DOCCI-FG and IIW-FG.
- **Long-text zero-shot evaluation:** DCI, ShareGPT-4V 1k/10k, and Urban-1k.

Refer to the original providers for data acquisition and usage terms. STARE-specific directory layouts, annotation processing, and preparation scripts are coming soon. Datasets are not redistributed in this release.

## Training, Evaluation, and Inference

Coming soon. Training configurations, checkpoint loading, and runnable commands will be documented alongside the code release.

The paper describes pretraining followed by GeoText-1652 fine-tuning for drone geolocalization, and separately evaluates pretrained models on general retrieval benchmarks. RASC uses bounding-box annotations during training. At inference, retrieval scores are computed from the TRG-grounded image representation and global text embedding; the T2I score matrix is the transpose of the I2T score matrix.

## Model Weights

Coming soon. Pretrained and fine-tuned checkpoints are not included in this release. Download links, model configurations, evaluation details, and applicable upstream model terms will be provided with the weights.

## Citation

If you find this work useful, please cite:

```bibtex
@inproceedings{liu2026stare,
  author = {Liu, Rongqiang and Deng, Xiang and Xiao, Jianqiang and Guan, Weili},
  title = {Spatially-Calibrated Token-to-Region Representation for Language-Guided Drone Geolocalization},
  booktitle = {Proceedings of the 34th ACM International Conference on Multimedia},
  series = {MM '26},
  year = {2026},
  publisher = {ACM},
  address = {New York, NY, USA},
  location = {Rio de Janeiro, Brazil},
  isbn = {979-8-4007-2213-4},
  doi = {10.1145/3767308.3836505},
  url = {https://doi.org/10.1145/3767308.3836505},
  numpages = {10}
}
```

See also [citation.bib](citation.bib). The paper link and bibliographic information follow the camera-ready metadata; availability of the DOI landing page depends on ACM's publication schedule.

## License

The original repository documentation is licensed under the **Apache License 2.0**; see [LICENSE](LICENSE).

The paper carries a separate **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)** notice. Third-party datasets, pretrained models, and other external materials remain subject to their respective licenses and terms.

## Contact

For questions about the paper, please open an issue in this repository or contact **Rongqiang Liu** at [liurongqiang.xii@gmail.com](mailto:liurongqiang.xii@gmail.com).
