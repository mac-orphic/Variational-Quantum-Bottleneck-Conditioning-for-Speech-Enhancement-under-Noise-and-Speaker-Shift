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

=======
# Quantum-conditioned speech enhancement (QUNet)

Research code and supporting materials for a hybrid classical/quantum speech-enhancement study. The project uses a complex-spectral U-Net and a small variational quantum circuit for global bottleneck conditioning.

## Repository contents

- `notebooks/data_preparation/`: notebook that generates paired clean/noisy examples.
- `notebooks/main_experiments/`: parameter-matched multi-location VQC study and a later 10K AMP-audited feasibility/protocol notebook. These are different experiment generations; follow the protocol and configuration tied to the results you intend to report.
- `notebooks/exploratory/`: QMicroSE-SVQC, a related but separate experiment. Do not combine its results with the QUNet manuscript results.
- `results/unseen_noise_seed_17_23_123/`: selected-seed unseen-noise output tables and per-seed metrics.
- `results/double_ood_seed_17_23_123/`: selected-seed joint unseen-speaker/noise tables and per-seed metrics.
- `results/10k_protocol_feasibility/`: feasibility and configuration summaries from the later protocol.
- `results/legacy_exports/`: older summary export; retained for provenance and not assumed to match the manuscript.
- `paper/`: manuscript source draft and earlier PDF/DOCX materials. The `.tex` is a working draft; check its scientific wording, affiliations, references, and compile it before citing it as the final version.
- `private_inputs/`: original uploaded result archives, retained locally for provenance. This directory is included in the starter ZIP for your private record but excluded from Git by `.gitignore`. Some archives contain machine-specific paths; do not force-add or publish them without cleaning and review.

## Before making the repository public

1. Review every notebook and result table to choose the exact experiment version used in the paper. The selected-seed stage-one results, the 10K feasibility package, and the separate QMicroSE study are not interchangeable.
2. Do not commit speech/audio datasets, raw audio, model checkpoints, credentials, or unreviewed manifests. Dataset licenses may restrict redistribution. Provide dataset download/preparation instructions instead.
3. Inspect manifests and notebook outputs for personal paths, names, email addresses, tokens, or other private information. Raw uploaded result bundles are under the ignored `private_inputs/` directory.
4. Freeze the manuscript's reported configuration and make sure the code, metrics, seeds, and tables correspond to it. Do not claim complete reproducibility unless the exact training/evaluation code and checkpoints can reproduce the reported values.
5. Add a license only after deciding what license applies to your code and confirming permissions for all included material.

## Suggested upload order

1. Create the GitHub repository as **Private** while reviewing it.
2. Upload `README.md` and `.gitignore` first.
3. Add the source notebooks under `notebooks/`.
4. Add clean result tables and protocol summaries under `results/`.
5. Add the manuscript source and supporting documents under `paper/`.
6. Review the commit diff and verify that `private_inputs/`, datasets, audio, and checkpoints are not staged.
7. Make the repository public only after checking co-author approval, licenses, privacy, and that the paper's reported results are the ones represented by the code.

## Upload from a terminal

After creating an empty GitHub repository, open a terminal in this folder and run:

```bash
git init
git add README.md .gitignore notebooks results paper
git status --short
# Inspect the staged file list before committing.
git commit -m "Organize QUNet speech enhancement research project"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/YOUR-REPOSITORY.git
git push -u origin main
```

Replace the remote URL with your repository URL. Since `private_inputs/` is ignored, the command above will not upload the original result archives.
>>>>>>> f60f81d3f2e1a99771c6a53935134efd9d21f7c3
