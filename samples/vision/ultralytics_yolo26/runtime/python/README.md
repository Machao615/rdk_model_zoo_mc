English | [简体中文](./README_cn.md)

# YOLO26 Python Runtime

This sample demonstrates how to run YOLO26 task models on RDK S100/S100P with `hbm_runtime`.

## Environment Dependencies

This sample has no special extra dependencies. Make sure the RDK S100/S100P Python environment is ready.

```bash
pip install numpy opencv-python hbm-runtime scipy
```

## Directory Structure

```text
.
├── main.py                # Inference entry script
├── yolo26_det.py          # Detection wrapper
├── yolo26_seg.py          # Segmentation wrapper
├── yolo26_pose.py         # Pose wrapper
├── yolo26_obb.py          # OBB wrapper
├── yolo26_cls.py          # Classification wrapper
├── run.sh                 # One-click execution script
└── README.md              # Usage instructions
```

## Parameter Description

| Parameter | Description | Default Value |
|---|---|---|
| `--task` | Task type: `detect`, `seg`, `pose`, `cls`, `obb` | (required) |
| `--model-path` | Path to the `.hbm` model file | `../../model/{nash-e|nash-m}/yolo26n_detect_{suffix}_640x640_nv12.hbm` |
| `--test-img` | Path to the test input image | `../../test_data/bus.jpg` |
| `--label-file` | Path to the label file, empty means using task default | `""` |
| `--img-save-path` | Path to save the result image | `result.jpg` |
| `--priority` | Model priority | `0` |
| `--bpu-cores` | BPU core indexes used for inference | `0` |
| `--score-thres` | Score threshold | `0.25` |
| `--nms-thres` | NMS threshold | `0.45` |
| `--topk` | Top-K results for classification | `5` |
| `--kpt-conf-thres` | Keypoint confidence threshold for pose | `0.50` |
| `--angle-sign` | Angle sign for OBB decoding | `1.0` |
| `--angle-offset` | Angle offset for OBB decoding | `0.0` |

> **Note**: The default `--model-path` is determined automatically based on the detected SoC. S100 uses the `nashe` suffix; S100P uses the `nashm` suffix.

## Quick Run

- **One-click Execution Script**
  ```bash
  bash run.sh detect
  ```

- **Manual Execution**
  - Use default parameters
    ```bash
    python3 main.py --task detect
    ```
  - Run detection with explicit parameters
    ```bash
    python3 main.py \
        --task detect \
        --model-path ../../model/nash-e/yolo26n_detect_nashe_640x640_nv12.hbm \
        --test-img ../../test_data/bus.jpg
    ```
  - Run detection explicitly on RDK S100P
    ```bash
    python3 main.py \
        --task detect \
        --model-path ../../model/nash-m/yolo26n_detect_nashm_640x640_nv12.hbm \
        --test-img ../../test_data/bus.jpg
    ```

## Task Examples

### Segmentation

```bash
python3 main.py \
    --task seg \
    --model-path ../../model/nash-e/yolo26n_seg_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### Pose

```bash
python3 main.py \
    --task pose \
    --model-path ../../model/nash-e/yolo26n_pose_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### OBB

```bash
python3 main.py \
    --task obb \
    --model-path ../../model/nash-e/yolo26n_obb_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### Classification

```bash
python3 main.py \
    --task cls \
    --model-path ../../model/nash-e/yolo26n_cls_nashe_224x224_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

## Interface Description

- **`YOLO26*Config`**: Encapsulates model path and runtime parameters for each task.
- **`YOLO26*`**: Provides the complete inference pipeline, including `pre_process`, `forward`, `post_process`, and `predict`.

Each wrapper class follows the standard interface:

```python
class Wrapper:
    def __init__(self, config)
    def set_scheduling_params(self, priority=None, bpu_cores=None)
    def pre_process(self, img, image_format="BGR")
    def forward(self, input_tensor)
    def post_process(self, outputs, ...)
    def predict(self, img, ...)
    def __call__(self, img, ...)
```

### Return Formats

| Task   | Return Type                                                          |
|--------|----------------------------------------------------------------------|
| detect | `(boxes, scores, cls_ids)` — numpy arrays `(N,4)`, `(N,)`, `(N,)`   |
| seg    | `(boxes, scores, cls_ids, masks)` — arrays + list of cropped binary masks |
| pose   | `(boxes, scores, cls_ids, keypoints)` — arrays + keypoints `(N,17,3)` |
| cls    | `[(class_id, probability), ...]` — list of tuples                    |
| obb    | `[{'rrect': (cx,cy,w,h,angle), 'score': float, 'id': int}, ...]`    |

Refer to the shared utilities under `utils/py_utils/` for common pre-processing, post-processing, and visualization helpers.
