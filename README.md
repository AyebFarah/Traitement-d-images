# Image Processing Labs — OpenCV & scikit-image

Three hands-on labs in image processing with Python. They go from basic image handling to contrast enhancement and image segmentation. The last lab measures segmentation quality against ground-truth masks.

The notebooks are in French.

## Contents

| Notebook | Topic |
|---|---|
| `Traitement_d’images.ipynb` | **TP1:** OpenCV basics |
| `Egalisation.ipynb` | **TP2:** Histogram equalization and contrast enhancement |
| `Segmentation_d'images.ipynb` | **TP3:** Image segmentation and evaluation |

## TP1: OpenCV Basics

- Load, display, and save images.
- Fix the BGR/RGB color order between OpenCV and Matplotlib.
- Convert images to grayscale.
- Resize, crop, rotate, and translate images.
- Split RGB channels and build masks for the dominant color.
- Create an image from a NumPy array.

## TP2: Histogram Equalization

- Plot histograms and CDFs.
- Apply global equalization (HE) with OpenCV.
- Apply adaptive equalization (AHE) with scikit-image.
- Tune CLAHE (`clipLimit`, `tileGridSize`) and compare the results.
- Reproduce four methods from Salem et al. (2019) on retina images:
  - **HE**: Histogram Equalization
  - **CHE**: Cumulative Histogram Equalization (written from scratch)
  - **QDHE**: Quadrant Dynamic Histogram Equalization (written from scratch)
  - **CLAHE**: Contrast Limited Adaptive Histogram Equalization
- Score each method with MSE, PSNR, and standard deviation.

**Result:** CLAHE gives the best trade-off on retina images. This matches the paper.

## TP3: Image Segmentation

### Part 1: Methods on test images

- **Preprocessing:** CLAHE, Gaussian blur vs. mean blur (3×3, 5×5, 9×9).
- **Edge detection:** Sobel gradients, gradient thresholding, Otsu, and Canny.
- **Line detection:** Probabilistic Hough transform.
- **Thresholding:** Manual threshold from histogram peaks vs. Otsu.
- **K-means clustering:** Tested with k = 2 to 8.
- **Region growing:** Written from scratch. It uses 8-connectivity, a BFS queue, and a running region mean. You pick the seed with a mouse click. It also handles multi-region segmentation.

### Part 2: Evaluation on a labeled dataset

The dataset has four categories: `1objet`, `2objets`, `Embryons`, and `textfort`. Each image has a ground-truth mask.

The pipeline:
1. Apply one of four preprocessing options: none, Gaussian, median, or CLAHE.
2. Segment with Otsu and with region growing.
3. Compare each mask to the ground truth with the **Jaccard index** and the **Dice coefficient**.
4. Average the scores per dataset and per method.

### Results (mean scores)

| Method | Preprocessing | Jaccard | Dice |
|---|---|---|---|
| Otsu | Median | **0.671** | **0.753** |
| Otsu | Gaussian | 0.668 | 0.751 |
| Otsu | None | 0.643 | 0.730 |
| Otsu | CLAHE | 0.528 | 0.644 |
| Region growing | Median | 0.302 | 0.386 |
| Region growing | Gaussian | 0.286 | 0.367 |
| Region growing | None | 0.260 | 0.337 |
| Region growing | CLAHE | 0.105 | 0.145 |

**Key findings:**
- Otsu beats region growing on every dataset.
- Median and Gaussian filtering give the best scores.
- CLAHE hurts results here because it also boosts noise.
- `2objets` scores highest (Dice up to 0.93).
- `Embryons` scores lowest (Dice ≈ 0.33). The images have low contrast and complex shapes.
- Region growing depends heavily on the seed and the threshold.

## Tech Stack

- Python 3
- OpenCV
- NumPy, pandas, SciPy
- scikit-image, scikit-learn
- Matplotlib, Pillow
- Google Colab

## Run the Notebooks

1. Install the dependencies:
   ```bash
   pip install opencv-python numpy pandas scipy scikit-image scikit-learn matplotlib pillow
   ```
2. Put the test images next to the notebooks (`Cat.jpg`, `fleur.png`, `vallee.png`, the retina images, `feuille.png`, `outil.png`, `mandrillgray.jpg`).
3. TP3 reads the dataset from Google Drive. Update `BASE_PATH` if you run it locally. Remove the `google.colab` import too.

The images and the dataset are not in this repo.

## Author

**Farah Ayeb**, Networks & Telecommunications Engineering, INSAT
