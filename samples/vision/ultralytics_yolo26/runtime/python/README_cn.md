[English](./README.md) | 简体中文

# YOLO26 Python 运行时

本示例演示如何在 RDK S100/S100P 上使用 `hbm_runtime` 运行 YOLO26 任务模型。

## 环境依赖

本示例无额外依赖。请确保 RDK S100/S100P Python 环境已就绪。

```bash
pip install numpy opencv-python hbm-runtime scipy
```

## 目录结构

```text
.
├── main.py                # 推理入口脚本
├── yolo26_det.py          # 目标检测封装
├── yolo26_seg.py          # 实例分割封装
├── yolo26_pose.py         # 姿态估计封装
├── yolo26_obb.py          # 旋转框检测封装
├── yolo26_cls.py          # 图像分类封装
├── run.sh                 # 一键运行脚本
└── README.md              # 使用说明
```

## 参数说明

| 参数 | 说明 | 默认值 |
|---|---|---|
| `--task` | 任务类型：`detect`、`seg`、`pose`、`cls`、`obb` | （必填） |
| `--model-path` | `.hbm` 模型文件路径 | `../../model/{nash-e|nash-m}/yolo26n_detect_{suffix}_640x640_nv12.hbm` |
| `--test-img` | 测试图片路径 | `../../test_data/bus.jpg` |
| `--label-file` | 标签文件路径，为空时使用任务默认标签 | `""` |
| `--img-save-path` | 结果图片保存路径 | `result.jpg` |
| `--priority` | 模型优先级 | `0` |
| `--bpu-cores` | BPU 核心索引 | `0` |
| `--score-thres` | 置信度阈值 | `0.25` |
| `--nms-thres` | NMS 阈值 | `0.45` |
| `--topk` | 分类 Top-K 结果数 | `5` |
| `--kpt-conf-thres` | 姿态关键点置信度阈值 | `0.50` |
| `--angle-sign` | 旋转框角度解码符号 | `1.0` |
| `--angle-offset` | 旋转框角度解码偏移量 | `0.0` |

> **注意**：`--model-path` 默认值根据检测到的 SoC 自动确定。S100 使用 `nashe` 后缀，S100P 使用 `nashm` 后缀。

## 快速运行

- **一键运行脚本**
  ```bash
  bash run.sh detect
  ```

- **手动运行**
  - 使用默认参数
    ```bash
    python3 main.py --task detect
    ```
  - 指定参数运行检测
    ```bash
    python3 main.py \
        --task detect \
        --model-path ../../model/nash-e/yolo26n_detect_nashe_640x640_nv12.hbm \
        --test-img ../../test_data/bus.jpg
    ```
  - 在 RDK S100P 上显式运行检测
    ```bash
    python3 main.py \
        --task detect \
        --model-path ../../model/nash-m/yolo26n_detect_nashm_640x640_nv12.hbm \
        --test-img ../../test_data/bus.jpg
    ```

## 任务示例

### 实例分割

```bash
python3 main.py \
    --task seg \
    --model-path ../../model/nash-e/yolo26n_seg_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### 姿态估计

```bash
python3 main.py \
    --task pose \
    --model-path ../../model/nash-e/yolo26n_pose_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### 旋转框检测

```bash
python3 main.py \
    --task obb \
    --model-path ../../model/nash-e/yolo26n_obb_nashe_640x640_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

### 图像分类

```bash
python3 main.py \
    --task cls \
    --model-path ../../model/nash-e/yolo26n_cls_nashe_224x224_nv12.hbm \
    --test-img ../../test_data/bus.jpg
```

## 接口说明

- **`YOLO26*Config`**：封装各任务的模型路径和运行参数。
- **`YOLO26*`**：提供完整推理流水线，包括 `pre_process`、`forward`、`post_process` 和 `predict`。

每个封装类遵循标准接口：

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

### 返回格式

| 任务     | 返回类型                                                            |
|---------|--------------------------------------------------------------------|
| detect  | `(boxes, scores, cls_ids)` — numpy 数组 `(N,4)`, `(N,)`, `(N,)`   |
| seg     | `(boxes, scores, cls_ids, masks)` — 数组 + 裁剪后的二值掩码列表    |
| pose    | `(boxes, scores, cls_ids, keypoints)` — 数组 + 关键点 `(N,17,3)`  |
| cls     | `[(class_id, probability), ...]` — 元组列表                        |
| obb     | `[{'rrect': (cx,cy,w,h,angle), 'score': float, 'id': int}, ...]`  |

通用预处理、后处理和可视化工具请参考 `utils/py_utils/` 目录。
