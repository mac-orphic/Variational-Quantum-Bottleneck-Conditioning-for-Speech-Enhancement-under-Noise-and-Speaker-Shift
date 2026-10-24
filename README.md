<<<<<<< HEAD
# Variational Quantum Bottleneck Conditioning for Speech Enhancement

## Project overview

This project studies whether a compact variational quantum circuit can serve as a global conditioning module inside a classical speech-enhancement network. The model uses a phase-aware, complex-spectral U-Net to process the noisy speech. It summarizes features from several encoder scales and the bottleneck, maps that global summary to six real-valued angles, and processes those angles with a six-qubit variational circuit.

The circuit re-uploads the six input angles through four layers of trainable single-qubit rotations and an ordered CNOT ring. Its readout consists of six single-qubit (Z) expectation values and six adjacent pairwise (ZZ) moments. These twelve features generate bounded channel-wise scale and bias values for the U-Net bottleneck. The circuit is evaluated with an exact statevector simulator in the reported experiments; the work does not report execution on quantum hardware or a quantum speedup.

A parameter-matched classical conditioner provides a comparison with the same six-dimensional input, twelve-dimensional output interface, and 72 stored core parameters. The complete conditioned models each contain 529,823 stored trainable parameters.

## Research questions

The materials address three related questions:

1. Can a small quantum feature map provide useful global conditioning for a phase-aware speech-enhancement model?
2. What mathematical properties follow from the chosen encoding, circuit readout, and bottleneck modulation?
3. How do the quantum-conditioned and parameter-matched classical-conditioned models compare on held-out noise and joint speaker/noise shifts?

The mathematical analysis covers the encoded-angle Fourier support, shared computational-basis measurement statistics, gatewise parameter-shift derivatives, and a bound on how readout estimation error affects the modulated bottleneck.

## Main reported experiment

The manuscript's selected-seed results use seeds 17, 23, and 123. Each evaluation contains 276 mixtures. The unseen-noise evaluation uses held-out noise categories with speakers represented in training/validation; the joint-shift evaluation uses speakers absent from those manifests together with held-out noise. Both use a common pool of eight noise recordings.

Across these selected runs, the reported mean quantum-minus-classical differences are:

| Evaluation | SI-SDR | PESQ | ESTOI |
|---|---:|---:|---:|
| Unseen noise | +0.04972 dB | +0.01240 | +0.000885 |
| Joint unseen-speaker/noise | +0.05703 dB | +0.01256 | +0.000588 |

These are descriptive results for a retrospectively selected seed set, not a prespecified confirmation over all attempted runs. The paper reports the selection scope. The small mean differences should be interpreted within these experiments; they do not establish broad generalization, perceptual benefit, hardware advantage, or quantum advantage.

## Repository contents

- `notebooks/data_preparation/making_data.ipynb` — paired clean/noisy mixture generation.
- `notebooks/main_experiments/qunet_multivqc_parameter_matched.ipynb` — parameter-matched multi-location VQC research notebook.
- `notebooks/main_experiments/qunet_10k_amp_feasibility.ipynb` — later AMP-audited feasibility and 10-seed-ready protocol.
- `notebooks/exploratory/qmicrose_svqc.ipynb` — separate QMicroSE-SVQC experiment; its results are not part of the QUNet manuscript results.
- `results/unseen_noise_seed_17_23_123/` — per-seed and aggregate unseen-noise metrics.
- `results/double_ood_seed_17_23_123/` — per-seed and aggregate joint-shift metrics.
- `results/10k_protocol_feasibility/` — feasibility, audit, and protocol summaries from a later experiment generation.
- `results/qmicrose_separate_experiment/` — per-file results from the separate QMicroSE study.
- `results/legacy_exports/` — an earlier summary export retained for provenance; it should not be assumed to match the selected-seed manuscript tables.
- `paper/` — LaTeX manuscript draft and earlier document/report files.
- `verification/` — numerical verification materials, where included.
- `private_inputs/` — original result archives supplied with the project bundle. They are retained as source material and may include local machine paths; the directory is excluded by `.gitignore`.

The notebooks and result folders span multiple iterations of the study. In particular, the 10K feasibility protocol and QMicroSE study are distinct from the selected-seed manuscript experiment. Their outputs should not be combined unless the corresponding methods and data are shown to match.

## Data and computational setup

The data-preparation and training notebooks describe the speech/noise mixture construction and model pipeline. The manuscript experiment uses VCTK-derived clean speech and environmental recordings from ESC-50, with 16-kHz audio and a complex short-time Fourier transform representation. The raw audio and model checkpoints are not included in this project bundle.

Notebook cells contain the relevant configuration and execution stages. Before running them, configure paths to the required datasets and output directories for your environment. The exact trained checkpoints and executed training/evaluation runners were not independently reproduced as part of the manuscript's numerical verification; the included checks verify circuit definitions, selected metric arithmetic, and implementation consistency.

## Interpretation and scope

The circuit contributes a structured nonlinear map from six learned global acoustic coordinates to twelve bounded measurement features. Pairwise readouts can represent correlations in the computational-basis distribution, but those diagonal correlations alone are not entanglement witnesses. Matching stored parameter counts does not make the quantum and classical function classes equivalent.

The empirical comparison uses ideal statevector expectations. Shared-shot measurement behavior is analyzed mathematically, but finite-shot and hardware execution are not the basis of the reported enhancement scores. The results therefore support a study of quantum-inspired conditioning architecture under the stated simulation and data protocol, rather than a claim of practical quantum advantage.

## Authors

Bhavin Makwana, Deep Bhatt, Manjunath Joshi, and Jaideep Mulherkar.

Affiliations and author contributions should be confirmed in the manuscript before submission.

