# DIP Image Compression Benchmark

### Rate-Distortion Benchmarking Suite: Classical vs. Learned Image Codecs

**Course:** CSE 681 Digital Image Processing & Video Processing  
**Project:** 30  
**Team:** 3 students  
**Duration:** 9 weeks  
**Status:** Milestone 2 in progress (Milestone 1 submitted)

## Overview

This project develops a **reproducible benchmarking pipeline** for comparing traditional image compression formats with pretrained neural image compression models. The goal is to understand the trade-offs between **compressed bitrate, reconstructed image quality, and computational cost** under one consistent evaluation protocol.

The benchmark uses shared lossless test images, multiple operating points per codec, standard image-quality metrics, and documented software and hardware settings. The project focuses on **fair comparison and reproducibility**, not on training a new compression model.

## Compression Methods

| Category | Codecs / models | Quality levels |
|---|---|---|
| Classical | JPEG, JPEG 2000, WebP, AVIF | quality grid covering about 0.075 to 1.5 bpp |
| Learned | Factorized Prior (`bmshj2018_factorized`) | 1 to 8 |
| Learned | Scale Hyperprior (`bmshj2018_hyperprior`) | 1 to 8 |
| Learned | Cheng2020 Anchor (`cheng2020_anchor`) | 1 to 6 |

