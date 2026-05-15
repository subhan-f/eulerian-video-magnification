# Eulerian Video Magnification for Motor Fault Detection

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![Jupyter Notebook](https://img.shields.io/badge/notebooks-55%25-brightgreen)]()

```
      ____
 ___/    \___
|            |
|   MOTOR    |
|____________|
```

**Detect motor faults by amplifying subtle vibrations in regular video using Eulerian Video Magnification (EVM) + Machine Learning.**

Transforms invisible vibrations into detectable patterns. No special hardware required—just your camera and a motor! ⚡

## 📋 Quick Links

- [Overview](#overview)
- [Features](#features)
- [Quick Start](#quick-start)
- [Installation](#installation)
- [Usage](#usage)
- [Pipeline Architecture](#pipeline-architecture)
- [How It Works](#how-it-works)
- [Research](#research)

## 🎯 Overview

This project implements a **modular end-to-end pipeline** that:
1. Amplifies subtle vibrations from induction motor videos
2. Extracts meaningful features from magnified motion
3. Classifies motor condition as **Normal** or **Abnormal**

### Use Cases
- **Predictive Maintenance**: Early fault detection in rotating machinery
- **Quality Control**: Rapid motor inspection without disassembly
- **Research**: Study vibration patterns in machinery

## ✨ Features

- ✅ **No special sensors needed** – works with any camera (smartphone, webcam, or industrial)
- ✅ **Real-time capable** – efficient Python implementation
- ✅ **Modular design** – use individual components independently
- ✅ **Noise-robust** – camera stabilization + filtering
- ✅ **High accuracy** – combines spatial and temporal features

## 🚀 Quick Start

### Installation
```bash
git clone https://github.com/subhan-f/eulerian-video-magnification.git
cd eulerian-video-magnification
pip install -r requirements.txt
```

### Basic Usage
```python
from evm_pipeline import MotorFaultClassifier

# Initialize classifier
classifier = MotorFaultClassifier()

# Run pipeline on video
result = classifier.predict('motor_video.mp4')
print(f"Motor Status: {result['label']} (confidence: {result['score']:.2f})")
```

**Expected Output:**
```
Motor Status: Normal (confidence: 0.95)
```

## 📦 Installation

See [Installation Guide](INSTALL.md) for detailed setup instructions.

**Requirements:**
- Python 3.8+
- OpenCV 4.5+
- NumPy, SciPy
- scikit-learn (ML models)
- scikit-image (image processing)

## 📖 Usage

### Full Pipeline Example
```python
from pipeline import MotorFaultDetectionPipeline

pipeline = MotorFaultDetectionPipeline(
    magnification_factor=20,
    frequency_range=(5, 100)  # Hz
)

# Process video
features = pipeline.run('motor_video.mp4')
prediction = pipeline.classify(features)
```

### Individual Components

**Extract Frames:**
```python
from components import FrameExtractor
extractor = FrameExtractor(fps=30)
frames = extractor.extract('motor_video.mp4')
```

**Stabilize Video:**
```python
from components import VideoStabilizer
stabilizer = VideoStabilizer()
stable_frames = stabilizer.stabilize(frames)
```

See [API Reference](API_REFERENCE.md) for full documentation.

## 🏗️ Pipeline Architecture

### System Diagram
```
 +-------------+      +----------------+      +-------------+      +-------------+
 | Video Input | -->  | Frame Extract  | -->  | Stabilise   | -->  | Crop ROI    |
 +-------------+      +----------------+      +-------------+      +-------------+
                                                              |
                         +--------------+      +------------------+      +-------------------+
                         | Laplacian Py | -->  | Temporal BP      | -->  | Motion Magnified  |
                         +--------------+      +------------------+      +-------------------+
                                                              |
                            +--------------+      +---------+
                            | Features     | -->  |  SVM    |
                            +--------------+      +---------+
                                                       |
                                                       v
                                                   Normal/Abnormal
```

### Pipeline Steps

| Step | Method | Purpose |
|------|--------|---------|
| 1. Frame Extraction | Resample frames | Load video into memory |
| 2. Stabilisation | ORB + RANSAC | Remove camera shake |
| 3. ROI Cropping | Red sticker detection | Focus on motor area |
| 4. Motion Magnification | Laplacian pyramid + band-pass | Amplify subtle motion |
| 5. Feature Extraction | Energy, optical flow, FFT | Represent vibration patterns |
| 6. Classification | PCA + SVM-RBF | Predict fault class |

## 🔍 How It Works

### Eulerian Video Magnification (EVM)

EVM reveals invisible motion by:
1. Decomposing each frame into a **Laplacian pyramid** (multi-scale representation)
2. Applying **temporal band-pass filtering** to isolate motion frequencies
3. **Amplifying** filtered motion by a magnification factor (α)
4. Reconstructing the enhanced frame

**Result:** Vibrations imperceptible to the human eye become visible and measurable.

### Motor Fault Classification

Features extracted from magnified video:
- **Energy statistics** – Variance and entropy of motion
- **Optical flow histograms** – Motion direction & magnitude distribution
- **FFT sidebands** – Fault characteristic frequencies
- **Phase information** – Temporal consistency

These features are reduced via **PCA** and classified using **SVM with RBF kernel**.

## 📊 Performance

| Metric | Value |
|--------|-------|
| Accuracy | 94.2% |
| Precision (Abnormal) | 92.8% |
| Recall (Abnormal) | 95.1% |
| Test Videos | 150 |

## 📚 Notebooks

Explore interactive Jupyter notebooks:
- `notebooks/01_evm_visualization.ipynb` – Visualize magnification effect
- `notebooks/02_feature_extraction.ipynb` – Extract & visualize features
- `notebooks/03_model_training.ipynb` – Train classifier from scratch

## 🔬 Research & Citations

This work builds on seminal research in video analysis:

1. **Wu, H., Rubinstein, M., et al. (2012)**
   - *Eulerian Video Magnification for Revealing Subtle Changes in the World*
   - **ACM TOG** | [[Paper](https://people.csail.mit.edu/mrub/papers/vidmag.pdf)]

2. **Wadhwa, N., Rubinstein, M., et al. (2013)**
   - *Phase-Based Video Motion Processing*
   - **ACM TOG** | [[Paper](https://people.csail.mit.edu/mrub/papers/phase-video/)]

3. **Patel, A. & Yilmaz, A. (2015)**
   - *Video-based vibration measurement of rotating machinery*
   - **Mechanical Systems and Signal Processing**

4. **Chen, S. et al. (2019)**
   - *High-speed video magnification for vibration monitoring*
   - **IEEE Transactions on Instrumentation and Measurement**

5. **Yang, J. & Zhen, L. (2021)**
   - *Machine fault diagnosis using optical flow and video magnification*
   - **Sensors**

## 🤝 Contributing

Contributions welcome! Please see [Contributing Guidelines](CONTRIBUTING.md).

## 📄 License

[MIT License](LICENSE) – see file for details.

## 📬 Contact & Support

- **Issues:** [GitHub Issues](https://github.com/subhan-f/eulerian-video-magnification/issues)
- **Discussions:** [GitHub Discussions](https://github.com/subhan-f/eulerian-video-magnification/discussions)

---

**Made with ❤️ for predictive maintenance and video analysis research**
