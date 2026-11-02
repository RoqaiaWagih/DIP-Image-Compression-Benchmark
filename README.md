# DIP Image Compression Benchmark

### Rate–Distortion Benchmarking Suite: Classical vs. Learned Image Codecs

**Course:** Digital Image Processing  
**Project:** 30  
**Team:** 3 students  
**Duration:** 9 weeks  
**Status:** In development — Milestone 1 foundation

## Overview

This project develops a **reproducible benchmarking pipeline** for comparing traditional image compression formats with pretrained neural image compression models. The goal is to understand the trade-offs between **compressed bitrate, reconstructed image quality, and computational cost** under a consistent evaluation protocol.

The final benchmark will use shared lossless test images, multiple operating points per codec, standardized image-quality metrics, and documented software/hardware settings. The project emphasizes **fair comparison and reproducibility**, rather than training a new compression model.

## Compression Methods

| Category | Codecs / models |
|---|---|
| Classical | JPEG, JPEG2000, WebP, AVIF |
| Learned | Factorized Prior (`bmshj2018_factorized`) |
| Learned | Scale Hyperprior (`bmshj2018_hyperprior`) |
| Learned | Cheng2020 Anchor (`cheng2020_anchor`) |

Learned models are loaded from pretrained checkpoints through [CompressAI](https://github.com/InterDigitalInc/CompressAI). Availability of individual checkpoints and quality levels depends on the installed version.

## Datasets

| Dataset | Role | Source |
|---|---|---|
| Kodak Lossless True Color Image Suite | Development and initial testing (24 images) | [Kodak](https://r0k.us/graphics/kodak/) |
| Tecnick | High-resolution extended evaluation | [Tecnick](https://sourceforge.net/projects/testimages/files/) |
| CLIC validation | Final benchmark evaluation | [CLIC](https://archive.compression.cc/2025/tasks/) |

Dataset versions, subsets, redistribution permissions, and integrity checks will be recorded before final experiments. Large datasets are not intended to be committed to Git.

## Evaluation Metrics

- **Bits per pixel (bpp):** `(compressed_bytes × 8) / (width × height)`; lower means fewer compressed bits per source pixel.
- **PSNR:** pixel-wise reconstruction fidelity in dB; higher is better.
- **SSIM:** structural similarity; higher is better.
- **MS-SSIM:** structural similarity across multiple scales; higher is better.
- **Encoding and decoding time:** runtime measurements under recorded hardware and software conditions.

The final evaluation will measure **actual decodable bitstreams**, including necessary headers and metadata. Early payload-only measurements will be labeled separately.

## Benchmarking Workflow

1. **Prepare data:** acquire lossless images, verify integrity, and export metadata.
2. **Configure codecs:** record codec/library versions, model checkpoints, quality settings, and device.
3. **Compress:** encode each source image at several bitrate or quality settings.
4. **Reconstruct:** decode the saved bitstream and restore the original image dimensions.
5. **Evaluate:** calculate bpp, PSNR, SSIM, MS-SSIM, and encoding/decoding times.
6. **Compare:** create rate–distortion curves and tables at matched or interpolated bitrates.
7. **Report:** publish reproducible results, methodology, limitations, and conclusions.

### Fair-comparison principles

Use identical source images, consistent RGB reconstruction metrics, original spatial dimensions, actual stored bitstream sizes, multiple operating points, and a documented timing methodology. For learned codecs, padding/cropping and any decoding metadata must be handled consistently. CPU and GPU timings should be reported separately rather than treated as directly comparable.

## Repository Layout

```text
DIP-Image-Compression-Benchmark/
├── notebooks/
│   ├── Dataset_Preparation.ipynb
│   └── AI_Compression_&_Evaluation_milestone_1.ipynb
├── datasets/
│   └── README.md
├── reports/
│   └── evaluation_protocol.md
├── results/
│   ├── classical/
│   ├── learned/
│   └── comparisons/
├── .gitignore
└── README.md
```

*This is a target layout. Not all files or output directories are present yet. The notebook names above correspond to the current two-notebook workflow; rename them in the repository if needed.*

## Getting Started (Google Colab)

1. Open the notebooks from this repository in Google Colab.
2. Run `Dataset_Preparation.ipynb` to obtain/organize images and produce validation, metadata, and reproducibility reports.
3. Run `AI_Compression_&_Evaluation_milestone_1.ipynb` to configure CompressAI, load pretrained models, prepare tensors, and validate quality-metric functions.
4. For later milestones, run codec experiments and export result CSVs, bitstreams, and plots to persistent storage.

The notebooks contain their own installation cells. Typical dependencies include:

```python
%pip install pillow numpy pandas matplotlib scikit-image tqdm
%pip install pytorch-msssim
%pip install "compressai==1.2.8"
```

**Note:** Check the Colab Python/PyTorch runtime and CompressAI compatibility before installation. GPU use is optional for some tasks; timings must identify the execution device. Colab files are temporary unless exported to Drive or downloaded.

## Development Roadmap

| Milestone | Due | Weight | Scope |
|---|---|---:|---|
| M1 — Research & setup | Week 3 | 15% | Dataset preparation, environment setup, model loading, preprocessing, evaluation functions, experimental protocol |
| M2 — Implementation | Week 6 | 35% | Classical and learned codec encoding/decoding, multi-rate tests, preliminary comparisons |
| M3 — Final benchmark | Week 9 | 50% | Complete rate–distortion analysis, reproducibility checks, final report, repository, and demo |

### Current implementation

The current notebooks contain dataset preparation and validation workflows for Kodak, Tecnick, and CLIC; metadata and SHA-256 utilities; environment configuration; pretrained model loading and inspection; RGB-to-tensor preprocessing and padding; and bpp, PSNR, SSIM, and MS-SSIM evaluation functions. **End-to-end results for all seven codecs are not yet claimed.** Benchmark tables and plots will be published as experiments are run and verified.

## Expected Deliverables

- Reproducible Python/Colab benchmarking workflow
- Validated dataset inventory and metadata
- Codec settings and model checkpoint records
- Per-image and aggregate results in CSV format
- Rate–distortion curves and runtime comparisons
- Final technical report discussing trade-offs and limitations

## Team

| Role | Responsibility | Member |
|---|---|---|
| Dataset & classical codecs | Data validation, metadata, JPEG/JPEG2000/WebP/AVIF baselines | TBD |
| Learned codecs | CompressAI models, compression/decompression, timing | TBD |
| Evaluation & reporting | Metrics, fair protocol, RD plots, result analysis | TBD |

## References

1. [CompressAI — GitHub repository](https://github.com/InterDigitalInc/CompressAI)
2. [Ballé et al., End-to-End Optimized Image Compression (2017)](https://arxiv.org/abs/1611.01704)
3. [Ballé et al., Variational Image Compression with a Scale Hyperprior (2018)](https://arxiv.org/abs/1802.01436)
4. [Cheng et al., Learned Image Compression with Discretized Gaussian Mixture Likelihoods and Attention Modules (2020)](https://arxiv.org/abs/2001.01568)
5. [CLIC Image Compression Challenge](https://compression.cc/)

---

*Academic project for the Digital Image Processing course. Documentation and experimental results will be updated as milestones are completed.*
