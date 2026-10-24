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
