English | [简体中文](./README_cn.md)

# MobileNetV1

MobileNetV1 is a lightweight ImageNet classification model based on depthwise
separable convolutions. This sample provides a standardized Python runtime for
RDK S-series devices with `hbm_runtime` and NV12 model input.

## Directory Structure

```text
.
|-- conversion/             # Model conversion notes
|-- evaluator/              # Accuracy and result validation notes
|-- model/                  # HBM download script and model README
|-- runtime/
|   `-- python/             # Python runtime entry and wrapper
|-- test_data/              # Test image and ImageNet labels
|-- README.md
`-- README_cn.md
```

## Quick Start

```bash
cd runtime/python
bash run.sh
```

The script downloads the published S100 HBM model to `model/s100/` and runs
classification on `test_data/zebra_cls.jpg`.

For direct execution:

```bash
cd runtime/python
python3 main.py \
  --model-path ../../model/s100/mobilenetv1_224x224_nv12.hbm \
  --test-img ../../test_data/zebra_cls.jpg \
  --label-file ../../test_data/imagenet_classes.names
```

## Model

| Model | Task | Input | Classes | Published HBM |
| --- | --- | --- | --- | --- |
| MobileNetV1 | Image classification | 224x224 NV12 (Y + UV) | ImageNet 1000 | S100 |

The public download currently provides the S100 HBM model. S100P validation is
not marked as complete unless a runnable S100P-compatible model is available and
passes board-side result checks.

## Result

Using `zebra_cls.jpg`, a correct run should include `zebra` in the Top-5 results,
with a reasonable non-zero confidence distribution.

## More Information

- [Model download](./model/README.md)
- [Python runtime](./runtime/python/README.md)
- [Conversion](./conversion/README.md)
- [Evaluation](./evaluator/README.md)
