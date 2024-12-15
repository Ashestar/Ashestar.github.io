---
title: ComfyUI和Flux
date: 2024-12-15 09:00:00
tags:
  - ComfyUI
  - Flux
  - AI
categories:
  - ComfyUI
  - Flux
  - AI
---

使用 [ComfyUI](https://github.com/comfyanonymous/ComfyUI) 部署 [Flux Dev 模型](https://huggingface.co/black-forest-labs/FLUX.1-dev/tree/main)。

<!--more-->

# ComfyUI

首先确保本机已经安装好了基于arm架构的Python3.11。

根据参考文章，之所以使用Python3.11，是因为这个版本性能有一定的优化，又不会像最新的3.13由于版本过新，引发依赖装不上的问题。

1. 安装python3.11，推荐使用Anaconda管理环境

```bash
# 下载指定版本
conda create --name python311 python=3.11
# 启用该环境
conda activate python311 
```

2. 部署ComfyUI

```bash
# 拉取ComfyUI
git clone https://github.com/comfyanonymous/ComfyUI.git

# 安装 MPS 版本的 torch
pip install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu

# 安装依赖
cd ComfyUI
pip install -r requirements.txt

# 安装 ComfyUI 的 Manager 项目，用来安装各种节点
cd custom_nodes  
git clone https://github.com/ltdrdata/ComfyUI-Manager.git

# 启动ComfyUI服务，强制使用fp16精度用来提升性能
python main.py --force-fp16
```

# Flux-dev-GGUF模型下载

下载需要的flux-dev模型，由于官方的模型体积太大(23G)，这里我们下载GGUF的量化版本

1. 下载 [Flux GGUF dev 模型](https://huggingface.co/city96/FLUX.1-dev-gguf) 并将模型文件放置在 comfyui/models/unet 目录下
2. 下载 [t5-v1_1-xxl-encoder-gguf](https://huggingface.co/city96/t5-v1_1-xxl-encoder-gguf) 并将模型文件放置在 comfyui/models/clip 目录下
3. 下载 [clip_l.safetensors](https://huggingface.co/comfyanonymous/flux_text_encoders/blob/main/clip_l.safetensors) 并将模型文件放置在 comfyui/models/clip 目录下
4. 下载 [ae.safetensors](https://huggingface.co/black-forest-labs/FLUX.1-schnell/blob/main/ae.safetensors) 并将模型文件放置在 comfyui/models/vae 目录下
5. 下载 [基于GGUF的工作流](https://promptingpixels.com/flux-gguf/) 并导入ComfyUI中
6. 安装 [ComfyUI-GGUF](https://github.com/city96/ComfyUI-GGUF) 插件，可以参考 [ComfyUI 插件安装教程](https://comfyui-wiki.com/zh/install/install-custom-nodes) 安装缺失的节点

# Have Some Fun

我测试过的模型版本是参考文章里面的 flux1-dev-Q4_1.gguf 和 t5-v1_1-xxl-encoder-Q5_K_M.gguf 。

由于我的 macbook 芯片是 m1 pro ，感觉是勉强能用，出图速度不快，只能说是玩票了，还是跑本地语言大模型更合适。

# 参考文章

- [m4 mac mini本地部署ComfyUI,测试Flux-dev-GGUF的workflow模型10步出图,测试AI绘图性能,基于MPS(fp16),优点是能耗小和静音](https://www.cnblogs.com/v3ucn/p/18593990)
- [Flux.1 ComfyUI 对应模型安装及教程指南](https://comfyui-wiki.com/zh/tutorial/advanced/flux1-comfyui-guide-workflow-and-examples)