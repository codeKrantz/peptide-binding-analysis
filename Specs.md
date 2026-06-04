# Project Overview

Research project on metal-peptide binding.

Professor: Ratul

## Objective

Determine how peptide sequence influences metal binding affinity.

Ultimately we want to estimate the minimum number of molecular dynamics (MD) and quantum chemistry simulations needed to understand metal-peptide binding behavior.

## Phase 1

Generate every possible peptide sequence using the 20 standard amino acids.

Lengths:
- 1 aa = 20
- 2 aa = 400
- 3 aa = 8,000
- 4 aa = 160,000
- 5 aa = 3,200,000
- 6 aa = 64,000,000

Total sequences ≈ 67 million.

Important:
- Never store all sequences in memory.
- Stream directly to files.
- Prefer efficient generators.

## Amino Acids

A C D E F G H I K L
M N P Q R S T V W Y

## Phase 2

Run CritiCL on all generated sequences.

CritiCL predicts probability distributions for metal binding.

Example:

D:
Co=0.3
Ni=0.5
Zn=0.2

Scores sum to 1.0.

## Phase 3

Investigate whether longer peptide scores can be predicted from shorter peptide scores.

Example:

A score + D score

vs

AD score

Determine whether effects are additive.

## Phase 4

Build machine learning models predicting metal binding probabilities from sequence.

## Development Standards

Language: Python preferred unless otherwise specified.

Focus on:
- memory efficiency
- batch processing
- reproducibility
- clear documentation

Always explain scientific reasoning behind implementation choices.