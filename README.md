# Image Color Quantization with K-Means Clustering

Can an unsupervised algorithm reduce a photograph to 6 colors while keeping it visually recognizable? This project applies K-Means clustering to compress the color palette of an image from millions of unique RGB values down to a fixed set of representative colors, demonstrating a practical use case for unsupervised learning in image processing.

---

## Overview

Color quantization is the process of reducing the number of distinct colors in an image. It is used in image compression, palette generation for graphic design tools, and retro-style visual effects. Rather than predicting a label, K-Means here groups pixels by color similarity and replaces each pixel's RGB value with the centroid of its assigned cluster.

The same algorithm used for customer segmentation or document clustering applies directly to image pixels: each pixel is a data point with three features (R, G, B), and the cluster centroids become the new color palette.

---

## Workflow

### Part 1: Load and Inspect
The image is loaded as a NumPy array of shape (height, width, 3), where each element is a pixel's RGB triplet with values from 0 to 255. A standard photograph at this resolution contains hundreds of thousands of unique color combinations.

### Part 2: Reshape to 2D
K-Means expects a 2D input matrix where each row is one observation. The 3D image array is reshaped from (height, width, 3) to (height x width, 3), treating each pixel as a single data point with three color channel features.

### Part 3: Fit K-Means (k=6)
K-Means is fit on the full pixel matrix with k=6 clusters. Each centroid learns the mean RGB value of all pixels assigned to it, identifying the 6 most representative colors in the image. `fit_predict` returns a cluster label for every pixel.

### Part 4: Reconstruct the Quantized Image
Each pixel's original RGB value is replaced with the centroid of its cluster. Indexing `rgb_codes[labels]` maps every pixel label to its cluster's RGB centroid. Reshaping back to (h, w, c) restores the 2D image structure, producing the quantized output.

### Part 5: Comparison
The original and quantized images are displayed side by side. The quantized version retains overall structure and composition using only 6 colors, with smooth gradients replaced by flat color regions.

---

## Results

With k=6, the algorithm identifies the dominant color palette of the image and reconstructs it faithfully enough to preserve subject recognition. Gradients and smooth color transitions are reduced to discrete flat regions, producing a posterized visual effect.

Increasing k improves color fidelity at the cost of a larger palette. Decreasing k produces more stylized, abstract results.

---

## Key Concepts

**Unsupervised Application:** No labels are used. K-Means operates purely on the geometric structure of the RGB color space, grouping pixels by proximity in three-dimensional color coordinates.

**Reshape as Feature Engineering:** The reshape step is the critical transformation that makes this work. It reframes a spatial image structure as a tabular dataset where observations are pixels and features are color channels.

**Centroid as Representative Color:** Each cluster centroid is the mean RGB value of all pixels assigned to it. Replacing every pixel with its centroid color is equivalent to projecting the entire color space onto k representative points.

**Effect of k:** k controls the tradeoff between compression and fidelity. Very low k values (2 to 4) produce abstract, graphic results. Higher k values (16 to 32) approach the quality of the original. The right value depends on the use case.

---

## Stack

- Python 3
- NumPy
- Matplotlib (image I/O and display)
- scikit-learn (KMeans)

---

## File Structure

```
kmeans-color-quantization/
├── kmeans_color_quantization.ipynb   # Main project notebook
├── palm_trees.jpg                    # Source image
└── README.md
```

---

## How to Run

1. Clone the repository
2. Install dependencies: `pip install numpy matplotlib scikit-learn`
3. Place the source image (`palm_trees.jpg`) in the same directory as the notebook
4. Open `kmeans_color_quantization.ipynb` in Jupyter and run all cells
