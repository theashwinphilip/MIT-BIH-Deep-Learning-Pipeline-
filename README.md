# MIT-BIH-Deep-Learning-Pipeline-
ECG Waveform Classification using Deep Learning Pipeline on MIT-BIH Dataset
Deep Learning for ECG Arrhythmia Classification

A state-of-the-art deep learning system for automatic classification of cardiac arrhythmias from ECG signals using CNN-LSTM architecture with attention mechanism. 

Overview

This project implements a comprehensive pipeline for ECG arrhythmia classification using deep learning. The system processes raw ECG signals, detects heartbeats, segments PQRST waves, extracts both learned and handcrafted features, and classifies beats into five AAMI categories.

AAMI Beat Classes

| Class |    Description   |    Examples    |
|-------|------------------|----------------|
|   N   | Normal           | N, L, R, e, j  |
|   S   | Supraventricular | A, a, J, S     |
|   V   | Ventricular      | V, E           |
|   F   | Fusion           | F              |
|   Q   | Unknown          | /, f, Q        |

Key Features

Signal Processing
- Bandpass filtering (0.5-50 Hz) for noise removal
- Baseline wander removal
- Adaptive R-peak detection with threshold optimization
- PQRST wave segmentation with validation
- Z-score normalization per lead

Deep Learning Architecture
- Dual-branch CNN-LSTM model (855K parameters)
- 3-layer CNN for morphological feature extraction
- 2-layer Bidirectional LSTM for temporal modeling
- Attention mechanism for interpretability
- Feature fusion with handcrafted features
- Class-weighted training for imbalanced data

Feature Engineering
- 10 morphological features (R-peak amplitude, slopes, energy, etc.)
- 10 frequency features (FFT coefficients)
- Clinical interval measurements (RR, QRS, PR, QT, QTc)
- Heart rate variability metrics

Comprehensive Visualization
- Beat-level predictions with waveform display
- Confusion matrix analysis
- ROC curves for all classes
- Confidence distribution analysis
- Error pattern identification
- High-confidence error detection

Architecture

Model Architecture Diagram

INPUT: ECG Beat (360 samples, 2 leads)
    ↓
┌─────────────────────────────────────┐
│  CNN FEATURE EXTRACTION             │
│  • Conv1D(32, 7) → MaxPool → BN     │
│  • Conv1D(64, 5) → MaxPool → BN     |
│  • Conv1D(128, 3) → MaxPool → BN    │
│  Output: (45, 128)                  │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│  BIDIRECTIONAL LSTM                 │
│  • BiLSTM(128, return_seq=True)     │
│  • BiLSTM(128, return_seq=True)     │
│  Output: (45, 256)                  │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│  ATTENTION MECHANISM                │
│  • Dense(128) → tanh                │
│  • Dense(1) → Softmax               │
│  • Weighted sum                     │
│  Output: (256,)                     │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│  FEATURE FUSION                     │
│  • Handcrafted features: (20,)      │
│  • Concat [CNN-LSTM, Features]      │
│  Output: (320,)                     │
└─────────────────────────────────────┘
    ↓
┌─────────────────────────────────────┐
│  CLASSIFICATION                     │
│  • Dense(256) → BN → Dropout        │
│  • Dense(128) → BN → Dropout        │
│  • Dense(5) → Softmax               │
│  Output: Class probabilities        │
└─────────────────────────────────────┘

Dataset

MIT-BIH Arrhythmia Database

- Source       : [PhysioNet](https://physionet.org/content/mitdb/1.0.0/)
- Patients     : 48 (47 subjects, 30 minutes each)
- Total Beats  : 109,433 annotated beats
- Sampling Rate: 360 Hz
- Leads        : MLII (primary), V5 (secondary)

Contributing

We welcome contributions! Please follow these steps:

1. Fork the repository
2. Create a feature branch
   bash
   git checkout -b feature/amazing-feature
   
3. Commit your changes
   bash
   git commit -m "Add amazing feature"
   
4. Push to the branch
   bash
   git push origin feature/amazing-feature
   ```
5. Open a Pull Request

Contribution Guidelines

- Write clear commit messages
- Add tests for new features
- Update documentation
- Follow PEP 8 style guide
- Ensure all tests pass

References

Papers

1. **MIT-BIH Arrhythmia Database**
   - Moody GB, Mark RG. *The impact of the MIT-BIH Arrhythmia Database.* IEEE Eng Med Biol Mag. 2001.

2. **AAMI Standards**
   - Association for the Advancement of Medical Instrumentation. *Testing and reporting performance results of cardiac rhythm and ST segment measurement algorithms.* ANSI/AAMI EC57:1998.

3. **Deep Learning for ECG**
   - Hannun AY, et al. *Cardiologist-level arrhythmia detection and classification in ambulatory electrocardiograms using a deep neural network.* Nature Medicine. 2019.

4. **Attention Mechanisms**
   - Vaswani A, et al. *Attention is all you need.* NeurIPS. 2017.

### Datasets

- **MIT-BIH Arrhythmia Database**: https://physionet.org/content/mitdb/1.0.0/
- **PhysioNet**: https://physionet.org/

### Tools & Libraries

- **TensorFlow**: https://tensorflow.org/
- **WFDB**: https://github.com/MIT-LCP/wfdb-python
- **BioSPPy**: https://github.com/PIA-Group/BioSPPy

---

