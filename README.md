# Underwater Garbage Detection

An underwater waste detection and segmentation project built with **Ultralytics YOLOv8**. The notebook downloads an underwater trash dataset, creates a YOLO dataset configuration, trains YOLOv8 segmentation model variants, and runs inference on validation images and a sample video.

## Overview

Underwater pollution threatens marine ecosystems. This project explores computer vision as a way to identify and localize underwater objects—including different kinds of waste—within images and video.

The notebook focuses on **instance segmentation** using YOLOv8 segmentation checkpoints. It also includes image/video visualization helpers for reviewing model predictions.

## Features

- Downloads the training dataset and inference sample files.
- Builds a YAML configuration for training and validation data.
- Trains YOLOv8 segmentation model variants (`yolov8n-seg`, `yolov8s-seg`, and `yolov8m-seg`).
- Runs inference on validation images.
- Runs inference on a sample video.
- Visualizes saved prediction images and video frames.

## Classes

The notebook configures 16 classes:

| ID | Class |
|---:|---|
| 0 | `rov` |
| 1 | `plant` |
| 2 | `animal_fish` |
| 3 | `animal_starfish` |
| 4 | `animal_shells` |
| 5 | `animal_crab` |
| 6 | `animal_eel` |
| 7 | `animal_etc` |
| 8 | `trash_etc` |
| 9 | `trash_fabric` |
| 10 | `trash_fishing_gear` |
| 11 | `trash_metal` |
| 12 | `trash_paper` |
| 13 | `trash_plastic` |
| 14 | `trash_rubber` |
| 15 | `trash_wood` |

## Tech Stack

- **Python**
- **Ultralytics YOLOv8**
- **OpenCV** (`cv2`)
- **NumPy**
- **Matplotlib**
- **PyYAML**
- **Requests**
- **Jupyter Notebook**

## Project Structure

After running the notebook, the working directory is expected to contain files and folders similar to:

```text
.
├── Underwater_garbage_detection-2.ipynb
├── trashcan_inst_material.yaml
├── trash_inst_material/                 # Dataset extracted by the notebook
├── trash_segment_inference_data/         # Sample inference assets
└── runs/
    └── segment/
        ├── yolov8n-seg/
        ├── yolov8s-seg/
        ├── yolov8m-seg/
        ├── yolov8m_predict/
        └── yolov8m_predict_videos1/
```

Exact output folders may vary depending on the Ultralytics version and run configuration.

## Getting Started

### 1. Clone the repository

```bash
git clone <YOUR_REPOSITORY_URL>
cd <YOUR_REPOSITORY_DIRECTORY>
```

Replace the placeholders with your GitHub repository URL and directory name.

### 2. Install dependencies

Use Python 3.9+ as a starting point, with a compatible PyTorch/Ultralytics setup for your machine.

```bash
pip install ultralytics opencv-python matplotlib numpy requests pyyaml jupyter
```

### 3. Open the notebook

```bash
jupyter notebook
```

Open `Underwater_garbage_detection-2.ipynb` and run the cells in order. Google Colab can also be used if the required packages and runtime are available.

## Training

The notebook sets the number of epochs to **5**, image size to **640**, and batch size to **16**. It attempts to train these segmentation model variants:

- `yolov8n-seg.pt` — nano
- `yolov8s-seg.pt` — small
- `yolov8m-seg.pt` — medium

The notebook saves training outputs under `runs/segment/`. These settings are intended as an initial experiment; they are not evidence of a particular accuracy or production-ready performance.

> **Note:** The notebook's second training command uses `task=detect` while specifying segmentation model weights (`yolov8s-seg.pt`). If the intent is instance segmentation, check that command and use `task=segment` consistently. Also verify the task setting for the medium model command before running.

## Run Inference

The notebook runs prediction using the trained medium segmentation model:

```bash
yolo predict \
  model=runs/segment/yolov8m-seg/weights/best.pt \
  source=trash_inst_material/val/images/ \
  name=yolov8m_predict \
  exist_ok=True \
  save=True
```

For video inference:

```bash
yolo predict \
  model=runs/segment/yolov8m-seg/weights/best.pt \
  source=trash_segment_inference_data/manythings.mp4 \
  name=yolov8m_predict_videos1 \
  exist_ok=True \
  save=True
```

Prediction outputs are saved under the relevant `runs/segment/` directory and can be inspected using the notebook's visualization function.

## Dataset and Downloads

The notebook downloads dataset archives from Dropbox links embedded in the notebook. Make sure those links are still accessible and that you have permission to use the dataset. Review the dataset's license and attribution requirements before redistributing any dataset files or trained weights.

## Results

The notebook includes code to display prediction images and play through predicted video frames. Add screenshots or GIFs of actual model outputs here after running the notebook:

```markdown
![Example prediction](assets/example-prediction.png)
```

No performance metrics are reported in the supplied notebook, so precision, recall, mAP, inference speed, and other results should be added only after evaluation.

## Limitations

- Results depend on the dataset, annotation quality, training configuration, and hardware.
- The notebook uses only 5 epochs, which may be insufficient for a reliable model.
- No quantitative evaluation or benchmark results are included in the notebook.
- Underwater visibility, lighting, turbidity, and object occlusion may affect predictions.
- Predictions should be validated before using the system in real-world marine cleanup workflows.

## Future Improvements

- Evaluate performance using precision, recall, mAP50, and mAP50–95.
- Tune epochs, image size, batch size, and augmentation settings.
- Compare model variants on the same held-out validation set.
- Add a webcam or live underwater-camera inference pipeline.
- Test robustness across different water conditions and camera setups.
- Document dataset provenance, licensing, and reproducible environment versions.

## Contributing

Contributions, bug reports, and suggestions are welcome. Open an issue or submit a pull request with a clear description of the proposed change.

## License

No project license is specified in the supplied notebook. Add a `LICENSE` file before inviting reuse, and ensure the dataset and pretrained model licenses are compatible with your intended use.

---

**Disclaimer:** This repository describes an experimental computer-vision notebook. It does not establish that the model is accurate enough for autonomous cleanup or environmental monitoring without further testing.
