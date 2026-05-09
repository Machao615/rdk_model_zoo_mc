[English](./README.md) | 简体中文

# 模型转换

Model Zoo 已提供 MobileNetV1 的 S100 HBM 模型。只需要运行推理的用户可以
直接在 `../model/` 目录下载模型。

## 已发布产物

| 文件 | 输入 | 运行时 |
| --- | --- | --- |
| `mobilenetv1_224x224_nv12.hbm` | 224x224 NV12 (Y + UV) | `hbm_runtime` |

## 重新生成说明

旧版 S100 sample 使用 MobileNet-Caffe 模型作为源模型，并通过 RDK S100 工具链
完成转换。如需重新生成模型，请使用 S100 OE 包中的模型转换环境，并保持当前
运行时接口不变：两个 NV12 输入，分别为 Y plane 和 UV plane。
