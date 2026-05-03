# INCRT-geo: Self-Architecting Protein Transformers

> Reference implementation and pretrained checkpoints for the paper:
> **"Self-Architecting Protein Transformers: An Empirical Study"**, by Cirrincione & Lovino, *Briefings in Bioinformatics*, 2026 (under review).

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![PyTorch 2.0+](https://img.shields.io/badge/pytorch-2.0+-orange.svg)](https://pytorch.org/)

## Overview

INCRT-geo is a **self-architecting transformer** for protein language modelling. Unlike standard pretrained protein language models (ESM-2, ProtBERT, ProtTrans), where the architecture is fixed *a priori*, INCRT-geo derives **its own depth and width from the spectral geometry of the task operator** during pretraining: heads are added (Level-1), pruned, and entire layers are introduced (Level-3) according to provable trigger conditions on the antisymmetric component of the attention motor.

This repository contains:

- **Five pretrained checkpoints** (v8/v9/v10/v11/v9') produced on the human reference proteome and on an 8-species vertebrate corpus
- **The notebooks** that produced each pretraining run from scratch
- **The all-night evaluation pipeline** for Pfam-50 classification (linear probe + light fine-tune), with multi-seed runs for error bars
- **Architectural diagnostics** (per-head asymmetry index, effective rank of the antisymmetric motor stack, pairwise head similarity)

## Headline result

INCRT-geo (v9) achieves **76.0% Pfam-50 linear-probe accuracy with 22.7M parameters**, pretrained on the human reference proteome only (~20k sequences). Comparison baselines:

| Model            | Params  | Pretraining corpus     | Pfam-50 probe | Pfam-50 fine-tune |
|------------------|---------|------------------------|---------------|-------------------|
| ESM-2 small      | 7.5 M   | UniRef50 (~60M seqs)   | 71.1%         | —                 |
| ProtBERT         | 420 M   | BFD (~2B seqs)         | 72.2%         | —                 |
| **INCRT-geo v9** | **22.7 M** | **Human Ensembl (~20k seqs)** | **76.0%** | **70.8%**     |

Three single-variable ablations (tokenisation granularity, depth growth Level-3, alpha-loss regularisation) and a multi-seed robustness study confirm the contribution of each design choice. A scaling experiment on a multi-vertebrate corpus (~284k sequences across 8 species) is reported as a controlled negative result.

Full details, error bars, and discussion are in the paper.

## Quick start: reproduce Pfam evaluation in 15 minutes

The fastest way to verify a result is to download a pretrained checkpoint and run the Pfam evaluation:

```python
# 1. Install dependencies
pip install -r requirements.txt

# 2. Download checkpoints from Zenodo (DOI: 10.5281/zenodo.XXXXXXX — see PLACEHOLDER below)
#    Alternatively, the all-night evaluation notebook handles this automatically.

# 3. Run the all-night evaluation pipeline (Pfam + diagnostics)
jupyter notebook scripts/allnight_eval.ipynb
```

On a single A100 GPU, end-to-end Pfam-50 evaluation of one model takes ~5-7 minutes (linear probe + light fine-tune). The all-night pipeline runs all five checkpoints + multi-seed runs + architectural diagnostics in approximately 5 hours.

## Repository structure

```
incrt-geo-proteins/
├── README.md                                  ← this file
├── LICENSE                                    ← Apache-2.0
├── CITATION.cff                               ← cite as
├── requirements.txt                           ← Python dependencies
├── .gitignore
│
├── notebooks/                                 ← pretraining notebooks (each end-to-end)
│   ├── incrt_geo_v8.ipynb                    ← 3-mer tokeniser baseline
│   ├── incrt_geo_v9.ipynb                    ← main: 1-mer + full INCRT-geo
│   ├── incrt_geo_v10.ipynb                   ← ablation: Level-3 disabled
│   ├── incrt_geo_v11.ipynb                   ← ablation: alpha-loss disabled
│   └── incrt_geo_v9_multivertebrate.ipynb    ← scaling: 8 vertebrate proteomes
│
├── scripts/                                   ← evaluation and diagnostics
│   ├── allnight_eval.ipynb                   ← Pfam (5 models, 3 seeds) + diagnostics
│   └── v9p_pfam_rescue.ipynb                 ← OOM-resistant Pfam eval (batch=8)
│
├── data/                                      ← input data
│   └── README.md                              ← FASTA download instructions
│
└── results/                                   ← pre-computed evaluation outputs
    ├── pfam_summaries/                        ← JSON: probe + fine-tune accuracies
    └── architectural_diagnostics/             ← JSON: alpha, effective rank, similarity
```

## Pretrained checkpoints

The five pretrained checkpoints are hosted on Zenodo for permanent archival access:

| Model | Description | File | Size |
|-------|-------------|------|------|
| v8    | 3-mer tokeniser baseline | `incrt_geo_v8.pt`   | 152 MB |
| **v9** | **Main: 1-mer, full framework** | `incrt_geo_v9.pt`   | 92 MB |
| v10   | Ablation: Level-3 disabled | `incrt_geo_v10.pt`  | 58 MB |
| v11   | Ablation: alpha-loss disabled | `incrt_geo_v11.pt`  | 62 MB |
| v9'   | Multi-vertebrate corpus | `incrt_geo_v9p.pt`  | 202 MB |

**Zenodo DOI:** `10.5281/zenodo.XXXXXXX` *(placeholder — to be replaced upon paper acceptance; see [ZENODO_INSTRUCTIONS.md](ZENODO_INSTRUCTIONS.md))*

## Background: the INCRT-geo framework

INCRT-geo applies the three-level **Incremental Transformer** framework introduced in Cirrincione & Ghione (2026, "Self-Architecting Transformers: A Theory of Incremental Growth and Pruning", submitted to JMLR). The framework rests on three growth mechanisms operating on three time-scales:

- **Level-1** allocates a new attention head when the dominant eigenvalue of the residual operator A_res = Π_⊥ sym(X⊤X M_a) Π_⊥ exceeds a threshold θ_w
- **Level-2** allocates a block of m heads when the residual spectrum is approximately m-fold degenerate
- **Level-3** adds a new layer when the topmost layer has saturated as a phase-transition order parameter (mean asymmetry index α_top > α_crit and frozen)

Two auxiliary losses regularise the dynamics:
- **alpha-loss** L_α drives heads towards the directional regime (α → 1)
- **geometric loss** L_geo encourages the antisymmetric motor norm to grow

This repository's contribution is the **first application of this framework to protein language modelling**, with a controlled empirical study of which design choices matter and which do not.

## Citation

If you use this code or the pretrained checkpoints, please cite:

```bibtex
@article{cirrincione2026incrtgeoproteins,
  title   = {Self-Architecting Protein Transformers: An Empirical Study},
  author  = {Cirrincione, Giansalvo and Lovino, Marta},
  journal = {Briefings in Bioinformatics},
  year    = {2026},
  note    = {Under review}
}
```

A `CITATION.cff` file is included for GitHub's "Cite this repository" button.

## License

This code is released under the **Apache License 2.0**. See [LICENSE](LICENSE) for the full text.

In short: you are free to use, modify, distribute, and use this code commercially, provided that you preserve the copyright notice and the license text. The Apache 2.0 license also includes an explicit patent grant.

## Contact

- **Giansalvo Cirrincione** — Laboratoire LTI, Université de Picardie Jules Verne (UPJV), Amiens, France — `giansalvo.cirrincione@u-picardie.fr`
- **Marta Lovino** *(corresponding author)* — Department of Engineering "Enzo Ferrari", Università degli Studi di Modena e Reggio Emilia, Modena, Italy — `marta.lovino@unimore.it`

For questions about the code or to report bugs, please open a [GitHub Issue](https://github.com/exin-lti-upjv/incrt-geo-proteins/issues).

## Acknowledgements

Pretraining and evaluation were performed on Google Colab Pro+ (NVIDIA A100). The INCRT-geo framework was developed in collaboration with Giorgia Ghione (Politecnico di Torino).
