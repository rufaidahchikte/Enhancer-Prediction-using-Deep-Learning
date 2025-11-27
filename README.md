# Enhancer-Prediction-using-Deep-Learning
Sequence-based deep learning models for enhancer recognition and multi-tissue regulatory activity prediction.

Enhancer Prediction using Deep Learning

This repository contains code, data-processing pipelines, and models for predicting enhancer activity and tissue-specific regulatory patterns using deep learning. The project integrates large-scale genomic and epigenomic annotations from ENCODE SCREEN with the GRCh38 reference genome to construct a unified multi-tissue enhancer dataset and evaluate three neural architectures: a CNN, a hybrid CNN+LSTM model, and a Transformer (BERT-style) encoder.

Project Overview

Enhancers are non-coding DNA elements that regulate gene expression, but their identification remains challenging due to variable genomic positions and dynamic activity across tissues. This project addresses two prediction tasks:

Enhancer classification (pELS/dELS vs non-enhancers)

Multi-tissue enhancer activity prediction across nine tissues

The study compares how convolutional, recurrent, and self-attention–based models capture enhancer features and tissue-specific regulatory patterns.
