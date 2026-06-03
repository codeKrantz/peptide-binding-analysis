# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Research project (PI: Prof. Ratul Chowdhury) investigating how peptide sequence influences metal binding affinity. The goal is to estimate the minimum number of molecular dynamics (MD) and quantum chemistry simulations needed to understand metal-peptide binding.

## Research Phases

1. **Sequence generation** — enumerate all peptide sequences of length 1–6 using the 20 standard amino acids (~67 million total). Use generators/streaming; never load all sequences into memory at once.
2. **CritiCL scoring** — run CritiCL on every sequence to get per-metal probability distributions (Co, Ni, Zn, …) that sum to 1.0.
3. **Additivity analysis** — test whether longer-peptide scores can be predicted from shorter-peptide scores (e.g., `score(AD) ≈ score(A) + score(D)`).
4. **ML modeling** — build models that predict metal binding probabilities from sequence.

## Amino Acids

The 20 standard amino acids used for enumeration:
```
A C D E F G H I K L M N P Q R S T V W Y
```

## Development Standards

- **Language**: Python unless otherwise specified.
- **Memory**: stream sequences directly to files; use generators (`itertools.product` etc.) — never materialize the full sequence space.
- **Batch processing**: process and score sequences in batches to stay within memory and API limits.
- **Reproducibility**: fix random seeds, version data outputs.
- **Scientific reasoning**: explain why an implementation choice was made, not just what it does.

## Running the Notebook

Open `protein-generation.ipynb` in Jupyter. There is no build or test suite yet.

```powershell
jupyter notebook protein-generation.ipynb
# or
jupyter lab
```

## Key Files

| File | Purpose |
|------|---------|
| `protein-generation.ipynb` | Main development notebook (Phase 1 sequence generation) |
| `Ratul.md` | Project spec — phases, constraints, amino acid list |
| `Cyclome.full.pdf` | Background literature |
