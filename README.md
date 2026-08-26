# EfficientCaps: A Lightweight Capsule Network for Robust Traffic Sign Recognition Under Real-World Degradation

**Author:** Mansoureh Fahimi

EfficientCaps is a lightweight Capsule Network for robust traffic sign recognition. The model is trained from scratch on clean GTSRB data and evaluated on challenging real-world degraded traffic signs without using degraded images during training.

## Highlights

- **99.1%** clean GTSRB accuracy
- **96.5%** Macro F1
- **51.5% (17/33)** accuracy on the real-world degraded evaluation set
- **14.20M** parameters
- **35 MFLOPs/image**
- **166 FPS** reported inference speed
- Depthwise-separable convolution backbone
- Efficient single-step capsule routing
- Weighted margin loss, balanced sampling, and hard-class augmentation
- Reliability-aware inference pipeline

## Motivation

Traffic-sign recognition is safety-critical in autonomous driving and driver-assistance systems. Although modern models achieve very high accuracy on clean benchmark datasets, real-world traffic signs may appear under snow, rain, blur, poor lighting, physical damage, graffiti, or other degradation.

This project investigates whether a model trained only on clean traffic-sign images can still generalize to challenging real-world degraded signs through architecture, training strategy, and inference-time processing.

## Contributions

- **EfficientCaps:** a lightweight capsule-based architecture trained from scratch on GTSRB without pre-training.
- **Efficient feature extraction:** depthwise-separable convolution blocks reduce feature-extraction cost.
- **Single-step routing:** replaces computationally expensive iterative capsule routing with a learned single routing step.
- **Class-aware training:** weighted margin loss, balanced sampling, and stronger augmentation for difficult classes.
- **Real-world robustness evaluation:** degraded traffic signs are used for evaluation rather than degraded classifier training.
- **Reliability-aware inference:** condition detection, Test-Time Augmentation (TTA), capsule-based reliability signals, and disambiguation.

## Architecture

```text
48×48 RGB Input
      ↓
5×5 Convolution + BatchNorm + ReLU
      ↓
Depthwise-Separable Convolution Block
      ↓
Depthwise-Separable Convolution Block
      ↓
Primary Capsules
      ↓
Efficient Single-Step Routing
      ↓
43 × 16-D Class Capsules
      ↓
L2 Norm of Capsule Length
      ↓
Traffic-Sign Classification
```

The length of each class-capsule vector is used as its class score. A reconstruction decoder is used during training as a regularizer.

## Training Configuration

| Setting | Value |
|---|---:|
| Input size | 48 × 48 |
| Classes | 43 |
| Batch size | 128 |
| Epochs | 30 |
| Optimizer | AdamW |
| Learning rate | 0.001 |
| Weight decay | 0.0001 |
| Scheduler | CosineAnnealingLR |
| Margin m+ | 0.9 |
| Margin m− | 0.1 |
| Alpha | 0.5 |
| Reconstruction weight | 0.0005 |
| Pretrained | No |

## Real-World Degraded Evaluation

The final model is trained on clean GTSRB data and then evaluated on a curated set of **33 challenging real-world degraded traffic-sign images**.

**EfficientCaps V3: 17/33 correct — 51.5% accuracy.**

This experiment focuses on generalization to real-world degradation without training the classifier directly on degraded examples.

## Computational Efficiency

EfficientCaps reduces the computational overhead associated with traditional Capsule Networks through depthwise-separable convolutions and single-step learned routing.

| Measurement | Reported result |
|---|---:|
| Parameters | **14.20 M** |
| FLOPs / image | **35 MFLOPs** |
| Inference speed | **166 FPS** |
| Inference GPU memory | **90.7 MB** |

## Repository Notice

This public repository presents the project, architecture, methodology, and experimental results. Complete training implementation, trained checkpoints, private datasets, and selected implementation details may be withheld from the public repository.

## Author

**Mansoureh Fahimi**

Copyright © 2026 Mansoureh Fahimi. All Rights Reserved.

See [`LICENSE`](LICENSE) for usage restrictions.
