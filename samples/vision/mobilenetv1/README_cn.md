[English](./README.md) | 简体中文

# MobileNetV1

MobileNetV1 是基于深度可分离卷积的轻量级 ImageNet 分类模型。本 sample
提供标准化 Python 推理入口，使用 `hbm_runtime` 运行 HBM 模型，模型输入为
NV12 的 Y/UV 双输入。

## 目录结构

```text
.
|-- conversion/             # 模型转换说明
|-- evaluator/              # 精度与结果验证说明
|-- model/                  # HBM 下载脚本与模型说明
|-- runtime/
|   `-- python/             # Python 推理入口与模型封装
|-- test_data/              # 测试图片与 ImageNet 标签
|-- README.md
`-- README_cn.md
```

## 快速开始

```bash
cd runtime/python
bash run.sh
```

脚本会将已发布的 S100 HBM 模型下载到 `model/s100/`，并使用
`test_data/zebra_cls.jpg` 执行分类推理。

直接运行入口：

```bash
cd runtime/python
python3 main.py \
  --model-path ../../model/s100/mobilenetv1_224x224_nv12.hbm \
  --test-img ../../test_data/zebra_cls.jpg \
  --label-file ../../test_data/imagenet_classes.names
```

## 模型

| 模型 | 任务 | 输入 | 类别数 | 已发布 HBM |
| --- | --- | --- | --- | --- |
| MobileNetV1 | 图像分类 | 224x224 NV12 (Y + UV) | ImageNet 1000 | S100 |

当前公开下载源提供 S100 HBM 模型。只有在存在可运行的 S100P 兼容模型并通过
板端结果正确性验证后，才标记 S100P 验证完成。

## 结果

使用 `zebra_cls.jpg` 时，正确结果应在 Top-5 中包含 `zebra`，且分数分布为
合理的非零值。

## 更多信息

- [模型下载](./model/README_cn.md)
- [Python 推理](./runtime/python/README_cn.md)
- [模型转换](./conversion/README_cn.md)
- [模型评估](./evaluator/README_cn.md)
