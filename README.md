```markdown
# UNESCO2026 - Resilient AI Challenge Test Repository

This repository contains the **validated test uploads** for the Text modality of the UNESCO Resilient AI Challenge. It demonstrates the complete workflow for splitting, uploading, and downloading large containerized AI models using GitHub Releases.

## 📦 Available Test Container

### Text Modality (Sarvam-1)
- **Model**: `sarvamai/sarvam-1`
- **Container Size**: 24 GB (exported from udocker)
- **Release**: [`text-modality-v1.0.0`](https://github.com/frank-morales2020/UNESCO2026/releases/tag/text-modality-v1.0.0)
- **Parts**: 13 parts (12 × 1.9GB + 1 × 1.1GB)
- **Status**: ✅ Fully validated and working

### 📊 Validated Performance
| Metric | Value | Target | Status |
|--------|-------|--------|--------|
| **Quality (METEOR)** | 0.9922 | > 0.5 | ✅ PASS |
| **VRAM Usage** | 1.668 GB | < 4.0 GB | ✅ PASS |
| **RTF** | 0.076 | < 1.0 | ✅ PASS |

## 🚀 How to Use This Container

### Prerequisites
- Install [udocker](https://github.com/indigo-dc/udocker):
  ```bash
  pip install udocker
  udocker --allow-root install
  ```

### Method 1: Smart Download (Auto-detects all parts)
Run this Python script in Colab or any Python environment:
```python
import os
i = 0
base_url = "https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0"
while True:
    part = f"text-part-{i:03d}"
    url = f"{base_url}/{part}"
    !wget -q $url
    if os.path.exists(part) and os.path.getsize(part) > 0:
        print(f"Downloaded {part}")
        i += 1
    else:
        !rm -f $part
        break
print(f"✅ Downloaded {i} parts")
```

### Method 2: Manual Download (Bash)
```bash
for i in {000..012}; do
    wget "https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0/text-part-$i"
done
```

### Method 3: Using GitHub CLI
```bash
gh release download text-modality-v1.0.0 \
    --repo frank-morales2020/UNESCO2026 \
    --pattern "text-part-*"
```

### After Downloading All Parts
```bash
# 1. Reassemble the container
cat text-part-* > somala-text.tar

# 2. Load into udocker
udocker --allow-root load -i somala-text.tar

# 3. Run the benchmark
udocker --allow-root run sarvam_unesco
```

### Complete Colab Example
```python
# Full working example for Google Colab
!pip install -q udocker
!udocker --allow-root install

# Smart download
import os
i = 0
while True:
    part = f"text-part-{i:03d}"
    url = f"https://github.com/frank-morales2020/UNESCO2026/releases/download/text-modality-v1.0.0/{part}"
    !wget -q $url
    if os.path.exists(part) and os.path.getsize(part) > 0:
        print(f"Downloaded {part}")
        i += 1
    else:
        !rm -f $part
        break
print(f"✅ Downloaded {i} parts")

# Reassemble and run
!cat text-part-* > somala-text.tar
!udocker --allow-root load -i somala-text.tar
!udocker --allow-root run sarvam_unesco
```

## 🔧 Technical Details
- **Base Image**: `nvcr.io/nvidia/cuda:12.8.0-cudnn-devel-ubuntu24.04`
- **Python Version**: 3.12
- **Key Libraries**: 
  - PyTorch 2.10.0
  - Flash Attention 2.8.3
  - Transformers 4.47.0
  - BitsAndBytes (4-bit quantization)
- **Attention Kernel**: Flash Attention 2 (falls back to SDPA if unavailable)

## 📝 Notes
- This is a **test repository** for validating the container upload/download workflow
- The same process will be used for Audio and Vision modalities
- For the main submission, see the [SOMALA-UNESCO-Resilient-AI](https://github.com/frank-morales2020/SOMALA-UNESCO-Resilient-AI) repository

## 📧 Contact
**Frank Morales Aguilera** — Sovereign Machine Lab  
frank.morales@sovereign-machine-lab.ai
```

### How to Update the File
1. Go to your README file on GitHub: https://github.com/frank-morales2020/UNESCO2026/blob/main/README.md
2. Click the pencil icon (✏️) to edit
3. Delete the existing "DOCKE TEST" text
4. Paste the entire content above
5. Scroll down and click "Commit changes"

This README now provides:
- ✅ Clear description of the test container
- ✅ Multiple download methods (smart Python, bash, GitHub CLI)
- ✅ Complete usage instructions
- ✅ Validation results
- ✅ Technical details
- ✅ Working Colab example
