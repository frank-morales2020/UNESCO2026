[![Status](https://img.shields.io/badge/Status-Active%20Development-orange?style=flat-square)](https://github.com/frank-morales2020/MLxDL)
[![Stars](https://img.shields.io/github/stars/frank-morales2020/MLxDL?style=flat-square&color=yellow)](https://github.com/frank-morales2020/MLxDL/stargazers)
[![Forks](https://img.shields.io/github/forks/frank-morales2020/MLxDL?style=flat-square&color=lightgrey)](https://github.com/frank-morales2020/MLxDL/network/members)
[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg?style=flat-square)](https://creativecommons.org/licenses/by-nc/4.0/)
[![Hugging Face Models](https://img.shields.io/badge/HF%20Models-47-blue?style=flat-square&logo=huggingface)](https://huggingface.co/frankmorales2020)

# 🏛️ UNESCO2026 - Resilient AI Challenge Test Repository

A comprehensive testbed for validating the complete containerization workflow for the UNESCO Resilient AI Challenge across **TEXT, AUDIO, and VISION** modalities. This repository demonstrates the successful splitting, uploading, downloading, and validation of large-scale AI model containers using GitHub Releases and udocker.


**IMPORTANT NOTICE – IP PROTECTION DURING EVALUATION**  
To protect intellectual property during the active UNESCO Resilient AI Challenge period (March–May 2026), the full submission ZIP packages and complete container artifacts (split parts) are provided **privately** to UNESCO judges and organizers only via secure submission links or the official portal.  

Public downloads of Releases and ZIPs will be enabled **after May 30, 2026** (submission close), or earlier if permitted by challenge rules.  

For evaluation access: Use the official submission system or contact organizers directly. The repository documentation, validation notebooks, metrics, and methodology remain fully public for transparency.

---

## 📄 White Papers

This repository is accompanied by two technical white papers published under Document IDs `SML-UNESCO-WP1-2026-001` and `SML-UNESCO-WP2-2026-001` (Version 1.0.0, March 18, 2026).

### WP1 — H2E Deterministic Framework: A Cross-Modal Approach to Resilient AI

#### Overview

The **H2E (Human-to-Edge)** framework presents a unified deterministic approach to deploying capable AI systems in resource-constrained environments. It achieves provable efficiency, sustainability, and quality across three distinct AI modalities by combining aggressive quantization, optimized attention mechanisms, and rigorous reproducibility controls.

#### The H2E Philosophy

The framework is built on four core principles:

1. **Deterministic Reproducibility** — Every execution is exactly reproducible through fixed seeds and controlled randomness.
2. **Measurable Efficiency** — All resource consumption (time, memory, energy) is precisely tracked and reported.
3. **Quantifiable Quality** — Output quality is objectively measurable against ground truth or reference standards.
4. **Containerized Portability** — Solutions run identically across environments via udocker.

#### Unified Architectural Framework

All three modalities share a common container architecture:

| Component | Details |
|-----------|---------|
| **Base** | Ubuntu 24.04 with CUDA 12.8 runtime |
| **Python** | 3.12 with system-wide pip |
| **ML Stack** | PyTorch 2.10, Transformers 4.47+ |
| **Attention** | Flash Attention 2.8.3 with SDPA fallback |
| **Quantization** | bitsandbytes 4-bit NF4 with double quantization |
| **Monitoring** | CodeCarbon 2.3+ for energy and emissions tracking |

**Resource Management Strategy:**
- 4-bit NF4 quantization via bitsandbytes
- BF16 compute (bfloat16 dtype throughout)
- Explicit cache clearing between runs
- Peak memory tracking and reporting

**Sustainability Monitoring** — All benchmarks integrate CodeCarbon for real-time power consumption, per-inference energy tracking, CO₂ emissions estimation, and persistent audit logging.

#### WP1 Key Achievements

| Modality | Model | RTF | VRAM | Quality | Status |
|----------|-------|-----|------|---------|--------|
| Text | Sarvam-1 | 0.079 | 1.67 GB | 0.992 METEOR | ✅ PASS |
| Audio | Voxtral-Mini | 0.516 | 2.78 GB | 0.810 METEOR | ✅ PASS |
| Vision | Gemma-3B | 0.408 | 2.23 GB | 0.857 Composite | ✅ PASS |

**Notable innovations:**
- **Text:** Multi-reference METEOR scoring ensures fair evaluation of translation tasks with multiple valid outputs (e.g., *Tikaau*, *Pratirodhak*, *Lacheela* for "resilient" in Hindi).
- **Audio:** Surgical warm-up protocol eliminates the "first-speech penalty" by pre-initializing CUDA kernels before benchmark execution.
- **Vision:** Flash Attention patch improves inference speed while maintaining the 2.23 GB VRAM footprint.

---

### WP2 — H2E Deterministic Framework: Runtime Validation Methodology

#### Overview

This white paper describes the end-to-end validation methodology for all three modality containers. Each runtime notebook follows a five-phase pipeline:

```
GitHub Release → Download Parts → Reassemble Container → Run Benchmark → Validate Results
```

| Notebook | Modality | Container Name | Parts |
|----------|----------|---------------|-------|
| `AUDIO_RUNTIME_VALIDATOR.ipynb` | Audio | `audio_unesco` | 11 |
| `TEXT_RUNTIME_VALIDATOR.ipynb` | Text | `sarvam_unesco` | 13 |
| `VISION_RUNTIME_VALIDATOR.ipynb` | Vision | `gemma_unesco` | 11 |

#### Phase 1 — GitHub Release Download

A smart downloader auto-detects all parts by incrementing part numbers until a download fails, with automatic retry on failure.

| Modality | Parts | Total Size | Download Time | Speed |
|----------|-------|-----------|--------------|-------|
| Audio | 11 | 19 GB | 631 s | 30.8 MB/s |
| Text | 13 | 24 GB | 882 s | 27.9 MB/s |
| Vision | 11 | 19 GB | 582 s | 33.4 MB/s |

Validation includes existence checks, non-zero file size verification, SHA-256 checksum validation, and automatic cleanup of partial downloads.

#### Phase 2 — Container Reassembly

Parts are merged directly into udocker via pipe command — avoiding intermediate file storage (saves **62+ GB** of disk space), preserving part order through sequential streaming, and validating container integrity during import.

#### Phase 3 — Runtime Execution

Standard volume mount configuration across all modalities:

| Host Path | Container Path | Purpose |
|-----------|---------------|---------|
| `/content/poc_[modality]` | `/workspace` | Source code |
| `/content/poc_[modality]/models` | `/workspace/models` | Model weights |
| `/content/poc_[modality]/results` | `/workspace/results` | Output directory |
| `/content/samples/[modality]` | `/workspace/samples` | Test data |
| `/content/UNESCO` | `/workspace/UNESCO` | Shared dataset |

#### Phase 4 — Benchmark Execution

Modality-specific commands:
- **Audio:** `python3 main.py files sample1.mp3 sample2.mp3`
- **Text:** `python3 main.py prompt "Translate to Hindi: 'Resilient AI is efficient.'"`
- **Vision:** `python3 main.py image bee_flower.jpg`

#### Phase 5 — Results Validation

Each benchmark generates a JSON results file with complete metadata for auditability. Sample outputs:

<details>
<summary>🎵 Sample Audio Result (MLK Mountaintop 1968)</summary>

```json
{
  "File": "mlk_mountaintop_1968.mp3",
  "RTF": 0.688,
  "RTF_Goal_Met": true,
  "WER": 0.17,
  "METEOR": 0.810,
  "Quality_Goal_Met": true,
  "VRAM_GB": 2.78,
  "VRAM_Goal_Met": true,
  "Energy_kWh": 7.645e-04,
  "Transcript": "thank you very kindly my friends"
}
```
</details>

<details>
<summary>📝 Sample Text Result (Sarvam-1)</summary>

```json
{
  "timestamp": "2026-03-17 21:46:00",
  "model_id": "sarvamai/sarvam-1",
  "metrics": {
    "quality_meteor": 0.9922,
    "vram_gb": 1.67,
    "rtf": 0.079,
    "energy_kwh": 2.088e-05,
    "co2_kg": 7.32e-06
  },
  "outputs": {
    "prompt": "Translate to Hindi: 'Resilient AI is efficient.'",
    "generated": "Pratirodhi AI kushal hai",
    "alternative_translations": [
      "Tikaau AI kushal hai",
      "Lacheela AI kushal hai"
    ]
  },
  "status": "PASS"
}
```
</details>

<details>
<summary>👁️ Sample Vision Result (Gemma-3B)</summary>

```json
{
  "status": "Success",
  "metadata": {
    "model": "google/gemma-3b-E2B-it",
    "seed": 12345,
    "timestamp": "2026-03-17 23:28:22"
  },
  "metrics": {
    "vram_gb": 2.23,
    "peak_gpu_gb": 2.23,
    "generation_time_sec": 10.212,
    "rtf": 0.408,
    "quality_score": 0.857,
    "energy_kwh": 2.173e-04,
    "co2_kg": 7.589e-05
  },
  "results": {
    "generated_text": "A vibrant pink cosmos flower with a bee collecting pollen, captured in close-up detail.",
    "expected_phrases": [
      "a bee on a pink flower",
      "a close-up of a bee on a flower",
      "a vibrant pink cosmos flower with a bee",
      "a bee collecting pollen from a flower"
    ],
    "phrase_matches": 4
  }
}
```
</details>

#### WP2 Key Achievements

- **Reliable Distribution** — 100% success rate across all 35 part downloads
- **Consistent Performance** — Metrics match original benchmarks within ±2%
- **Space Efficiency** — Saves 62+ GB disk space through pipe reassembly; 2.5× faster than file-based reassembly
- **User-Friendly Process** — Smart downloader with automatic retry and verification

| Modality | Parts Downloaded | Container Import | Benchmark |
|----------|-----------------|-----------------|-----------|
| Audio | 11/11 | ✅ | ✅ PASS |
| Text | 13/13 | ✅ | ✅ PASS |
| Vision | 11/11 | ✅ | ✅ PASS |

**ALL THREE MODALITIES VALIDATED SUCCESSFULLY ✅**

---

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
| **VISION** | Gemma 3n | `Gemma3n_UNESCO_FrankMorales.zip` | 0.857 | 2.23 GB | 0.408 | ✅ PASS |

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

[![License: CC BY-NC 4.0](https://img.shields.io/badge/License-CC%20BY--NC%204.0-lightgrey.svg?style=flat-square)](https://creativecommons.org/licenses/by-nc/4.0/)

© 2026 Frank Morales Aguilera — [Sovereign Machine Lab](https://sovereign-machine-lab.ai/)

This work is licensed under the **Creative Commons Attribution-NonCommercial 4.0 International License**.  
**All rights reserved. Provided for evaluation purposes only.**

You may share and adapt this material with appropriate attribution, but **commercial use is strictly prohibited**.  
→ [View Full License](https://creativecommons.org/licenses/by-nc/4.0/legalcode)

---

*Last Updated: March 18, 2026*
