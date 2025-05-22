---
title: 이미지 생성형 AI - Stable Diffusion Forge
date: 2025-05-06
categories: [Artificial Intelligence, Basic]
tags: [Artificial Intelligence, Basic]
---

## Stable Diffusion Forge
Stable Diffusion 용 사용자 인터페이스

## Stable Diffusion Checkpoint
모델의 학습 과정에서 특정 시점의 저장한 상태(가중치, 하이퍼파라미터 등)

- __Stable Diffusion v1.5__  
Stable Diffusion v1.2 모델을 업그레이드한 모델  
기본 해상도 : 512x512  

- __Stable Diffusion XL__  
Stable Diffusion v1.5 모델을 업그레이드한 모델  
기본 해상도 : 1024x1024  

- __FLUX__  
Black Forest Labs 에서 개발한 모델  
텍스트를 정확하게 렌더링  
부정 프롬프트 지원X  


## ControlNet
이미지를 세밀하게 제어할 수 있도록 하는 확장 모듈  
이미지 생성 모델에 조건부 제어를 추가하는 오픈소스 신경망  


## LoRA (Low Rank Adaption)
기존 학습 모델(체크포인트)에 원하는 스타일을 추가로 학습시킨 모델  
파라미터를 줄여 전체 모델을 경량화 하는 기법으로 매개변수 가중치 중 일부만 미세 조정  


## Azure 로 Stable Diffusion Forge WebUI 사용
1. Azure 리소스(작업 영역, 가상 머신) 생성
2. 가상 머신에서 WebUI 설치 및 실행
- 가상 머신 SSH 접속
  ```bash
  ssh -i SSH 키 파일 사용자명@가상머신 ip -p 50000
  ```
- Stable Diffusion Forge Git 설치
  ```bash
  git clone https://github.com/lllyasviel/stable-diffusion-webui-forge.git
  ```  
- 모델 설치 (/stable-diffusion-webui-forge/models/Stable-diffusion 디렉토리)
  ```bash
  # SD 1.5 모델
  curl -H "Authorization: Bearer 허깅페이스 토큰" https://huggingface.co/runwayml/stable-diffusion-v1-5/resolve/main/v1-5-pruned-emaonly.ckpt --location --output v1-5-pruned-emaonly.ckpt
  # flux 모델
  curl -H "Authorization: Bearer 허깅페이스 토큰" https://huggingface.co/lllyasviel/flux1_dev/resolve/main/flux1-dev-fp8.safetensors --location --output flux1-dev-fp8.safetensors
  ```
- WebUI 실행 (/stable-diffusion-webui-forge 디렉토리)
  ```bash
  ./webui.sh --share --enable-insecure-extension-access --gradio-auth 사용자명:비밀번호
  ```  


##  참고
[[논문리뷰] Adding Conditional Control to Text-to-Image Diffusion Models (ControlNet)](https://kimjy99.github.io/%EB%85%BC%EB%AC%B8%EB%A6%AC%EB%B7%B0/controlnet/)  
[LoRA(Low-Rank Adaptation) AI모델의 미세조정 기술에 대하여](https://news.aikoreacommunity.com/lora-low-rank-adaptation-ai-model-fine-tuning/)  
[LoRA 기법에 대해서](https://ai0-0jiyun.tistory.com/6)  