[English](./README.md) | 简体中文

# 模型转换

本目录包含将 Ultralytics YOLO ONNX 模型转换为 HBM 格式（适用于 RDK S100/S100P）的转换配置和文档。

转换详情待定，将在后续更新中补充。

## 模型编译环境

如需转换模型，请在 x86 Linux 机器上准备 RDK S100/S100P OpenExplore 工具链。详细操作说明请参考 D-Robotics 模型转换文档。

## 说明

- RDK S100/S100P 推理模型格式为 `.hbm`。
- 所有模型使用 NV12 输入（Y + UV 两个独立输入张量）。
- 模型后缀因平台不同而异：S100 为 `nashe`，S100P 为 `nashm`。
