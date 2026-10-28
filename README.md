<div align="center">

# STARE

### Spatially-Calibrated Token-to-Region Representation for Language-Guided Drone Geolocalization

**ACM Multimedia 2026 (MM '26)**

Rongqiang Liu · Xiang Deng<sup>*</sup> · Jianqiang Xiao · Weili Guan<sup>*</sup>

Harbin Institute of Technology (Shenzhen)

<sup>*</sup> Corresponding authors

[Paper (DOI)](https://doi.org/10.1145/3767308.3836505) · [Citation](#citation) · [Contact](#contact)

</div>

> **Release status: paper introduction only.** This repository currently provides the method overview, paper-reported results, and citation information. Code, environment specifications, data preparation instructions, and model weights are coming soon.
>
> The DOI link is transcribed from the camera-ready paper. Online availability may depend on ACM's publication schedule.

## Overview

STARE addresses language-guided drone geolocalization by matching a textual description to a drone-view image. A description can mention several objects and spatial relationships, so a useful representation needs to retain evidence from multiple image regions.

The framework combines a frozen DINOv3 vision backbone with a CLIP text encoder. **Token-to-Region Grounding (TRG)** lets multiple aggregated text tokens attend to image patches and pools the resulting visual evidence into a compact, text-conditioned retrieval embedding. During training, **Region-Anchored Spatial Calibration (RASC)** uses annotated bounding boxes to calibrate these grounded features against region-level visual references. Retrieval scores compare the grounded image representation for each candidate text with that text's embedding.

## Highlights

- **Multiple grounding cues:** TRG uses multiple text-derived tokens to gather visual evidence from different regions of an image.
- **Spatial calibration:** RASC combines region-text alignment and feature consistency to supervise the same grounding mechanism used for retrieval.
- **Drone geolocalization:** STARE achieves **30.2% image-to-text** and **14.7% text-to-image Recall@1** on GeoText-1652 with 165M parameters.
- **Efficiency and transfer:** The paper reports **16× faster training** and **11× faster inference** than the GeoText-1652 baseline under identical hardware conditions, and evaluates transfer to standard, fine-grained, and long-caption retrieval benchmarks.

## Main Results

All numbers below are reported in the paper. Recall values are percentages.

### Fine-tuned retrieval on GeoText-1652

| Retrieval direction | R@1 | R@5 | R@10 |
| --- | ---: | ---: | ---: |
| Text-to-image (T2I) | 14.7 | 26.4 | 33.4 |
| Image-to-text (I2T) | 30.2 | 59.1 | 71.6 |

### Zero-shot retrieval with 12M pretraining

The following results use the model pretrained on **CC12M-recap**, without task-specific fine-tuning on these evaluation benchmarks.

| Benchmark | T2I R@1 | T2I R@5 | I2T R@1 | I2T R@5 |
| --- | ---: | ---: | ---: | ---: |
| MSCOCO | 50.6 | 75.9 | 64.3 | 85.9 |
| Flickr30k | 77.6 | 94.0 | 90.0 | 98.5 |
| DOCCI-FG | 23.6 | 42.3 | 53.0 | 79.5 |
| IIW-FG | 40.1 | 62.4 | 86.5 | 98.4 |

## Release Scope

| Item | Status |
| --- | --- |
| Method introduction and paper-reported results | Available |
| Bibliographic information | Available in [citation.bib](citation.bib) |
| Training, evaluation, and inference implementation | Coming soon |
| Environment and installation instructions | Coming soon |
| Data preparation scripts and configuration files | Coming soon |
| Pretrained and fine-tuned model weights | Coming soon |

This release contains documentation only. The reported results describe the paper's experiments; this repository does not yet provide an executable reproduction package.

## Environment Installation

Installation instructions and dependency versions will accompany the code release. No environment or package versions are specified here because they cannot be established from the paper alone.

The paper reports the following experimental settings:

| Setting | Reported value |
| --- | --- |
| Vision backbone | Frozen pretrained DINOv3 ViT-B/16 |
| Visual adaptation | Two trainable Transformer blocks |
| Text encoder | OpenAI CLIP text encoder |
| Image resolution | 224 × 224 for training and inference |
| Aggregated text tokens | 8 |
| Learnable pooling queries | 4 |
| Learning rate | 1 × 10<sup>−4</sup> |
| Batch size | 1,024 |
| Training hardware | A800 GPUs |

The paper does not specify the exact CLIP text checkpoint, optimizer, training duration, loss weights, or complete software environment. These details will need to be documented with the implementation before exact reproduction is possible.

## Data Preparation

The paper uses [DreamLIP](https://github.com/ant-research/DreamLIP)'s **CC3M-recap** and **CC12M-recap** for separate pretraining settings, followed by fine-tuning on [GeoText-1652](https://github.com/MultimodalGeo/GeoText-1652). GeoText-1652 augments University-1652 with region-level bounding boxes and textual descriptions. Its official splits and evaluation protocols are used in the paper. Refer to the original authors' repositories for dataset acquisition information.

Standard and fine-grained zero-shot evaluation covers **MSCOCO**, **Flickr30k**, **DOCCI-FG**, and **IIW-FG**. The paper also evaluates long-caption retrieval on **DCI**, **ShareGPT-4V 1k/10k**, and **Urban-1k**.

Download references, expected directory layouts, annotation processing, and preparation commands are coming soon. Datasets are not included in this release; obtain and use them under their respective providers' terms.

## Training, Evaluation, and Inference

Implementation and runnable commands are coming soon. The paper describes a pretraining and GeoText-1652 fine-tuning pipeline, with bounding-box annotations used by RASC during training.

At inference, TRG constructs a text-conditioned image embedding for each image-text pair. Its cosine similarity with the global text embedding determines the retrieval score. The text-to-image score matrix is the transpose of the image-to-text score matrix.

Training configurations, checkpoint loading, evaluation protocols, and inference examples will be documented alongside the corresponding code.

## Model Weights

Pretrained and fine-tuned model weights are coming soon. No checkpoint downloads are available in this release. Future checkpoint documentation should identify the training data, configuration, evaluation results, and applicable upstream model terms.

## Citation

If you use this work, please cite the paper. The bibliographic information below is transcribed from the camera-ready version; see also [citation.bib](citation.bib).

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

## License

The original repository documentation in this release is licensed under the **Apache License 2.0**; see [LICENSE](LICENSE).

The paper is separately licensed under **Creative Commons Attribution-NonCommercial-NoDerivatives 4.0 International (CC BY-NC-ND 4.0)**, as stated in its camera-ready version. Third-party datasets, pretrained models, and other external materials remain subject to their respective licenses and terms.

## Contact

For questions about this work, please open an issue in this repository or contact **Rongqiang Liu** at [liurongqiang.xii@gmail.com](mailto:liurongqiang.xii@gmail.com).
