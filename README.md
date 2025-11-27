# Enhancer-Prediction-using-Deep-Learning

Sequence-based deep learning models for enhancer recognition and multi-tissue regulatory activity prediction.

---

## Overview

This repository contains code, data-processing pipelines, and models for predicting enhancer activity and tissue-specific regulatory patterns using deep learning. The project integrates large-scale genomic and epigenomic annotations from ENCODE SCREEN with the GRCh38 reference genome to construct a unified multi-tissue enhancer dataset and evaluate three neural architectures:

- **Convolutional Neural Network (CNN)**
- **Hybrid CNN+LSTM model**
- **Transformer (BERT-style encoder)**

---

## Project Summary

Enhancers are non-coding DNA elements that regulate gene expression, but their identification remains difficult due to variable genomic positions and tissue-dependent activity. This project focuses on two prediction tasks:

1. **Enhancer classification** (pELS/dELS vs non-enhancers)  
2. **Multi-tissue enhancer activity prediction** across nine tissues  

The study compares how convolutional, recurrent, and self-attention-based models capture enhancer sequence features and tissue-specific patterns.

---

## Dataset Acquisition

The dataset integrates genomic and epigenomic information from several sources:

- **ENCODE SCREEN cCRE Registry (GRCh38):** includes genomic coordinates, regulatory classes (PLS, pELS, dELS, CA-*), and IDs.  
- **GRCh38 Reference Genome:** RefSeq FASTA converted to UCSC-style chromosome naming and used to extract exact DNA sequences for each cCRE interval.  
- **Tissue-specific SCREEN cCRE files:** nine tissues (brain, blood, muscle, skin, lung, embryo, large intestine, heart, kidney).  
  - pELS/dELS labels were converted into binary activity indicators: `active_<tissue>`.

Dataset merging was performed using exact genomic coordinates: **chrom, start, end**.  
The final dataset includes sequence, enhancer label, and multi-tissue activity vectors.

---

## Feature Engineering

To prepare sequences for deep learning models, the following steps were applied:

- **Sequence sanitization:** uppercase normalization and removal of invalid characters.  
- **Reverse-complement canonicalization:** ensures strand invariance by selecting the lexicographically smaller of the forward and reverse-complement sequences.  
- **MD5 hashing:** used for leak-free grouping and safe train/validation/test splits.  
- **3-mer tokenization:** overlapping trinucleotides mapped to 64-token integer vocabulary (special token for ambiguous bases).  
- **Padding and masking:** sequences padded to fixed length and binary masks created to indicate true vs padded positions.

The processed dataset contains k-mer IDs, masks, enhancer labels, and tissue activity labels.

---

## Model Architectures

### 1. CNN
Learns local sequence motifs using convolutional layers.  
Shared feature representation is fed into two heads:
- Binary enhancer classifier
- Multi-label tissue activity predictor

### 2. CNN+LSTM (Hybrid)
Combines:
- Convolutional layers for capturing local motifs  
- Bidirectional LSTM for modeling sequential dependencies  
- Dual-task output heads identical to the CNN architecture

### 3. Transformer (BERT-style Encoder)
Configuration:
- 6 encoder layers  
- 4 attention heads per layer  
- Hidden size: 256  
- Feed-forward dimension: 1024  
- Dropout: 0.1  

Uses masked mean pooling and two prediction heads:
- Enhancer (binary)
- Tissue activity (multi-label)

A weighted multi-task loss addresses enhancer/tissue imbalance.

---

## Results Summary

### Enhancer Prediction
- CNN and CNN+LSTM achieved similar accuracy.  
- Enhancer classification relies heavily on **local motifs**, making CNN sufficient.  
- Transformer performs competitively with slightly stronger generalization.

### Tissue Prediction
- Transformer model performed best among the three architectures.  
- All models struggled due to:
  - Severe class imbalance  
  - Sparse labels  
  - Limited sequence-only information

Performance highlights:
- Transformer enhancer precision ≈ **0.80**, recall ≈ **0.75**  
- Tissue F1 ≈ **0.29** across 9 tissues (multi-label)

---

## Limitations

- Highly imbalanced tissue labels reduce model sensitivity.  
- Sequence-only features miss important chromatin context.  
- 3-mer tokenization may be too shallow for complex enhancer logic.  
- Fixed input length restricts modeling of long-range interactions.  
- Multi-task learning may underweight tissue prediction.

---

## Future Work

- Explore richer tokenization (6-mers, DNA subword units, DNABERT-style embedding).  
- Integrate ATAC-seq, DNase-seq, or histone marks for multi-modal prediction.  
- Use architectures supporting long-range context (Longformer, Performer).  
- Improve multi-task balancing (uncertainty weighting, hierarchical losses).  
- Increase dataset diversity and balance across tissues.

---


