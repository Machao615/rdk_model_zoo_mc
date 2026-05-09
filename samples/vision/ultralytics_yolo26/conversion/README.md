English | [简体中文](./README_cn.md)

# Model Conversion

This directory contains documentation and tools for converting YOLO26 models from ONNX to HBM format for RDK S100/S100P.

Conversion details are pending and will be added in a future update.

## Model Compilation Environment

To convert models, prepare the RDK S100/S100P OpenExplore toolchain on an x86 Linux machine. Refer to the D-Robotics model conversion documentation for detailed instructions.

## Notes

- The RDK S100/S100P inference model format is `.hbm`.
- All models use NV12 input (Y + UV as two separate tensors).
- The model suffix differs by platform: `nashe` for S100, `nashm` for S100P.
