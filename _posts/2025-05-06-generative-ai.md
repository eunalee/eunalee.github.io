---
title: 생성형 AI
date: 2025-05-06
categories: [AI, AI]
tags: [AI, AI]
---

## 생성형 AI
새로운 콘텐츠(텍스트, 이미지, 비디오, 음악 등)를 생성할 수 있는 인공지능  
ANI 의 일종  
머신러닝 모델을 사용하여 사람이 만든 <span style="color:#337ea9">__기존 콘텐츠 패턴을 학습하고 더 나아가 입력된 데이터의 속성을 모방하는 새로운 데이터를 생성__</span>


## 생성형 AI 모델의 종류
데이터 학습을 통해 잠재 변수의 확률 분포를 추정해 <span style="color:#337ea9">__기존 데이터와 같은 확률적 특성을 갖는 새로운 데이터를 임의로 생성__</span>

<blockquote class="prompt-info">
    <p>
        <b>잠재변수</b><br>
        데이터로부터 직접적으로 관찰되지 않는 변수<br>
        생성형 AI 모델로 학습할 수 있으며 이 정보를 활용하여 새로운 데이터를 생성할 수 O
    </p>
</blockquote>
<br>

- __GAN (Generatvie Adverarial Networks)__  
<span style="color:#337ea9">__생성기와 판별기가 경쟁적으로 학습__</span>하여 기존의 데이터와 유사한 새로운 데이터를 생성하는 모델  
    - 생성기 (Generator) : 새로운 데이터 생성
    - 판별기 (Discriminator)  : 생성기에서 만들어낸 데이터의 진위 여부 평가  
    <br>
    1. 판별기에 진짜 데이터를 학습시켜 훈련 데이터 분포 생성  
    2. 생성기에서 노이즈를 가미해 가짜 데이터를 생성하고 판별기는 가짜 데이터를 구별  
    3. 생성기에서는 점점 더  진짜와 가까운 데이터를 만들어내고 판별기는 그 데이터의 진위 여부를 구분하는 행위를 반복해 훈련된 데이터 분포를 따라감  
<br>

- __VAE (Variational Autoencoder)__  
<span style="color:#337ea9">__인코더와 디코더로 구성되어 입력 데이터의 특징을 학습__</span>하여 새로운 데이터를 생성하는 모델  
    - 인코터 (Encoder) : 입력 데이터를 저차원 잠재 공간의 확률 분포로 매핑하여 중요한 특징만 추출  
    - 디코더 (Decoder) : 잠재 공간으로부터 특징을 받아 원래 입력 데이터를 재구성하여 출력  
    <br>
    1. 입력 데이터를 인코딩하여 각 잠재 변수에 대한 확률 분포를 출력  
    2. 잠재 변수 값을 랜덤하게 뽑은 뒤, 디코딩  
<br>

- __LLM (Large Language Model)__  
<span style="color:#337ea9">__방대한 텍스트 데이터를 학습__</span>하는 모델  
Transformer 구조를 활용해 주어진 프롬프트를 이해하고 답변 생성  
    <blockquote class="prompt-info">
        <p>
            <b>Transformer (트랜스포머) 구조</b><br>
            <span style="color:#337ea9"><b>병렬 처리</b></span>를 통해 효율적으로 데이터를 처리할 수 있는 구조<br>
            <span style="color:#337ea9"><b>Self-Attention 매커니즘</b></span>을 핵심으로 입력된 텍스트의 각 부분이 다른 부분과 어떻게 관련되는지 학습하고 이를 바탕으로 문장 내 단어 간의 관계를 이해
        </p>
    </blockquote>


## 분야별 생성형 AI 모델
- __텍스트__  
    - ChatGPT
    - Perplexity  

- __이미지__  
    - Midjourney
    - DALL-E
    - Stable Diffusion  

- __오디오__  
    - Suno
    - Udio  

- __비디오__  
    - Runway  


##  참고
[산업 전반에 결합하고 있는 생성형AI (1)편 – 개념, 동향](https://ahha.ai/2023/11/17/genai1/)  
[VAE(Variational AutoEncoder)](https://gaussian37.github.io/dl-concept-vae/)  
[AI-hub 공공데이터를 활용하여 한국어-영어 번역 LLM 만들기 (2) 모델 불러오기](https://bestkcs1234.tistory.com/85)  