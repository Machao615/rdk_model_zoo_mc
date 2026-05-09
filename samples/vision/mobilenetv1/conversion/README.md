English | [简体中文](./README_cn.md)

# Model Conversion

The Model Zoo provides a pre-compiled S100 HBM model for MobileNetV1. Users who
only need runtime inference can download the model from `../model/`.

## Published Artifact

| File | Input | Runtime |
| --- | --- | --- |
| `mobilenetv1_224x224_nv12.hbm` | 224x224 NV12 (Y + UV) | `hbm_runtime` |

## Regeneration Notes

The legacy S100 sample used the MobileNet-Caffe model as the source model and
converted it with the RDK S100 toolchain. If the model needs to be regenerated,
use the S100 OE package model conversion environment and keep the published
runtime interface unchanged: two NV12 inputs, Y plane and UV plane.
