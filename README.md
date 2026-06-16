# MNIST Autoencoder & PCA Reconstruction

This was a group project (me and Vishwa) where we explored different ways to compress and reconstruct handwritten digit images from the MNIST dataset. The idea was to compare traditional dimensionality reduction (PCA) against neural network-based autoencoders and see which one does a better job at recreating the original images.

---

## What We Did

We worked through three main tasks:
1. Used PCA to reduce MNIST images to 2D and 32D
2. Built a 1-layer autoencoder and trained it on MNIST
3. Built a deeper autoencoder and tested different activation functions

---

## Dataset

- **MNIST** — 5,000 samples of handwritten digits (0–9)
- Each image is 28×28 pixels, flattened to 784 features for processing

---

## Task 1 — PCA

We reduced the 784 features down to just 2 dimensions first (for visualization) then to 32 dimensions (for reconstruction). The 2D scatter plot showed how well digits naturally cluster in lower-dimensional space. For reconstruction, we used inverse PCA transform to get the images back.

| Metric | Value |
|--------|-------|
| MSE (32D Reconstruction) | 0.01684 |
| Explained Variance (32D) | 74.91% |

The reconstructions looked pretty close to the originals, which was surprising for just 32 dimensions capturing 75% of the variance.

---

## Task 2 — 1-Layer Autoencoder

We built a simple autoencoder in PyTorch with 32 neurons in the bottleneck. Encoder used ReLU, decoder used Sigmoid. Trained with Adam optimizer and MSE loss.

We first tried 20 epochs — results were okay but not great. Bumped it up to 30 epochs and saw a clear improvement.

| Epochs | MSE Loss |
|--------|----------|
| 20 epochs | 0.028207 |
| 30 epochs | 0.010472 |

The 30-epoch reconstructions were sharp and very close to the originals. The autoencoder also beat PCA on reconstruction quality.

| Method | MSE Loss |
|--------|----------|
| PCA (32D) | 0.016773 |
| 1-Layer AE (30 Epochs) | 0.010472 |

---

## Task 3 — Deep Autoencoder

We extended the 1-layer AE into a deeper network:
- **Encoder:** 784 → 256 → 64 → 32
- **Decoder:** 32 → 64 → 256 → 784

We tested three activation functions to see which works best:

| Activation Function | MSE Loss (20 Epochs) |
|--------------------|----------------------|
| ReLU | 0.020968 |
| Tanh | 0.028900 |
| Sigmoid | 0.060763 |

ReLU won by a good margin. The deep autoencoder with ReLU gave cleaner reconstructions than both PCA and the shallow AE at the same epoch count.

---

## Final Comparison

| Model | MSE Loss |
|-------|----------|
| PCA (32D) | 0.016773 |
| 1-Layer AE (20 Epochs) | 0.028207 |
| 1-Layer AE (30 Epochs) | 0.010472 |
| Deep AE – ReLU (20 Epochs) | 0.020968 |
| Deep AE – Tanh (20 Epochs) | 0.028900 |
| Deep AE – Sigmoid (20 Epochs) | 0.060763 |

The 1-layer autoencoder with 30 epochs got the lowest MSE overall. But comparing fairly at the same 20 epochs, the deep autoencoder with ReLU outperforms everything else — which makes sense since deeper networks capture more complex patterns.

---

## What We Learned

PCA is surprisingly competitive for a non-learned method. But autoencoders, especially deeper ones with the right activation function and enough training, clearly produce better reconstructions. ReLU consistently outperformed Sigmoid and Tanh for this task, and more epochs always helped.

---

## Tech Stack

- Python
- PyTorch
- scikit-learn (PCA)
- Matplotlib
- NumPy

---

**Ravya Vangaveti & Vishwa**  
Artificial Intelligence Course Project  
University of North Carolina at Greensboro
