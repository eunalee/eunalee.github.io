---
title: 이미지 생성형 AI - Stable Diffusion
date: 2025-05-06
categories: [Artificial Intelligence, Basic]
tags: [Artificial Intelligence, Basic]
---

## Stable Diffusion
텍스트 및 이미지 프롬프트에서 고품질의 이미지를 생성하는 생성형 AI 모델  
오픈 소스로 공개하여 개인PC에 설치해 실행할 수 O  


## Diffusion Model
<span style="color:#337ea9">__노이즈 추가/제거 작업__</span>을 통해 데이터 분포를 학습하는 모델  
학습한 것과 비슷한 새로운 데이터를 생성  
신경망 모델에게 추가된 노이즈를 예측하도록 학습시킴!  
스테이블 디퓨전은 노이즈 예측기(noise predictor)로 U-Net 모델을 사용  

- __Forward Diffusion__
1. 학습용 이미지 선택  
2. 무작위 노이즈 이미지 생성  
3. 학습용 이미지에 정해진 단계만큼 무작위 노이즈 이미지를 추가하여 손상시킴  
4. 노이즈 예측기에게 노이즈 추가량을 학습시킴 (가중치를 조정하고 정답을 보여주는 방식으로 작업 수행)  
⇒ 이미지에 추가된 노이즈를 예측할 수 있는 노이즈 예측기 확보 (복잡한 분포 → 단순한 분포)

- __Reserve Diffusion__
1. 완전한 노이즈 이미지 생성  
2. 노이즈 예측기로 생성된 이미지에 노이즈가 얼마나 포함되었나 예측  
3. 노이즈 예측기가 반환한 예측 노이즈를 원래 노이즈 이미지에서 제거하여 새로운 이미지 생성  
4. 새로 생성된 이미지에서 다시 노이즈 예측기를 통해 예측과 이미지 생성을 반복  
⇒ 새로운 이미지 생성/기존 이미지 변환 (단순한 분포 → 복잡한 분포)  


## Latent Diffusion Model
<span style="color:#337ea9">__잠재 공간__</span>에서 Diffusion 과정이 동작하는 모델  
이미지를 잠재 공간으로 압축한 뒤 연산을 수행하여 처리 속도가 빠름 (잠재 공간 x 48 = 이미지 공간)  

__1. 텍스트 인코딩__  

1. CLIP (텍스트 인코더) 을 이용해 사용자 텍스트 프롬프트를 토큰(컴퓨터가 읽을 수 있는 숫자)으로 변환  
2. 토큰으로 변환된 값을 벡터화(다차원 숫자 구조로 변환)  
3. 임베딩된 벡터는 트랜스포머에서 처리된 후, U-Net (노이즈 예측기) 에 전달  

---

__2. 이미지 생성 과정__  

1. 임의의 원본 이미지가 VAE 인코더(이미지를 잠재 공간으로 압축)로 잠재 공간 상의 벡터로 변환됨  
2. 잠재 공간에서 Forward Diffusion 과정을 거쳐 노이즈로 변환  
3. 잠재 공간에서 Reserve Diffusion 과정을 거쳐 노이즈 제거  
4. 반복되는 작업(이 때, 전달된 임베딩 벡터 사용)을 통해 사용자가 원하는 이미지 정보를 가진 벡터 출력  

---

__3. 이미지 디코딩__  

1. 출력된 잠재 공간 상의 이미지 벡터를 VAE 디코더(잠재 공간의 이미지를 복원)를 통해 픽셀 이미지로 변환  

---


##  참고
[그림 그려주는 AI : Stable Diffusion AI의 원리 (Latent Diffusion Model)](https://tech-savvy30.tistory.com/9)  
[Stable Diffusion에 대한 기본적인 이론](https://www.internetmap.kr/entry/Basic-Theory-of-Stable-Diffusion#diffusion)  
[[20240423] 이미지 처리 : Stable Diffusion과 Diffusion Model](https://velog.io/@jsyun0412/20240423-Stable-Diffusion%EC%9D%B4%EB%9E%80)  