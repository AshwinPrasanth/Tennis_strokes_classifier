# Tennis Stroke Classification from Video

This project studies tennis stroke classification under **limited data conditions** using four different modeling approaches.  
The goal is to understand how **spatial**, **spatiotemporal**, **pose-based**, and **hybrid residual** representations perform when training data is scarce.

The task is a **4-class classification problem**:
- Forehand
- Backhand
- Serve
- No-stroke

---

## Dataset Overview
- Short tennis video clips recorded from a fixed rear-court viewpoint
- Each clip is uniformly sampled into **16 frames**
- Resolution: **112×112** (for video models), **224×224** (for 2D CNN)
- Train / Validation / Test split at **video level** to avoid data leakage

---

## Approach 1 — 2D CNN (Spatial Baseline)

**Pipeline**  
Single Frame → 2D CNN → Class

**Description**  
This approach uses a **single RGB frame** from each video and ignores temporal information entirely.  
A lightweight 2D CNN is trained from scratch to learn **spatial cues** such as posture and racket position.

**Purpose**
- Acts as a spatial baseline
- Measures how much information is present in a single frame
- Provides a reference for temporal models

---

## Approach 2 — 3D CNN (Spatiotemporal Model)

**Pipeline**  
Video Clip (16 frames) → 3D CNN → Class

**Description**  
This model uses **3D convolutions** to jointly model **space and time**.  
Each input is a tensor of shape `[C, T, H, W]`, allowing the network to learn motion patterns directly.

**Purpose**
- Introduces explicit temporal modeling
- Highlights the data requirements of 3D CNNs trained from scratch
- Serves as a direct spatiotemporal baseline

---

## Approach 3 — Pose + LSTM (Keypoint-Based Temporal Model)

**Pipeline**  
Pose Keypoints (33 joints × 16 frames) → LSTM → Class

**Description**  
MediaPipe Pose is used to extract **33 anatomical keypoints per frame**.  
Each video is represented as a sequence of **66-dimensional vectors** (x, y per joint), processed by a **2-layer LSTM**.

**Purpose**
- Evaluates whether **skeletal motion alone** is sufficient
- Removes RGB appearance information entirely
- Serves as a pose-only temporal baseline

---

## Approach 4 — Hybrid R3D-18 with Synthetic Augmentation

**Pipeline**  
Real + Synthetic Clips → R3D-18 Backbone → Classifier Head

**Description**  
This approach fine-tunes a **pretrained R3D-18** video backbone.  
Training data is augmented using **temporally consistent synthetic transformations**, and real and synthetic clips are mixed using weighted sampling.

**Purpose**
- Combine strong pretrained temporal representations with aggressive augmentation
- Improve robustness and generalization in low-data settings
- Achieves the best overall performance among all approaches

---

## Model Checkpoints

The **best-performing model from each approach** is saved and can be loaded directly for evaluation or inference.

**Download links (Google Drive):**
- **Approach 1 (2D CNN):**  
  1. `https://drive.google.com/file/d/1QgRa5_QTxaruKAW8TfBMX47OUaopL2A7/view?usp=sharing`
- **Approach 2 (3D CNN):**  
  2. `https://drive.google.com/file/d/1VNm11YjHjnBzNzMrKNBEmbybEtBfwb0R/view?usp=sharing`
- **Approach 3 (Pose + LSTM):**  
  3. `https://drive.google.com/file/d/1T-rZsBf8rzXAC0ojLmOP2QYSIlw_Uso_/view?usp=sharing`
- **Approach 4 (Hybrid R3D-18)- Best approach:**  
  4. `https://drive.google.com/file/d/1QUhFuDdtWQftnLfahbqnzBObpvIyj4K1/view?usp=sharing`


---
