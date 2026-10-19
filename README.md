# DIP-Image-Compression-Benchmark

# Digital Image Processing — Image Compression Benchmarking Suite

**Project 30: Rate–Distortion Benchmarking — Classical vs. Learned Image Codecs**

**Course:** Digital Image Processing  
**Team Size:** 3 Students  
**Project Duration:** 9 Weeks  
**Current Milestone:** Milestone 1 — Week 3  
**Milestone Weight:** 15%

---

## 1. Project Overview

This project aims to develop a reproducible image compression benchmarking system that compares traditional image compression algorithms with pretrained deep learning-based compression models.

The main objective is to evaluate compression efficiency, reconstructed image quality, and computational performance using standardized datasets and evaluation metrics.

### Classical Compression Algorithms
- JPEG
- JPEG2000
- WebP
- AVIF

### Learned Compression Models
- Factorized Prior (`bmshj2018_factorized`)
- Scale Hyperprior (`bmshj2018_hyperprior`)
- Cheng2020 Anchor (`cheng2020_anchor`)

The project uses Python, Google Colab, PyTorch, CompressAI, and GitHub.

---

## 2. Milestone 1 — Research, Setup & Experimental Protocol

**Deadline:** End of Week 3  
**Weight:** 15%

### Objectives

Milestone 1 establishes the technical foundation of the project through:

1. Preparing standard image datasets.
2. Configuring the Python and Google Colab environment.
3. Identifying and testing pretrained AI compression models.
4. Implementing preliminary image quality evaluation metrics.
5. Defining a reproducible experimental protocol.
6. Preparing the project proposal and research documentation.

Full codec benchmarking will be completed in subsequent milestones.

---

## 3. Team Responsibilities

### Dataset Preparation

**Responsibilities:**
- Download the Kodak Lossless Image Dataset (24 images).
- Identify official sources for Tecnick and CLIC validation datasets.
- Organize dataset directories.
- Load and display images using Python.
- Extract image dimensions, formats, color modes, and file sizes.
- Verify dataset integrity.

**Implementation:**

`notebooks/Student_A_Dataset_Preparation.ipynb`

**Outputs:**
- Kodak image collection
- Dataset metadata CSV
- Image verification results
- Dataset documentation

### Learned Image Compression

**Responsibilities:**
- Configure PyTorch and CompressAI.
- Identify three pretrained compression architectures.
- Load pretrained model weights.
- Perform image compression and decompression.
- Generate reconstructed images.
- Calculate preliminary PSNR and compressed payload bitrate.
- Record compression and decompression times.

**Models:**
1. Factorized Prior (2018)
2. Scale Hyperprior (2018)
3. Cheng2020 Anchor (2020)

**Implementation:**

`notebooks/Student_B_AI_Compression.ipynb`

**Outputs:**
- Reconstructed images
- Preliminary model comparison CSV
- Encoding and decoding measurements

### Evaluation & Benchmarking Protocol

**Responsibilities:**
- Implement PSNR, SSIM, and MS-SSIM calculations.
- Implement bits-per-pixel calculations.
- Evaluate original and reconstructed images.
- Generate preliminary rate–distortion graphs.
- Define a common benchmarking protocol.
- Establish requirements for fair codec comparisons.

**Implementation:**

`notebooks/Student_C_Evaluation.ipynb`

**Outputs:**
- Image quality evaluation results
- Preliminary JPEG rate–distortion curve
- Evaluation methodology documentation

---

## 4. Dataset Information

| Dataset | Description | Usage |
|---|---|---|
| Kodak | 24 lossless reference images | Initial development and testing |
| Tecnick | High-resolution test image collection | Extended evaluation |
| CLIC Validation | Challenge image compression dataset | Final benchmarking |

### Dataset Sources

**Kodak:**  
https://r0k.us/graphics/kodak/

**Tecnick:**  
https://sourceforge.net/projects/testimages/files/

**CLIC:**  
https://archive.compression.cc/2025/tasks/

Dataset versions, selected subsets, and licensing requirements will be documented before final experiments.

---

## 5. Project Repository Structure

```text
DIP-Image-Compression-Benchmark/
│
├── notebooks/
│   ├── Dataset_Preparation.ipynb
│   ├── AI_Compression.ipynb
│   └── Evaluation.ipynb
│
├── datasets/
│   └── README.md
│
├── reports/
│   ├── dataset_information.csv
│   └── evaluation_protocol.md
│
├── results/
│   ├── AI_c0mpression
│   │   ├── student_b_models_comparison.csv
│   │   ├── kodim01_AI_reconstructed.png
│   │   ├── scale_hyperprior.png
│   │   └── cheng2020_anchor.png
│   │
│   └── evaluation
│       ├── jpeg_rd_results.csv
│       ├── ai_quality_results.csv
│       └── jpeg_rd_curve.png
│
├── .gitignore
└── README.md
```

