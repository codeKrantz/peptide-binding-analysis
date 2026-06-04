# peptide-binding-analysis

Research project (PI: Prof. Ratul Chowdhury) investigating how peptide sequence influences metal binding affinity. The ultimate goal is to estimate the minimum number of molecular dynamics (MD) and quantum chemistry simulations needed to understand metal-peptide binding behavior.

## Research Phases

| Phase | Description | Status |
|-------|-------------|--------|
| 1 | Sequence generation — enumerate all peptide sequences (length 1–6) | **Complete** |
| 2 | CritiCL scoring — predict per-metal binding probability distributions | Pending |
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

## Running the Notebook

```powershell
jupyter notebook protein-generation.ipynb
# or
jupyter lab
```

All generation and combining code lives in `protein-generation.ipynb`.

## Development Standards

- **Memory**: stream sequences to files via generators; never materialize the full sequence space.
- **Batch processing**: process and score sequences in batches.
- **Reproducibility**: fix random seeds, version data outputs.
- **Scientific reasoning**: explain why an implementation choice was made, not just what it does.
