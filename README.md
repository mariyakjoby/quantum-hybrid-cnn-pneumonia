# Classical vs Quantum-Hybrid CNNs for Chest X-Ray Pneumonia Detection

A reproducible research project comparing a classical convolutional neural network (CNN) with a quantum-hybrid CNN for pneumonia detection from chest X-ray images.

## Overview

This project investigates whether replacing the final classical prediction head of a CNN with a small variational quantum circuit changes classification performance on a public chest X-ray pneumonia dataset.

The study uses:

* Grayscale chest X-ray images
* Histogram equalization
* 8×8 image resizing
* Normalization to `[0, 1]`
* A shared CNN feature extractor
* A classical prediction head
* An 8-qubit quantum-hybrid prediction head
* Patient-aware train/validation/test splitting
* Five random seeds: `13, 42, 101, 202, 777`

The main comparison is between:

**Model A — Classical CNN**

Shared CNN feature extractor + classical prediction head.

**Model B — Quantum-Hybrid CNN**

Shared CNN feature extractor + variational quantum prediction head.

The canonical experiment evaluates both models using the same patient-aware data split and reports performance across five random seeds.

## Dataset

The project uses the public **Chest X-Ray Images (Pneumonia)** dataset.

The dataset contains:

* 1,341 NORMAL images
* 3,875 PNEUMONIA images
* 5,216 JPEG images in total

The dataset is obtained programmatically in the notebook using KaggleHub.

The dataset itself is **not included in this repository**.

## Model Architecture

The CNN feature extractor consists of three convolutional blocks with channel sizes:

```text
16 → 32 → 64
```

The convolutional features are reduced using global average pooling and passed through a fully connected layer:

```text
CNN → Global Average Pooling → 64 → 8
```

The resulting eight-dimensional representation is used by the respective prediction heads.

### Classical head

The classical model uses a conventional neural-network prediction head.

### Quantum-hybrid head

The quantum model uses an 8-qubit variational circuit with:

* RY-based feature embedding
* 2 strongly entangling layers
* Pauli-Z measurements

The quantum circuit is simulated rather than executed on physical quantum hardware.

## Experimental Protocol

The canonical experiment uses patient-aware grouping to reduce the possibility of patient leakage between training, validation, and test sets.

A fixed patient-aware test split is established first. The remaining training data is then divided into training and validation sets.

For each of the five seeds:

```text
13
42
101
202
777
```

the classical and quantum-hybrid models are trained and evaluated using the same experimental protocol.

Classification thresholds are selected using the validation set and then applied to the fixed test set.

The notebook reports accuracy, AUC, and other evaluation metrics for each seed and summarizes the results using mean and standard deviation.

## Reported Results

The canonical five-seed study reports the following mean ± standard deviation results:

| Model              |      Accuracy |           AUC |
| ------------------ | ------------: | ------------: |
| Classical CNN      | 0.945 ± 0.011 | 0.986 ± 0.002 |
| Quantum-Hybrid CNN | 0.956 ± 0.003 | 0.988 ± 0.002 |

These values are the reported canonical results of the study.

The repository also contains code for an earlier preliminary ensemble experiment. That experiment uses a different image-level split and should not be interpreted as equivalent to the canonical patient-aware comparison.

## Repository Structure

```text
quantum-hybrid-cnn-pneumonia/
│
├── notebooks/
│   └── qml_project.ipynb
│
├── results/
│   ├── canonical_patient_aware_summary.csv
│   ├── canonical_patient_aware_per_seed.csv
│   └── ...
│
├── README.md
├── requirements.txt
├── .gitignore
└── LICENSE
```

Model checkpoint files (`*.pt`) are excluded from version control.

## Installation

Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/quantum-hybrid-cnn-pneumonia.git
cd quantum-hybrid-cnn-pneumonia
```

Create a virtual environment:

```bash
python -m venv .venv
```

Activate it.

### Windows

```bash
.venv\Scripts\activate
```

### macOS / Linux

```bash
source .venv/bin/activate
```

Install the dependencies:

```bash
pip install -r requirements.txt
```

## Running the Notebook

Launch Jupyter:

```bash
jupyter notebook
```

Open:

```text
notebooks/qml_project.ipynb
```

Run the notebook from top to bottom.

The notebook downloads the dataset using KaggleHub and performs preprocessing, model construction, training, evaluation, and result generation.

The canonical experiment is computationally intensive, particularly because it trains both architectures across five random seeds.

## Results

The canonical experiment saves:

```text
results/canonical_patient_aware_summary.csv
results/canonical_patient_aware_per_seed.csv
```

The per-seed file contains the individual experimental results, while the summary file contains aggregated mean and standard deviation statistics.

## Reproducibility

The canonical experiment explicitly uses the following seeds:

```python
SEEDS = [13, 42, 101, 202, 777]
```

The test split is fixed before the seed-specific training runs, and patient-aware grouping is used to reduce patient-level leakage.

The notebook records the resulting metrics for each seed.

## Limitations

Several limitations should be considered when interpreting the results:

* Images are resized to only 8×8 pixels.
* The experiment uses a single fixed patient-aware split.
* The quantum circuit is simulated rather than executed on quantum hardware.
* The study does not establish a practical quantum computational advantage.
* External clinical validation is not performed.
* The dataset is from a public chest X-ray collection and may not represent all clinical populations or imaging settings.
* The preliminary ensemble experiment uses a different image-level split and is not directly comparable with the canonical patient-aware experiment.

Therefore, the results should be interpreted as an experimental machine-learning comparison rather than evidence of clinical utility or demonstrated quantum advantage.

## Research Paper

This repository accompanies the research study:

**“Classical versus Quantum-Hybrid CNNs with Cross-Architecture Ensembling for Chest X-Ray Pneumonia Detection: A Patient-Aware Multi-Seed Study”**

The notebook provides the computational implementation of the experiments described in the study.

## License

This project is intended for research and educational use. Please check the dataset's own license and terms before redistributing or using the dataset.
