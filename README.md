DIP-Image-Compression-Benchmark
Digital Image Processing — Image Compression Benchmarking Suite
Project 30: Rate–Distortion Benchmarking — Classical vs. Learned Image Codecs
Course: Digital Image Processing
Team size: 3 students
Duration: 9 weeks
Current milestone: Milestone 1 — Research, Setup & Experimental Protocol (Week 3)
Milestone weight: 15%
Status: Milestone 1 implementation is documented in two Google Colab notebooks. Dataset preparation, learned-model setup, image preprocessing, and evaluation metric functions are implemented. Full compression/decompression experiments and codec comparisons are not yet demonstrated in these notebooks. Execution results depend on the datasets uploaded and the Colab runtime.

1. Project Overview
This project develops a reproducible benchmark to compare classical image codecs (JPEG, JPEG2000, WebP, AVIF) with pretrained neural codecs (Factorized Prior, Scale Hyperprior, Cheng2020 Anchor). The eventual comparison will examine rate–distortion performance, image fidelity, and encoding/decoding cost using Kodak, Tecnick, and CLIC validation images.

Milestone 1 focuses on the experimental foundation, not the final benchmark.
2. Milestone 1 Objectives and Scope
- Prepare and validate the Kodak, Tecnick, and CLIC datasets.
- Record image metadata, dataset statistics, file checksums, and environment versions.
- Configure PyTorch and CompressAI, and register three pretrained architectures.
- Initialize a pretrained baseline and inspect its parameters.
- Convert images to RGB tensors, add a batch dimension, and pad dimensions for neural processing.
- Implement and validate bpp, PSNR, SSIM, and MS-SSIM functions.
- Define reproducibility and fair-comparison rules for subsequent milestones.
Not included as completed Milestone 1 results: end-to-end encoded bitstreams, reconstructed images from all codecs, codec timing benchmarks, JPEG rate–distortion curves, or final comparison tables.
3. Notebook Guide
A. notebooks/Dataset_Preparation.ipynb
This notebook covers:
1. Environment and folders: installs Pillow, Pandas, Matplotlib, and Requests; creates /content/DIP_Project directories.
2. Kodak: downloads kodim01.png through kodim24.png and checks image integrity.
3. Tecnick: accepts a manually uploaded dataset archive, extracts it, and counts image files.
4. CLIC validation: accepts a manually uploaded archive, extracts it, and counts images.
5. Inspection: displays sample images and extracts resolution, file format, color mode, and file size.
6. Validation: verifies images, flags invalid files, checks expected Kodak filenames, and calculates SHA-256 fingerprints.
7. Reporting: exports metadata, statistics, validation summaries, and library versions.
8. Visualization: plots image counts and image resolution distributions.
9. Preprocessing protocol: documents image handling for subsequent codec tests.
Generated CSVs (after successful execution):
- reports/dataset_information.csv
- reports/dataset_summary.csv
- reports/validation_summary.csv
- reports/environment.csv
Tecnick and CLIC are not automatically downloaded in this notebook; the user must supply their archives.
B. notebooks/AI_Compression_&_Evaluation_milestone_1.ipynb
The combined notebook has four sections:

Section 1 — Environment setup
- Installs compressai==1.2.8 and other dependencies; installs huggingface_hub for sample-image retrieval.
- Checks Python, PyTorch, NumPy, and CompressAI versions.
- Configures the /content/DIP_Project workspace, random seeds, and CPU/GPU selection.
- Attempts to retrieve one sample image each from Kodak, Tecnick, and CLIC and displays sample metadata.
Section 2 — Pretrained compression architectures
- Registers bmshj2018_factorized (qualities 1–8), bmshj2018_hyperprior (qualities 1–8), and cheng2020_anchor (qualities 1–6).
- Defines a reusable pretrained model loader.
- Initializes Factorized Prior at quality level 3 and inspects model architecture and parameter counts.
- The other two architectures are configured but not benchmarked in this milestone notebook.
Section 3 — Image preprocessing
- Converts PIL RGB images to normalized PyTorch tensors.
- Adds a batch dimension and moves tensors to the selected device.
- Pads image dimensions to a multiple of 64.
- Visualizes original/padded images and checks tensor shapes.
- The final benchmark must crop decoded images back to their original dimensions before evaluating fidelity.
Section 4 — Evaluation metrics
- Implements calculate_bpp, calculate_psnr, calculate_ssim, calculate_ms_ssim, and evaluate_image_quality.
- Installs pytorch-msssim and validates metric functions by comparing an image with itself.
- The bpp function is tested using an example byte count; this is not a measured codec bitrate.
4. Datasets
Dataset	Milestone 1 workflow	Intended use
Kodak	Download 24 PNG images; validate and inspect	Initial development and reference tests
Tecnick	Upload/extract archive; validate and inspect	High-resolution evaluation
CLIC Validation	Upload/extract archive; validate and inspect	Extended benchmark evaluation


Dataset sources: Kodak image suite · Tecnick test images · CLIC challenge archives