*The structure above represents the intended repository organization. Files should be added after successful execution and verification.*

---

## 6. Evaluation Metrics

### Bits Per Pixel (bpp)

Measures the number of compressed bits used per pixel.

**Formula:**

bpp = (Compressed Size in Bytes × 8) / (Image Width × Image Height)

Lower bpp indicates a lower bitrate.

### Peak Signal-to-Noise Ratio (PSNR)

Measures reconstruction fidelity based on mean squared error.

PSNR = 10 × log₁₀(MAX² / MSE)

Higher PSNR generally indicates better numerical reconstruction quality.

### Structural Similarity Index (SSIM)

Measures image similarity using structural information, luminance, and contrast.

Values closer to 1 indicate greater similarity.

### Multi-Scale SSIM (MS-SSIM)

Extends structural similarity evaluation across multiple spatial scales.

### Computational Performance

Encoding and decoding times will be measured to assess compression efficiency and practical performance.

---

## 7. Experimental Methodology

The benchmark follows these steps:

1. Load original lossless images.
2. Compress images using classical or learned codecs.
3. Store compressed bitstreams and required decoding metadata.
4. Decompress and reconstruct the images.
5. Calculate bpp, PSNR, SSIM, and MS-SSIM.
6. Record encoding and decoding times.
7. Compare different codecs at matched or interpolated bitrate levels.
8. Generate rate–distortion curves and comparison tables.

### Fair Comparison Rules

- Use identical source images for all codecs.
- Preserve the original image dimensions after reconstruction.
- Standardize the color representation for quality comparisons.
- Use actual compressed bitstream sizes rather than estimated entropy alone.
- Include required decoding metadata in final bitrate accounting.
- Record codec settings, software versions, and hardware configuration.
- Use multiple bitrate operating points per codec.
- Compare reconstruction quality at similar bitrates.

Preliminary Week 3 payload-only bpp results are not considered final rate measurements.

---

## 8. Google Colab Setup

The project is designed to run in Google Colab.

### Required Libraries

```python
%pip install pillow numpy pandas matplotlib scikit-image
%pip install pytorch-msssim compressai
```

PyTorch and CompressAI versions must be compatible with the selected Python runtime. Check the official installation instructions before running the learned models.

### Running the Project

**Step 1:** Open Google Colab.

**Step 2:** Open the relevant notebook from GitHub.

**Step 3:** Install the required dependencies.

**Step 4:** Run the dataset preparation notebook.

**Step 5:** Run the learned compression experiments.

**Step 6:** Run the evaluation notebook.

**Step 7:** Export the generated CSV files and figures.

Downloaded datasets and temporary Colab outputs are not automatically saved to GitHub.

---



## 10. Project Timeline

| Milestone | Week | Main Deliverable | Weight |
|---|---|---|---|
| M1 | 3 | Research, setup, proposal, and experimental protocol | 15% |
| M2 | 6 | Working classical and learned codec comparison | 35% |
| M3 | 9 | Complete benchmark, final report, repository, and demo | 50% |

---

## 11. References

1. **CompressAI — PyTorch Image Compression Library**  
   https://github.com/InterDigitalInc/CompressAI

2. **Ballé et al. (2018)** — Variational Image Compression with a Scale Hyperprior  
   https://arxiv.org/abs/1802.01436

3. **Ballé et al. (2017)** — End-to-End Optimized Image Compression  
   https://arxiv.org/abs/1611.01704

4. **Cheng et al. (2020)** — Learned Image Compression with Discretized Gaussian Mixture Likelihoods and Attention Modules  
   https://arxiv.org/abs/2001.01568

5. **Kodak Lossless True Color Image Suite**  
   https://r0k.us/graphics/kodak/

6. **CLIC Image Compression Challenge**  
   https://compression.cc/

Additional relevant publications from 2023–2026 will be reviewed as part of the Milestone 1 literature review.

---

## 12. Team Members

| Student | Role | Name |
|---|---|---|
| Student A | Dataset Preparation & Classical Codec Baselines | [Student Name] |
| Student B | Learned Compression Models | [Student Name] |
| Student C | Evaluation & Benchmarking | [Student Name] |

**Course:** Digital Image Processing  
**Instructor:** [Instructor Name]  
**Academic Term:** [Term]

---

## Project Status

**Current Phase: Milestone 1 — Week 3**

The repository documents the planned benchmarking methodology and preliminary implementation. Results and completion status will be updated as experiments are verified.

**Next Phase:** Milestone 2 — Implementation and preliminary comparison of classical and learned image codecs.
