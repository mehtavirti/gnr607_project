# GNR607 Project: Airplane Detection using Template Matching (NCC) with DFT High-Boost Filtering

Detects airplanes in aerial images. YOLOv8-OBB picks one airplane crop as the template, then normalized cross-correlation (NCC, computed with FFTs) searches the whole image at several rotations. The matching is run on the original image and on a DFT high-boost sharpened image, and the two are compared.

## How to run

1. **Download the notebook** `gnr607.ipynb` from the GitHub repository:
   https://github.com/mehtavirti/gnr607_project
   (open the file on GitHub and click *Download raw file*, or clone the repo).
2. **Open it in Google Colab** (recommended) or Jupyter / Kaggle.
   In Colab: *File → Upload notebook* and select `gnr607.ipynb`.
3. **Run the cells one by one, from top to bottom** (*Runtime → Run all* also works).
   - The first cell installs the required libraries, downloads all images from the repository into the current runtime folder and removes the temporary clone. No manual image upload is needed.
   - The following cells define the helper functions, detect airplanes with YOLO, pick a template, apply the filters, run NCC matching and print and plot the results.
4. Read the printed detections (position, angle, NCC score) and the summary comparing the original and DFT-sharpened results.

## Requirements

- Python 3.8 or newer
- An internet connection (to clone the repo and to download the YOLO weights on the first run)

Libraries (the first cell installs `ultralytics`, which brings in most of the rest):

| Library | Purpose |
|---|---|
| `ultralytics` | YOLOv8-OBB model (`yolov8n-obb.pt` is downloaded automatically) |
| `numpy` | arrays and FFT |
| `opencv-python` (`cv2`) | image reading, grayscale conversion, template rotation |
| `matplotlib` | plots |
| `torch` | YOLO backend (installed with ultralytics) |

Colab already has numpy, opencv and matplotlib. If you run locally, install everything with:

```
pip install ultralytics numpy opencv-python matplotlib
```

`git` must be available (it is in Colab) for the image download cell.

## Images

`img1.jpg`, `img2.jpeg`, `img3.jpg`, `img4.jpg`, `img5.jpg` are pulled from the repository into the working directory by the first cell.

## Main parameters (set in the notebook)

| Parameter | Meaning |
|---|---|
| `threshold` | minimum NCC score to accept a detection (0.6) |
| `ANGLES` | template rotations tried, in degrees (0, 90, 180, 270) |
| `A`, `D0` | high-boost factor and Gaussian cutoff of the DFT filter |
| `k`, `sigma` | gain and blur width of unsharp masking on the template |
| `idx` | which YOLO detection is used as the template |

## Notes

- The images are large (about 13 MP), so each NCC run takes roughly 30 seconds per image with 4 angles.
- The template is cropped from the same image, so the template's own location always gives a near-perfect match (NCC close to 1).
- NCC is sensitive to size and orientation. Planes that look similar but differ in size or angle give low correlation.

## Course

GNR607, Indian Institute of Technology Bombay.
