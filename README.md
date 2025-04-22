# PlantSegNet – In‑the‑Wild Plant‑Disease Segmentation  
*CSE 428 · Image Processing · Fall 2024*

---

## 1 · Project Snapshot  
Modern crop‑monitoring apps can flag *whether* a plant is unhealthy, but rarely *where* the infection sits on the leaf or stem.  
**PlantSegNet** bridges that gap: it predicts a pixel‑precise mask that highlights diseased tissue even under messy, real‑world shooting conditions (variable lighting, cluttered backgrounds, occlusion).

---

## 2 · Dataset  
Please read the dataset paper for further details. (PlantSeg)

* **Split** – 56 % train (8 019) · 14 % val (1 603) · 30 % test (3 437)  
* **Classes** – four high‑level crop groups (profit, staple, fruit, vegetable) retained for coarse statistics; segmentation is binary (disease vs. background/healthy).

---

## 3 · Model & Training Recipe  
| Component | Choice | Rationale |
|-----------|--------|-----------|
| **Backbone** | ResNet‑34 encoder (pre‑trained on ImageNet) | strong feature extractor with modest footprint |
| **Decoder** | Five‑stage up‑sampling with skip connections + bottleneck compression | preserves spatial detail without heavy compute |
| **Loss** | Binary Cross‑Entropy | stable for highly imbalanced foreground/background pixels |
| **Optimizer** | Adam (1 e‑4) + cosine LR decay | quick convergence, smooth fine‑tuning |
| **Augmentation** | Random flips, color‑jitter, mix‑up | mimics lighting / viewpoint variance in the field |

Training converged in ~30 epochs

---

## 4 · Results  
| Split | IoU ↑ | Dice ↑ | BCE Loss ↓ |
|------ |------:|-------:|-----------:|
| **Validation** | **0.81** | **0.89** | **0.12** |
| **Test**       | 0.79 | 0.87 | 0.14 |

*Qualitative highlights*  
* Masks remain crisp under motion blur and complex foliage.  
* Most failure cases involve severe back‑lighting or disease regions < 2 % of the frame.


> This repository hosts all training notebooks, model code, and evaluation scripts corresponding to the results above. Feel free to explore—or fork it for your own agri‑vision projects!
