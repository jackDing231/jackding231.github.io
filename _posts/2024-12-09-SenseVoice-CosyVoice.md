---
layout:     post
title:      "语音识别模型SenseVoice、语音生成模型CosyVoice使用教程"
subtitle:   "SenseVoice/CosyVoice tutorial"
date:       2024-12-09
author:     "Jack Ding"
header-style: text
tags:
    - LLM
    - tutorial
---

Slide: https://docs.google.com/presentation/d/1gliFsKkR40Ofr9kPwWYWOnLdwc4U4hjnx6gGWAYCUBs/edit?usp=sharing
vLLM: 
pip install vllm
https://github.com/vllm-project/vllm 
Cosyvoice: 
Follow README.md
https://github.com/FunAudioLLM/CosyVoice 
https://github.com/BiboyQG/bob-cosyvoice 
Sensevoice:
Follow README.md
https://github.com/FunAudioLLM/SenseVoice

7月初，阿里云通义宣布正式开源两款前沿的语音基座模型**SenseVoice**与**CosyVoice**，这两款模型分别针对语音识别与语音生成领域，以其卓越的性能和广泛的应用潜力见长，其中**SenseVoice**在语音识别方面的表现尤为突出，其识别效果已超越行业标杆OpenAI Whisper。
**SenseVoice**作为一款专注于高精度多语言语音识别的模型，其独特之处在于其广泛的语言覆盖、强大的情感辨识能力以及高效的推理性能。该模型基于超过40万小时的多样化语音数据训练而成，能够支持超过50种语言的识别，展现出卓越的跨语言识别能力。与市场上其他主流模型相比，**SenseVoice**在识别精度上实现了显著提升，特别是在复杂场景下的表现尤为出色。
除了基本的语音识别功能外，**SenseVoice**还具备丰富的情感识别与音频事件检测能力。它能够精准捕捉语音中的情感波动，如喜悦、悲伤、愤怒等，并在测试数据上展现出与当前最佳情感识别模型相媲美甚至更优的性能。同时，**SenseVoice**还内置了声音事件检测功能，能够识别出音乐、掌声、笑声、哭声、咳嗽、喷嚏等多种常见人机交互事件，为开发者提供了更加全面的语音分析工具。

在性能优化方面，**SenseVoice**采用了先进的非自回归端到端框架，实现了极低的推理延迟。据官方数据显示，**SenseVoice-Small**模型在处理10秒长的音频时，仅需70毫秒即可完成推理，这一速度比**Whisper-Large**模型快了15倍以上，为实时语音识别应用提供了有力支持。
此外，**SenseVoice**还提供了便捷的微调脚本与策略，帮助用户根据特定业务场景进行模型定制与优化。同时，其完整的服务部署链路支持多并发请求处理，并兼容多种客户端编程语言（如Python、C++、HTML、Java、C#等），极大地方便了开发者的集成与应用。

与此同时，阿里云通义还推出了另一款语音生成模型——**CosyVoice**。该模型同样具备多语言支持、音色与情感控制等先进功能，并在多语言语音生成、零样本语音生成、跨语言语音克隆以及指令跟随等方面展现出卓越的性能。
**CosyVoice**的推出，不仅为开发者提供了更加灵活多样的语音生成解决方案，还进一步推动了语音技术的边界拓展。无论是创建个性化语音助手、还是构建多语言交互平台，**CosyVoice**都能为开发者提供强大的技术支持与创意灵感。



ChatTTS(同济子豪兄推荐)

ChatTTS源码：https://github.com/2noise/ChatTTS
ChatTTS模型：https://huggingface.co/2Noise/ChatTTS
ChatTTS在线网页Demo：https://huggingface.co/spaces/lenML/ChatTTS-Forge
ChatTTS二次开发项目合集：https://github.com/libukai/Awesome-ChatTTS

子豪兄代码教程：https://github.com/TommyZihao/ChatTTS_Tutorials



百度智能云免费测试资源领取：

[语音技术 - 百度智能云控制台 (baidu.com)](https://console.bce.baidu.com/ai/#/ai/speech/overview/resource/getFree)

调用量：5万次 ；QPS/并发：5并发

每个账号只能领取一次

有效期：180天