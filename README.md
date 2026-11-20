# NPark Internship: Fishing Detection

This repository contains object-detection experiments for fishing-related imagery and video. It includes two implementations:

- **YOLOv5 (PyTorch):** training, validation, and image/video detection scripts. Its dataset configuration defines one class, `fishing`.
- **YOLOv3 (Keras):** an older Keras implementation with separate training and video-detection scripts.
- **Project report:** `NPark_Internship/final_report_single_column.pdf`.

## Repository layout

```text
NPark_Internship/
|-- final_report_single_column.pdf
|-- keras-yolo3/       # Keras YOLOv3 implementation
`-- yolov5/            # PyTorch YOLOv5 implementation
```

## YOLOv5

The YOLOv5 code and its dependency list are in `NPark_Internship/yolov5`. The scripts use relative paths, so run them from that directory.

### Install dependencies

Use a Python environment compatible with the versions in `requirements.txt`. This is an older codebase; in particular, the requirements pin `torchvision==0.8.1`, so newer Python and PyTorch environments may need dependency adjustments.

```bash
cd NPark_Internship/yolov5
python -m pip install -r requirements.txt
```

### Prepare a dataset

The supplied `data.yaml` expects training and validation images at `../train/images` and `../valid/images`, relative to the YOLOv5 working directory. Put matching YOLO label files under `../train/labels` and `../valid/labels`, or edit the paths in `data.yaml` to match your dataset.

Each label file should have the same base name as its image and use YOLO's normalized format:

```text
class_id x_center y_center width height
```

The configured class is `fishing` with class ID `0`.

### Train

From `NPark_Internship/yolov5`, run:

```bash
python train.py --img-size 416 --batch-size 16 --epochs 100 --data data.yaml --weights yolov5s.pt
```

The script attempts to download `yolov5s.pt` if that starting checkpoint is not available locally. Training outputs are written under `runs/train`; the best checkpoint is normally saved at `runs/train/exp/weights/best.pt`.

### Detect objects

Provide a model checkpoint and an image, video, folder, webcam index, or supported stream as the source:

```bash
python detect.py --weights path/to/checkpoint.pt --source path/to/input.mp4
```

Detection results are written under `runs/detect`. For webcam input, use `--source 0`. Run `python detect.py --help` for the available options.

## YOLOv3 (Keras)

The `NPark_Internship/keras-yolo3` directory contains the older Keras implementation. Its `yolo_video.py` script accepts a video input and optional output path, model, anchors, and class list. Run it from that directory, for example:

```bash
cd NPark_Internship/keras-yolo3
python yolo_video.py --input path/to/input.mp4 --output detections.mp4 --model path/to/model.h5 --anchors model_data/yolo_anchors.txt --classes path/to/classes.txt
```

This code expects a compatible TensorFlow/Keras environment. There is no Keras requirements file in the repository. The training script and default detector configuration contain machine-specific paths under `C:/fyp`; edit those paths and provide the matching annotations, class list, anchors, and model weights before using them.

## Data and model files

The Git checkout contains source code and dataset-export notes, but does not track the datasets, media examples, or trained model checkpoints. Ignore rules exclude common image/video files and weight formats such as `.pt`, `.h5`, and `.weights`. Some ignored files may exist in a local working copy; they will not be present in a fresh clone. Supply the needed assets separately before training or inference.


