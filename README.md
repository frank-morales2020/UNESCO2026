[![Status](https://img.shields.io/badge/Status-Active%20Development-orange?style=flat-square)](https://github.com/frank-morales2020/MLxDL)
[![Stars](https://img.shields.io/github/stars/frank-morales2020/MLxDL?style=flat-square&color=yellow)](https://github.com/frank-morales2020/MLxDL/stargazers)
[![Forks](https://img.shields.io/github/forks/frank-morales2020/MLxDL?style=flat-square&color=lightgrey)](https://github.com/frank-morales2020/MLxDL/network/members)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Hugging Face Models](https://img.shields.io/badge/HF%20Models-47-blue?style=flat-square&logo=huggingface)](https://huggingface.co/frankmorales2020)


# 🏛️ UNESCO2026 - Resilient AI Challenge Test Repository

A dedicated testbed for validating the complete containerization workflow for the UNESCO Resilient AI Challenge. This repository demonstrates the successful splitting, uploading, and downloading of large-scale AI model containers using GitHub Releases.

## ✅ Validated Test Container

### 📝 Text Modality (Sarvam-1)
A fully containerized and validated text model demonstrating the complete workflow.

| Attribute | Details |
|-----------|---------|
| **Model** | `sarvamai/sarvam-1` |
| **Container Size** | 24 GB (exported from udocker) |
| **Release** | [`text-modality-v1.0.0`](https://github.com/frank-morales2020/UNESCO2026/releases/tag/text-modality-v1.0.0) |
| **Parts** | 13 parts (12 × 1.9GB + 1 × 1.1GB) |
| **Status** | ✅ Fully validated and working |

### 📊 Performance Metrics
All benchmarks run on NVIDIA L4 GPU with deterministic seed 123.

| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| **Quality (METEOR)** | 0.9922 | > 0.5 | ✅ PASS |
| **VRAM Usage** | 1.668 GB | < 4.0 GB | ✅ PASS |
| **RTF** | 0.076 | < 1.0 | ✅ PASS |
| **Generated Output** | `प्रतिरोधी एआई कुशल है` | | |
| **Attention Kernel** | Flash Attention 2 | | |

## 🚀 Quick Start Guide

### Prerequisites
```bash
# Install udocker
pip install udocker
udocker --allow-root install
