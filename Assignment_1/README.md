# Assignment 1 — Byte-Pair Encoding (BPE) Tokenizer

## Overview

This assignment focuses on implementing, training, and analyzing a Byte-Pair Encoding (BPE) tokenizer from scratch. The work is divided into three main parts:

1. **Word-level corpus analysis** using the Brown corpus.
2. **Implementation of a BPE tokenizer** without relying on existing tokenization libraries.
3. **Training and evaluation** of the tokenizer using statistical metrics such as fertility and tokenized sentence length.

The goal of the assignment is to gain a deeper understanding of vocabulary distributions in natural language and the practical behavior of subword tokenization methods.

---

## Task 1 — Word-Level Statistics and Vocabulary Coverage

- The Brown corpus from NLTK is used as the primary dataset.
- Word frequencies are computed after lowercasing tokens.
- A cumulative coverage curve is plotted, showing how much of the total token mass is covered by the top-*k* most frequent words.
- The **appropriate vocabulary size** is defined as the smallest *k* such that the cumulative coverage reaches at least **90%** of all token occurrences.
- The observed behavior follows **Zipf’s law**, where a small number of frequent words accounts for most of the corpus.

---

## Task 2 — BPE Tokenizer Implementation

A Byte-Pair Encoding tokenizer is implemented from scratch:

- Words are represented as sequences of characters with an end-of-word marker (`</w>`).
- At each training step, the most frequent adjacent symbol pair is merged.
- Merge operations are stored and applied in order of their learned priority.
- Tokenization applies these merges consistently to new text.

No external BPE/tokenization libraries (e.g., SentencePiece, HuggingFace tokenizers) are used.

---

## Task 3 — Training and Analysis

- The BPE tokenizer is trained on the Brown corpus.
- An appropriate BPE vocabulary size is selected (distinct from the word-level vocabulary size).
- The tokenizer is evaluated on a subset of sentences using:
  - **Fertility**: average number of subword tokens per word.
  - **Tokenized sentence length**: number of subword tokens per sentence.
- Mean and standard deviation are reported for both metrics.

---

## Repository Structure

```

.
├── Assignment1.ipynb
├── README.md
└── requirements.txt

```

---

## Requirements

- Python 3.10+
- `numpy`
- `matplotlib`
- `nltk`

Install dependencies with:

```bash
pip install -r requirements.txt
```

---

## Notes

* Training a naive BPE implementation on a large corpus is computationally expensive; therefore, training time can be significant.
* This implementation prioritizes clarity and correctness over performance optimizations.
