[![Status](https://img.shields.io/badge/Status-Active%20Development-orange?style=flat-square)](https://github.com/frank-morales2020/MLxDL)
[![Stars](https://img.shields.io/github/stars/frank-morales2020/MLxDL?style=flat-square&color=yellow)](https://github.com/frank-morales2020/MLxDL/stargazers)
[![Forks](https://img.shields.io/github/forks/frank-morales2020/MLxDL?style=flat-square&color=lightgrey)](https://github.com/frank-morales2020/MLxDL/network/members)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)
[![Hugging Face Models](https://img.shields.io/badge/HF%20Models-47-blue?style=flat-square&logo=huggingface)](https://huggingface.co/frankmorales2020)

# 🏛️ UNESCO2026 - Resilient AI Challenge Test Repository

A comprehensive testbed for validating the complete containerization workflow for the UNESCO Resilient AI Challenge across **TEXT, AUDIO, and VISION** modalities. This repository demonstrates the successful splitting, uploading, downloading, and validation of large-scale AI model containers using GitHub Releases and udocker.

## 📦 Submission Packages

Each modality solution includes a complete submission package (ZIP file) containing the benchmark brain (`main.py`) and all supporting files:

| Modality | Submission Package | Contents |
|----------|-------------------|----------|
| **TEXT** | `Sarvam-1_UNESCO_FrankMorales.zip` | `main.py`, `run.sh`, `test.sh`, Dockerfile, requirements.txt, README.md |
| **AUDIO** | `Voxtral_UNESCO_FrankMorales.zip` | `main.py`, `run.sh`, `test.sh`, Dockerfile, requirements.txt, README.md |
| **VISION** | `Gemma3n_UNESCO_FrankMorales.zip` | `main.py`, `run.sh`, `test.sh`, Dockerfile, docker-compose.yml, requirements.txt, README.md |

These ZIP files contain the complete benchmark implementations and will be made available alongside the container releases.

---

## ✅ Validated Test Containers

### 📝 Text Modality (Sarvam-1)
A fully containerized and validated text-to-text model for Hindi translation.

| Attribute | Details |
|-----------|---------|
| **Model** | `sarvamai/sarvam-1` |
| **Submission Package** | [`Sarvam-1_UNESCO_FrankMorales.zip`](https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0/Sarvam-1_UNESCO_FrankMorales.zip) |
| **Container Size** | 24 GB (exported from udocker) |
| **Release** | [`text-modality-v1.0.0`](https://github.com/frank-morales2020/UNESCO2026/releases/tag/text-modality-v1.0.0) |
| **Parts** | 13 parts (12 × 1.9GB + 1 × 1.1GB) |
| **Status** | ✅ Fully validated and working |

#### 📊 Text Performance Metrics (NVIDIA L4)
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| **Quality (METEOR)** | 0.9922 | > 0.5 | ✅ PASS |
| **VRAM Usage** | 1.670 GB | < 4.0 GB | ✅ PASS |
| **RTF** | 0.079 | < 1.0 | ✅ PASS |
| **Generated Output** | `"प्रतिरोधी एआई कुशल है"` | | |
| **Attention Kernel** | Flash Attention 2 | | |

---

### 🎵 Audio Modality (Voxtral Realtime)
A fully containerized and validated speech recognition model.

| Attribute | Details |
|-----------|---------|
| **Model** | `mistralai/Voxtral-Mini-4B-Realtime-2602` |
| **Submission Package** | [`Voxtral_UNESCO_FrankMorales.zip`](https://github.com/frank-morales2020/UNESCO2026/releases/download/audio-modality-v1.0.0/Voxtral_UNESCO_FrankMorales.zip) |
| **Container Size** | ~18 GB (exported from udocker) |
| **Release** | [`audio-modality-v1.0.0`](https://github.com/frank-morales2020/UNESCO2026/releases/tag/audio-modality-v1.0.0) |
| **Parts** | 11 parts |
| **Status** | ✅ Fully validated and working |

#### 📊 Audio Performance Metrics (NVIDIA L4)
| File | RTF | WER | METEOR | VRAM |
|------|-----|-----|--------|------|
| Obama Transition Address | 0.345 | 0.17 | 0.81 | 2.78 GB |
| MLK Mountaintop 1968 | 0.688 | 0.17 | 0.81 | 2.78 GB |

| Aggregate Metric | Value | Target | Status |
|------------------|-------|--------|--------|
| **Average RTF** | 0.516 | < 1.0 | ✅ PASS |
| **Average Quality (METEOR)** | 0.807 | > 0.5 | ✅ PASS |
| **Peak VRAM** | 2.78 GB | < 4.0 GB | ✅ PASS |

---

### 👁️ Vision Modality (Gemma 3n)
A fully containerized and validated image-to-text model.

| Attribute | Details |
|-----------|---------|
| **Model** | `google/gemma-3n-E2B-it` |
| **Submission Package** | [`Gemma3n_UNESCO_FrankMorales.zip`](https://github.com/frank-morales2020/UNESCO2026/releases/download/vision-modality-v1.0.0/Gemma3n_UNESCO_FrankMorales.zip) |
| **Container Size** | ~11 GB (exported from udocker) |
| **Release** | [`vision-modality-v1.0.0`](https://github.com/frank-morales2020/UNESCO2026/releases/tag/vision-modality-v1.0.0) |
| **Parts** | 11 parts |
| **Status** | ✅ Fully validated and working |

#### 📊 Vision Performance Metrics (NVIDIA L4)
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| **RAM Usage** | 2.23 GB | < 4.0 GB | ✅ PASS |
| **GPU Peak** | 6.77 GB | (monitored) | ⚠️ Note |
| **RTF** | 0.408 sec/word | < 1.0 | ✅ PASS |
| **Quality Score** | 0.857 | > 0.5 | ✅ PASS |
| **Energy Consumption** | 0.000217 kWh | | 🌿 |
| **CO₂ Emissions** | 7.589e-05 kg | | 🌿 |

**Generated Description:**
> "Captured in a close-up shot, a vibrant pink cosmos flower takes center stage, its delicate petals radiating a soft hue. The flower is adorned with..."

---

## 🎯 Challenge Targets Summary

| Modality | Model | Submission Package | Quality | VRAM | RTF | Status |
|----------|-------|-------------------|---------|------|-----|--------|
| **TEXT** | Sarvam-1 | `Sarvam-1_UNESCO_FrankMorales.zip` | 0.992 | 1.67 GB | 0.079 | ✅ PASS |
| **AUDIO** | Voxtral | `Voxtral_UNESCO_FrankMorales.zip` | 0.807 | 2.78 GB | 0.516 | ✅ PASS |
| **VISION** | Gemma 3n | `Gemma3n_UNESCO_FrankMorales.zip` | 0.857 | 2.23 GB* | 0.408 | ✅ PASS |

*RAM usage, GPU peak at 6.77 GB

---

## 🚀 Quick Start Guide

### Prerequisites
```bash
# Install udocker
pip install udocker
udocker --allow-root install

# Set your Hugging Face token
export HF_TOKEN=hf_xxxxxxxxxxxx
```

### Download and Run Any Modality

#### TEXT (Sarvam-1)
```bash
# Download submission package and container parts
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0/Sarvam-1_UNESCO_FrankMorales.zip
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0/text-part-{000..012}

# Extract submission package
unzip Sarvam-1_UNESCO_FrankMorales.zip -d sarvam-text
cd sarvam-text

# Import container to udocker
cat ../text-part-* | udocker --allow-root import - sarvam_unesco:latest

# Download model (requires HF_TOKEN)
./run.sh download-model

# Run benchmark
./run.sh run
# OR with udocker directly:
udocker --allow-root run \
  -v $(pwd):/workspace \
  -v $(pwd)/sarvam_model:/workspace/sarvam_model \
  -v $(pwd)/results:/workspace/results \
  -e MODEL_PATH=/workspace/sarvam_model \
  -e RESULTS_DIR=/workspace/results \
  -e HF_TOKEN=$HF_TOKEN \
  sarvam_unesco /bin/bash -c "cd /workspace && python3 main.py"
```

#### AUDIO (Voxtral)
```bash
# Download submission package and container parts
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/audio-modality-v1.0.0/Voxtral_UNESCO_FrankMorales.zip
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/audio-modality-v1.0.0/audio-part-{000..010}

# Extract submission package
unzip Voxtral_UNESCO_FrankMorales.zip -d voxtral-audio
cd voxtral-audio

# Import container to udocker
cat ../audio-part-* | udocker --allow-root import - audio_unesco:latest

# Clone test dataset
git clone https://github.com/frank-morales2020/UNESCO.git

# Download model (requires HF_TOKEN)
./run.sh download-model

# Run benchmark
./run.sh run
# OR with udocker directly:
udocker --allow-root run \
  -v $(pwd):/workspace \
  -v $(pwd)/voxtral_model:/workspace/voxtral_model \
  -v $(pwd)/results:/workspace/results \
  -v $(pwd)/UNESCO:/workspace/UNESCO \
  -e MODEL_PATH=/workspace/voxtral_model \
  -e RESULTS_DIR=/workspace/results \
  audio_unesco /bin/bash -c "cd /workspace && python3 main.py"
```

#### VISION (Gemma 3n)
```bash
# Download submission package and container parts
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/vision-modality-v1.0.0/Gemma3n_UNESCO_FrankMorales.zip
wget https://github.com/frank-morales2020/UNESCO2026/releases/download/vision-modality-v1.0.0/vision-part-{000..010}

# Extract submission package
unzip Gemma3n_UNESCO_FrankMorales.zip -d gemma-vision
cd gemma-vision

# Import container to udocker
cat ../vision-part-* | udocker --allow-root import - gemma_unesco:latest

# Download model (requires HF_TOKEN)
./run.sh download-model

# Run benchmark
./run.sh run
# OR with udocker directly:
udocker --allow-root run \
  -v $(pwd):/workspace \
  -v $(pwd)/gemma_model:/workspace/gemma_model \
  -v $(pwd)/results:/workspace/results \
  -e MODEL_PATH=/workspace/gemma_model \
  -e RESULTS_DIR=/workspace/results \
  gemma_unesco /bin/bash -c "cd /workspace && python3 main.py"
```

---

## 📁 Complete Repository Structure

```
UNESCO2026/
├── 📦 Submission Packages/
│   ├── Sarvam-1_UNESCO_FrankMorales.zip    # Text solution (main.py brain)
│   ├── Voxtral_UNESCO_FrankMorales.zip      # Audio solution (main.py brain)
│   └── Gemma3n_UNESCO_FrankMorales.zip      # Vision solution (main.py brain)
│
├── 📝 text-modality-v1.0.0/                  # Text container parts
│   ├── text-part-000
│   ├── text-part-001
│   └── ... (up to text-part-012)
│
├── 🎵 audio-modality-v1.0.0/                  # Audio container parts
│   ├── audio-part-000
│   ├── audio-part-001
│   └── ... (up to audio-part-010)
│
└── 👁️ vision-modality-v1.0.0/                 # Vision container parts
    ├── vision-part-000
    ├── vision-part-001
    └── ... (up to vision-part-010)
```

---

## 🔧 Technical Implementation

All three validators follow the same proven pattern:

1. **Package Builder** - Creates submission ZIP with the benchmark brain:
   - `main.py` - **The Brain** - Modality-specific benchmark logic
   - `run.sh` - Container management script
   - `test.sh` - Environment validation
   - Dockerfile + requirements.txt
   - README.md with validated results

2. **Runtime Validator** - Tests with udocker:
   - Downloads model via Hugging Face token
   - Downloads/imports split container from GitHub Releases
   - Mounts volumes for model + results
   - Executes benchmark brain (`main.py`) inside container

3. **Common Technology Stack:**
   - PyTorch 2.10 + CUDA 12.8
   - Flash Attention 2 acceleration
   - 4-bit quantization (bitsandbytes)
   - CodeCarbon for energy tracking
   - udocker for rootless container execution

---

## 📊 Validation Environment

All benchmarks were validated on:
- **GPU:** NVIDIA L4
- **CUDA:** 12.8
- **PyTorch:** 2.10.0
- **udocker:** 1.3.17
- **Flash Attention:** 2.8.3

---

## 👤 Author

**Frank Morales Aguilera**  
Founder and CEO

[Sovereign Machine Lab](https://sovereign-machine-lab.ai/) 

[frank.morales@sovereign-machine-lab.ai](mailto:frank.morales@sovereign-machine-lab.ai)


---

## 📜 License

MIT License - see [LICENSE](LICENSE) file for details.

---

*Last Updated: March 18, 2026*
