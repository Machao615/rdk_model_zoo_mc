![](imgs/RDK_LLM_Solution.jpg)

English | [简体中文](./README_cn.md)

# RDK LLM Solution

## Overview

Based on current research, SBCs with NPUs run vision models like YOLO at 10-100x the speed of CPU-only execution. However, for language models (LLMs), NPU speed is at most 1.2-1.6x that of CPU, and NPU solutions limit context length to very short lengths (e.g., 256 tokens). Running LLMs on NPU also requires significantly more development effort. Therefore, using CPU for language tasks while dedicating NPU to vision tasks is a practical and reasonable approach for the current stage.

This solution references community contributor @潜沉10's work, using the llama.cpp framework to run large language models on RDK X5. Tests were conducted with `thread_num=4` and `thread_num=8`, covering 63 models from 8 vendors with parameter sizes ranging from 0.5B to 14B. Among them, 9 models exceed 10 tokens/s, 14 exceed 5 tokens/s, and 52 exceed 1 token/s.

## Benchmark Results

See the full benchmark document: [Feishu Doc](https://horizonrobotics.feishu.cn/docx/LQU9dYyjcoXJ9hxJdUYc2l4InEf)

## Usage

### References

- Running LLMs on RDK with llama.cpp: [D-Robotics Forum](https://developer.d-robotics.cc/forumDetail/256524800871478519)
- llama.cpp build guide: [GitHub](https://github.com/ggerganov/llama.cpp/blob/master/docs/build.md)
- GLM GGUF models: [HuggingFace](https://huggingface.co/THUDM/glm-edge-1.5b-chat-gguf/blob/main/README_zh.md)

### Download and Build llama.cpp

```bash
git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp
cmake -B build
cmake --build build --config Release
```

### Add to PATH

```bash
PATH=/media/rootfs/99_projects/test_llama.cpp/llama.cpp/build/bin:$PATH
```

### Run a Model

Use a fixed prompt to generate 128 tokens, then press Ctrl+C to view performance results.

```bash
llama-cli \
-m <path to your gguf model> \
-n 512 -c 2048 \
-p "You are a helpful assistant" -co -cnv \
--threads 8
```

For RWKV models (without KV Cache), add `--no-context-shift`:

```bash
llama-cli \
-m rwkv-6-finch-3b-Q8_0.gguf \
-n 512 -c 2048 \
-p "You are a helpful assistant" -co -cnv \
--threads 8 --no-context-shift
```
