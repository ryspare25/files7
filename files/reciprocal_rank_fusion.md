# Reciprocal Rank Fusion (RRF)

## Core Problem

Two retrieval systems (VectorIndex = semantic similarity, BM25 = keyword frequency) produce incompatible scores. You can't average 0.234 and 0.184 directly — they're from different distributions.

## RRF Solution

Discard the raw scores entirely. Use only *rank position*, transformed via reciprocal:

$$\text{RRF}(d) = \sum_{r \in \text{retrievers}} \frac{1}{k + \text{rank}_r(d)}$$

where `k` is a smoothing constant (typically 60) that dampens the outsized advantage of rank 1.

## Worked Example

Query: "INC-2023-Q4-011"

| Chunk | Vector Rank | BM25 Rank | RRF Score |
|---|---|---|---|
| Section 2: Software Engineering | 1 | 2 | 1/(60+1) + 1/(60+2) = **0.03253** |
| Section 7: Historical Research | 2 | 3 | 1/(60+2) + 1/(60+3) = **0.03197** |
| Section 6: Product Engineering | 3 | 1 | 1/(60+3) + 1/(60+1) = **0.03253** |

Section 2 and Section 6 tie (symmetric ranks 1,2 vs 3,1). Section 7 ranks just below.

## Key Properties

- **Scale-invariant:** raw scores are thrown away; only ordinal position matters
- **k smoothing:** prevents rank-1 dominance; higher k = flatter weighting curve
- **Additive fusion:** each retriever contributes independently; more retrievers = more signal
- **No calibration needed:** works out of the box across arbitrarily different scoring systems

## Analogy to Multi-Signal SRS Scheduling

RRF is the retrieval equivalent of a multi-signal picker algorithm. When combining forgetting-curve decay, recency, and interleaving pressure into a single selection score, each signal lives in a different scale. Taking reciprocal rank (or its analog) before summing prevents any single signal from dominating by virtue of native magnitude rather than actual informational value. The `k` constant is the dampening knob — a floor that prevents rank-1 from being infinitely preferred.