Learned models are loaded as pretrained MSE-optimized checkpoints from [CompressAI](https://github.com/InterDigitalInc/CompressAI) 1.2.8.

## Datasets

| Dataset | Images used | Source | License |
|---|---|---|---|
| Kodak | all 24 images, 768x512 | [r0k.us/graphics/kodak](https://r0k.us/graphics/kodak/) | no formal license; released by Kodak for unrestricted usage |
| Tecnick (TESTIMAGES) | standard subset of 100 RGB images, 1200x1200 (selection in progress) | [TESTIMAGES on SourceForge](https://sourceforge.net/projects/testimages/files/OLD/OLD_SAMPLING/) | CC BY-NC-SA 4.0 |
| CLIC 2025 validation | all 32 images | [CLIC 2025 tasks](https://archive.compression.cc/2025/tasks/) | Unsplash license |

Images are not committed to this repository. The notebooks download them, check every file, and record dimensions, format, colour mode, file size and SHA-256 checksums in `reports/`.

## Evaluation Metrics

- **Bits per pixel (bpp):** `(compressed_bytes x 8) / (width x height)`, using the original image size.
- **PSNR:** pixel-wise reconstruction fidelity in dB; higher is better.
- **SSIM:** structural similarity; higher is better.
- **MS-SSIM:** structural similarity across multiple scales; higher is better.
- **Encoding and decoding time:** measured on the same CPU runtime for all codecs.

## Evaluation Protocol

1. **Same inputs:** every codec receives the same lossless 8-bit RGB image, and every reconstruction keeps the original size.
2. **Padding:** for learned codecs, images are zero-padded symmetrically to a multiple of 64 and cropped back after decoding.
3. **Rate:** bpp is computed from the real compressed file or entropy-coded bitstream, not from estimated likelihoods. Payload-only and total (payload plus header) bpp are recorded separately.
4. **Distortion:** learned-codec outputs are rounded to 8-bit values before evaluation, and the same metric function is used for all codecs.
5. **Averaging:** metrics are computed per image and averaged over each dataset for every codec and quality level.
6. **BD-rate:** computed against JPEG, only over the overlapping bitrate range.
7. **Timing:** CPU only, one warm-up run before timing, median reported.
8. **Reproducibility:** fixed random seed (42), pinned package versions, recorded encoder settings.

## Repository Layout

```text
DIP-Image-Compression-Benchmark/
├── notebooks/
│   ├── Dataset_Preparation_kodak.ipynb                  (M1, Kodak download and checks)
│   ├── Dataset_Preparation_milestone1.ipynb             (M1, Kodak, Tecnick and CLIC preparation)
│   ├── AI_Compression_try.ipynb                         (M1, first test of the learned codecs)
│   ├── Evaluation_try.ipynb                             (M1, first metrics and JPEG test)
│   ├── AI_Compression_&__Evaluation_milestone_1.ipynb   (M1, model loading, preprocessing, metrics)
│   └── M2_StudentB_Learned_Codecs.ipynb                 (M2, learned codecs on all Kodak images)
├── references/
│   └── references.bib
├── reports/
│   ├── dataset_information_kodak.csv
│   ├── dataset_summary.csv
│   ├── validation_summary.csv
│   └── environment.csv
├── results/
│   ├── AI_compression/      (M1 single-image test)
│   ├── evaluation/          (M1 JPEG and metric tests)
│   └── M2_learned/          (M2 learned-codec results on Kodak)
├── .gitignore
├── README.md
└── README_Milestone_1.md
```

## Getting Started (Google Colab)

1. Open a notebook from this repository in Google Colab and select the CPU runtime.
2. Run the installation cell, then restart the session (Runtime, Restart session).
3. Run the rest of the notebook with Runtime, Run cell and below.

Main dependencies:

```python
%pip install "compressai==1.2.8" pytorch-msssim
%pip install pillow numpy pandas matplotlib
```

**Note:** Colab now uses Python 3.13, while CompressAI 1.2.8 asks for NumPy older than 2.0. The installation cell of `M2_StudentB_Learned_Codecs.ipynb` handles this case. Colab files are temporary, so results are saved to Google Drive.

## Development Roadmap

| Milestone | Due | Weight | Scope |
|---|---|---:|---|
| M1 Proposal and literature review | Week 3 | 15% | Dataset preparation, environment setup, model loading, preprocessing, evaluation functions, protocol |
| M2 Implementation and preliminary results | Week 6 | 35% | Classical and learned codecs on Kodak at all quality levels, check against published CompressAI results, first comparison table |
| M3 Final report and presentation | Week 9 | 50% | All datasets, RD curves, BD-rate, timing, ablations, final report, repository and demo |

### Current status

- **Milestone 1 (done):** dataset download and validation for Kodak and CLIC with SHA-256 checksums, environment setup, model loading, preprocessing and metric functions, single-image feasibility test.
- **Milestone 2 (in progress):** the three learned codecs are being run on all 24 Kodak images at every quality level and compared with the results published by CompressAI. Classical codec results and the selection of the 100-image Tecnick subset are in progress. Results are added to `results/` after they are verified.

## Team

| Member | Role | Responsibility |
|---|---|---|
| Elham Ehab | Student A in M1, Student B in M2 | Datasets and literature review (M1); learned codecs on Kodak (M2) |
| Roqaia Khalid | Student B in M1, Student A in M2 | CompressAI setup and first tests (M1); classical codecs and Tecnick subset (M2) |
| Shimaa Sherif | Student C | Evaluation metrics, protocol and codec comparisons |

## References

The full bibliography is in [`references/references.bib`](references/references.bib). Main references:

1. Ballé, Laparra and Simoncelli, [End-to-End Optimized Image Compression](https://arxiv.org/abs/1611.01704), ICLR 2017
2. Ballé et al., [Variational Image Compression with a Scale Hyperprior](https://arxiv.org/abs/1802.01436), ICLR 2018
3. Minnen, Ballé and Toderici, [Joint Autoregressive and Hierarchical Priors for Learned Image Compression](https://arxiv.org/abs/1809.02736), NeurIPS 2018
4. Cheng et al., [Learned Image Compression with Discretized Gaussian Mixture Likelihoods and Attention Modules](https://arxiv.org/abs/2001.01568), CVPR 2020
5. Bégaint et al., [CompressAI](https://github.com/InterDigitalInc/CompressAI), 2020
6. Tatwawadi et al., [What Matters in Practical Learned Image Compression](https://arxiv.org/abs/2605.05148), CVPR 2026
7. [CLIC 2025 Challenge on Learned Image Compression](https://archive.compression.cc/2025/tasks/)

---

*Academic project for the CSE 681 Digital Image Processing course. Results will be updated as milestones are completed.*
