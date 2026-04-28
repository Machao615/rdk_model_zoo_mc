![](imgs/RDK_Video_Solution.jpg)

English | [简体中文](./README_cn.md)

# RDK Video Solution

In real-time video stream inference, the RDK Video Solution optimizes the following aspects:

- **Program initialization and model loading time**: In real-time video inference tasks, these costs are incurred only once — there is no need to repeatedly load the program and model.

- **Pre-processing time**: Demo programs use OpenCV to prepare NV12 input data for quick results and maximum compatibility. This involves redundant color space conversions that increase latency. In production, more efficient video pipelines use hardware accelerators (JPU, VPU, Codec) to prepare input data, VPS/VSE for pre-processing, and BPU for normalization — all faster than OpenCV and CPU-free.

- **Inference time**: Production systems use multi-threaded and multi-process asynchronous processing instead of a simple while loop. Only asynchronous multi-threaded processing allows CPU, BPU, JPU, VPU, Codec, VSE, and other hardware to compute in parallel. Refer to the Model Zoo README for compiling `.bin` models for TROS usage — as long as the camera frame rate is sufficient, BPU throughput can be fully utilized.

- **Rendering**: In production, rendering is a low-priority requirement and typically not run on the board. If needed, hardware OSD overlay is used instead of OpenCV.

## Serial vs Parallel Programming

### Serial Programming
![](imgs/Serial_Programming.png)

### Parallel Programming
![](imgs/Parallel_Programming.png)

## DataFlow References

### IPC Camera (TROS DataFlow)
![](imgs/TROS_IPC_Camera.png)

### USB Camera (TROS DataFlow)
![](imgs/TROS_USB_Camera.png)

### MIPI Camera (TROS DataFlow)
![](imgs/TROS_MIPI_Camera.png)
