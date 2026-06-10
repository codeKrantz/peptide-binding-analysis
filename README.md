# peptide-binding-analysis

Research project (PI: Prof. Ratul Chowdhury) investigating how peptide sequence influences metal binding affinity. The ultimate goal is to estimate the minimum number of molecular dynamics (MD) and quantum chemistry simulations needed to understand metal-peptide binding behavior.

## Research Phases

| Phase | Description | Status |
|-------|-------------|--------|
| 1 | Sequence generation — enumerate all peptide sequences (length 1–6) | **Complete** |
| 2 | CritiCL scoring — predict per-metal binding probability distributions | **In Progress** |
| 3 | Additivity analysis — test whether longer scores can be predicted from shorter ones | Pending |
| 4 | ML modeling — predict metal binding probabilities from sequence | Pending |

## Phase 1 — Sequence Generation

All 67,368,420 peptide sequences across lengths 1–6 have been generated using the 20 standard amino acids and written to CSV files.

### Output Files

| File | Sequences | Size |
|------|-----------|------|
| `sequences_1aa.csv` | 20 | ~70 B |
| `sequences_2aa.csv` | 400 | ~1.6 KB |
| `sequences_3aa.csv` | 8,000 | ~40 KB |
| `sequences_4aa.csv` | 160,000 | ~960 KB |
| `sequences_5aa.csv` | 3,200,000 | ~22 MB |
| `sequences_6aa.csv` | 64,000,000 | ~512 MB |
| `sequences_all.csv` | **67,368,420** | ~535 MB |

`sequences_all.csv` is the combined file containing all sequences from every length in a single `sequence` column.

### Amino Acids

The 20 standard amino acids used for enumeration:

```
A C D E F G H I K L M N P Q R S T V W Y
```

### Implementation Notes

- Sequences are generated with `itertools.product` and streamed directly to disk — the full sequence space is never held in memory.
- The combined file is produced by streaming each per-length file line-by-line, writing the header once.
- The 6aa generation takes ~12 minutes to run.

## Phase 2 — CritiCL Scoring

CritiCL uses a pre-trained XGBoost classifier on top of ESM-C (300M) embeddings to predict per-metal binding probability distributions. Output metals: **Co, Ln, Mn, Ni, Zn** (probabilities sum to 1.0).

### How it works

1. Each sequence is embedded using `ESMC.from_pretrained("esmc_300m")` — residue embeddings are mean-pooled to a 960-dimensional vector.
2. The XGBoost model (`KarunaAnna/CritiCL` on Hugging Face) classifies the vector into the most likely binding metal and returns per-class probabilities.

### Running locally

Open `Copy_of_CritiCL .ipynb` in Jupyter and run **Kernel → Restart & Run All**.

Cell 0 installs all dependencies (`esm`, `torch`, `httpx`, etc.) into the active kernel's Python using `sys.executable`, so packages land in the right environment regardless of which Python Jupyter is using.

Input options (prompted interactively):
- Single sequence
- Multiple sequences (manual entry)
- CSV or Excel file — enter the full local file path when prompted

Output is a DataFrame with columns: `Sequence`, `prediction`, `confidence_max`, `proba_Co`, `proba_Ln`, `proba_Mn`, `proba_Ni`, `proba_Zn`.

> **Note**: The notebook was originally a Google Colab file. All `google.colab` file upload/download calls have been replaced with local path prompts and `os.path.abspath` output so it runs in any Jupyter environment.

### Environment requirements

| Package | Role |
|---------|------|
| `torch` | ESM-C backbone |
| `esm` | ESM-C model and API (`esm.models.esmc`, `esm.sdk.api`) |
| `httpx` | Required transitive dependency of `esm` |
| `xgboost` | CritiCL classifier |
| `huggingface_hub` | Downloads model weights from `KarunaAnna/CritiCL` |
| `joblib` | Loads serialized model and label encoder |

## Running the Notebooks

```powershell
jupyter notebook protein-generation.ipynb
# or
jupyter lab
```

All generation and combining code lives in `protein-generation.ipynb`. CritiCL scoring lives in `Copy_of_CritiCL .ipynb`.

## Development Standards

- **Memory**: stream sequences to files via generators; never materialize the full sequence space.
- **Batch processing**: process and score sequences in batches.
- **Reproducibility**: fix random seeds, version data outputs.
- **Scientific reasoning**: explain why an implementation choice was made, not just what it does.