Dataset version, source, selected subset, and licensing conditions should be recorded before the final benchmark. The sample images retrieved by the AI notebook are not equivalent to running the full datasets.
5. Repository Structure
DIP-Image-Compression-Benchmark/
├── notebooks/
│   ├── Dataset_Preparation.ipynb
│   └── AI_Compression_&_Evaluation_milestone_1.ipynb
├── datasets/
│   └── README.md                  # Dataset acquisition instructions
├── reports/                       # CSVs exported from Colab when run
│   ├── dataset_information.csv
│   ├── dataset_summary.csv
│   ├── validation_summary.csv
│   └── environment.csv
├── results/                       # Reserved for later codec experiments
├── .gitignore
└── README.md
This is the recommended repository layout. The notebooks use /content/DIP_Project at runtime; outputs must be downloaded or copied to the repository explicitly. Generated files shown above are not guaranteed to be committed.
6. Running the Colab Notebooks
1. Open Dataset_Preparation.ipynb in Google Colab.
2. Run its installation and directory setup cells.
3. Download Kodak; upload Tecnick and CLIC archives when prompted.
4. Run integrity checks, metadata extraction, statistics, and reporting cells.
5. Open AI_Compression_&_Evaluation_milestone_1.ipynb in Colab.
6. Run installation and environment checks. Confirm the runtime supports the pinned CompressAI version.
7. Retrieve the notebook's sample images, configure/load the pretrained baseline, and execute preprocessing.
8. Run the metric definitions and self-comparison validation.
9. Export the resulting reports and record any download, compatibility, or execution issues.
Colab note: Each notebook creates or uses /content/DIP_Project; separate Colab sessions do not automatically share files. Upload or mount the relevant datasets again if necessary. Model weight and sample-image retrieval require network access.
7. Evaluation Metrics
Metric	Definition	Milestone 1 status
bpp	8 × compressed_bytes / (width × height)	Function implemented; synthetic-size test
PSNR	10 log10(1 / MSE) for tensors normalized to [0, 1]	Function implemented and self-tested
SSIM	Structural similarity on normalized images	Function implemented and self-tested
MS-SSIM	Multi-scale structural similarity	Function implemented and self-tested
Encode/decode time	Elapsed compression/decompression time	Planned for Milestone 2


Self-comparison should yield zero MSE, infinite PSNR, and SSIM/MS-SSIM near 1, subject to supported image sizes and numerical behavior. These checks validate metric functions, not codec reconstruction quality.
8. Experimental Protocol for Milestones 2–3
For a fair final benchmark:
1. Use the same source images for every codec.
2. Keep source resolution unchanged; crop away any model padding after decoding.
3. Standardize RGB conversion and metric computation.
4. Store real compressed bitstreams and count required decoding metadata in final bpp.
5. Evaluate multiple operating points per codec and compare quality at matched or interpolated bitrates.
6. Report PSNR, SSIM, MS-SSIM, and encode/decode latency.
7. Document dataset subsets, codec options, model quality levels, library versions, hardware, and timing methodology.
8. Produce rate–distortion curves and aggregate results only after validating the full pipeline.
9. Milestone Deliverables and Progress
Deliverable	Status in supplied notebooks
Dataset acquisition and directory setup	Implemented; Tecnick/CLIC require user archives
Image verification and metadata CSV	Implemented; results depend on execution
SHA-256, dataset statistics, and visualizations	Implemented
CompressAI setup and three-model configuration	Implemented
Factorized Prior initialization and parameter inspection	Implemented
RGB tensor preparation and padding	Implemented
bpp, PSNR, SSIM, MS-SSIM metric functions	Implemented with basic validation
Full neural codec compress/decompress results	Not yet demonstrated
Classical codec comparisons and RD curves	Planned for Milestone 2
Final cross-codec benchmarking report	Planned for Milestone 3


10. Timeline
Milestone	Week	Deliverable	Weight
M1	3	Research, dataset preparation, environment, metric foundations, protocol	15%
M2	6	Working classical and learned codec experiments; preliminary comparisons	35%
M3	9	Complete reproducible benchmark, final report, repository, and demo	50%


11. References
1. CompressAI repository
2. Ballé et al. (2017), End-to-End Optimized Image Compression.
3. Ballé et al. (2018), Variational Image Compression with a Scale Hyperprior.
4. Cheng et al. (2020), Learned Image Compression with Discretized Gaussian Mixture Likelihoods and Attention Modules.
5. Kodak Lossless True Color Image Suite.
6. CLIC challenge.
Additional literature from 2023–2026 will be incorporated into the Milestone 1 research documentation.
12. Team
Student	Responsibility	Name
A	Dataset preparation and classical codec baseline planning	[Student Name]
B	Learned compression models and preprocessing	[Student Name]
C	Evaluation metrics and benchmark protocol	[Student Name]


Instructor: [Instructor Name]
Academic term: [Term]
Next step: Milestone 2 — implement actual classical and learned codec encoding/decoding, collect bitrate and fidelity measureme
