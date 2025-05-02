---
title: 파이썬 데이터 시각화 도구
date: 2025-04-24
categories: [Programming, Python]
tags: [Programming, Python]
---

## matplotlib
데이터 시각화를 위한 파이썬 라이브러리

### 특징
- 다양한 그래프 지원
- Numpy, Pandas 와 연동이 잘됨

### 사용
1. matplotlib 패키지 설치  
  ```python
  %pip install matplotlib
  ```  

2. 모듈 가져오기  
  ```python
  import matplotlib.pyplot as plt
  ```  

### 그래프
- 선(plot) - 연속적인 데이터 변화 표현  
  ```python
  # 그래프 제목
  plt.title('plot')

  # x, y축 라벨링
  plt.xlabel('월')
  plt.ylabel('강수량')

  # 선 그래프 그리기 - 월별 강수량
  plt.plot([1, 2, 3, 4, 5], [40, 15, 10, 20, 25], color='limegreen')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/matplotlib_plot.png)


- 막대(bar) - 범주형 데이터 값 비교  
  ```python
  # 그래프 제목
  plt.title('bar')

  # x, y축 라벨링
  plt.xlabel('토이스토리 시리즈')
  plt.ylabel('누적관객수')

  # 막대 그래프 그리기 - 영화 토이스토리 시리즈별 누적관객수
  plt.bar(['시리즈1', '시리즈2', '시리즈3', '시리즈4'], [200000, 300000, 3000000, 2250000])

  # 그래프 숫자 표기법 변경
  plt.ticklabel_format(axis='y', useOffset=False, style='plain')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/matplotlib_bar.png)

- 산점도(scatter) - 두 변수간 상관관계 파악  
  ```python
  # 그래프 제목
  plt.title('scatter')

  # x, y축 라벨링
  plt.xlabel('키')
  plt.ylabel('몸무게')

  # 산점도 그리기 - 키별 몸무게
  plt.scatter([160, 158, 187, 174, 169, 165, 170, 164], [52, 55, 72, 68, 63, 65, 53, 60], s=200, c='orange', alpha=0.5)

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/matplotlib_scatter.png)

- 히스토그램(hist) - 데이터 분포 표현  
  ```python
  # 그래프 제목
  plt.title('histogram')

  # x, y축 라벨링
  plt.xlabel('점수')
  plt.ylabel('학생수')

  # 히스토그램 그리기 - 점수별 학생수 분포
  plt.hist([59, 71, 66, 81, 80, 68, 77, 74, 73, 72, 70], bins=4, alpha=0.5)

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/matplotlib_hist.png)

- 박스 플롯(boxplot) - 변수 분포를 요약하여 표현  
  ```python
  # 그래프 제목
  plt.title('boxplot')

  # x, y축 라벨링
  plt.xlabel('월')
  plt.ylabel('코로나 확진자수')

  # 박스 플롯 그리기 - 월별 코로나 확진자수
  plt.boxplot([np.random.normal(400, 15, 1000), np.random.normal(600, 30, 1000)])

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/matplotlib_boxplot.png)
<br/>

## seaborn
데이터 시각화를 위한 파이썬 라이브러리

### 특징
- matplotlib 을 기반으로 만들어짐
- Pandas 자료형과 연동이 잘됨
- 기본 dataset 제공

### 사용
1. seaborn 패키지 설치  
  ```python
  %pip install seaborn
  ```  

2. 모듈 가져오기  
  ```python
  import seaborn as sns
  ```  

### 그래프
  ```python
  # 기본 데이터셋 penguins 가져오기
  df = sns.load_dataset('penguins')
  df.head()
  ```  

- 선(lineplot) - 연속적인 데이터 변화 표현  
  ```python
  # 그래프 제목
  plt.title('lineplot')

  # 선 그래프 그리기 - 펭귄 종별 날개 길이에 따른 몸무게
  sns.lineplot(data=df, x='flipper_length_mm', y='body_mass_g', hue='species')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_lineplot.png)

- 막대(countplot/barplot) - 범주형 데이터 값 비교  
  \- countplot() : 데이터별 갯수
  ```python
  # 그래프 제목
  plt.title('countplot')

  # 막대 그래프 그리기 - 종별, 성별 펭귄수
  sns.countplot(data=df, x='species', hue='sex')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_countplot.png)
  
  \- barplot() : 데이터에 대한 값의 크기
  ```python
  # 그래프 제목
  plt.title('barplot')

  # 막대 그래프 그리기 - 펭귄 종별 부리 길이
  sns.barplot(data=df, x='species', y='bill_length_mm', hue='species')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_barplot.png)

- 산점도(scatterplot) - 두 변수간 상관관계 파악  
  ```python
  # 그래프 제목
  plt.title('scatterplot')

  # 산점도 그리기 - 펭귄 종별 날개 길이에 따른 몸무게
  sns.scatterplot(data=df, x='flipper_length_mm', y='body_mass_g', hue='species')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_boxplot.png)

- 히스토그램(histplot) - 데이터 분포 표현  
  ```python
  # 그래프 제목
  plt.title('histplot')

  # 히스토그램 그리기 - 거주 지역에 따른 종별 펭귄수
  sns.histplot(data=df, x='island', hue='species', multiple='stack')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_histplot.png)

- 박스 플롯(boxplot) - 변수 분포를 요약하여 표현  
  ```python
  # 그래프 제목
  plt.title('boxplot')

  # 박스 플롯 그리기 - 몸무게에 따른 펭귄 종별, 성별 분포
  sns.boxplot(data=df, x='body_mass_g', y='species', hue='sex')

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_boxplot.png)

- 히트맵(heatmap) - 색상 강도로 데이터 분포 표현  
  ```python
  # 그래프 제목
  plt.title('heatmap')

  # 히트맵 그리기 - 종별 펭귄 몸무게의 최소값, 평균값, 최대값
  # penguins_pivot : 가공한 데이터
  sns.heatmap(data=penguins_pivot, annot=True).set(xlabel='weight(kg)')

  # y축 라벨 정렬
  plt.yticks(rotation=0)

  # 그래프 화면에 표현
  plt.show()
  ```  
![ ](/assets/img/posts/seaborn_heatmap.png)
<br/>

## folium
지도 시각화를 위한 파이썬 라이브러리

### 특징
- 지도 위에 다양한 정보 표현 가능

### 사용
1. folium 패키지 설치  
  ```python
  %pip install folium
  ```  

2. 모듈 가져오기  
  ```python
  import folium
  ```  
  
### 지도
- 지도 생성
  ```python
  # 서울시 중심부 지도 그리기
  m = folium.Map(location=[37.566535, 126.9779692], zoom_start=12)
  m
  ```  
![ ](/assets/img/posts/folium_map.png)

- 마커 추가
  ```python
  # 클릭 시, 장소명(경복궁)이 나타나는 마커 추가
  folium.Marker(location=[37.5759, 126.9768], tooltip='Click', popup='경복궁', icon=folium.Icon(color='red', icon='star')).add_to(m)
  m
  ```  
![ ](/assets/img/posts/folium_marker.png)

- 지도 저장
  ```python
  # HTML 파일로 저장
  m.save('Gyeongbokgung.html')
  ```  
  
- 데이터 표현
  ```python
  # 데이터 표현할 서울시 중심부 지도 그리기
  m = folium.Map(location=[37.566535, 126.9779692], zoom_start=11)

  # 서울시 지역구별 위치 정보를 바탕으로 지역구별 인구수 데이터 지도에 표현하기
  # seoul_geo : 서울시 지역구별 위치 정보 (by. json 파일)
  # seoul_total_popluation : 지역구별 인구수 데이터 지도 (by. csv 파일)
  folium.Choropleth(geo_data=seoul_geo, data=seoul_total_popluation, columns=['지역구', '전체'], fill_color='YlGnBu', key_on='feature.properties.name').add_to(m)
  m
  ```  
![ ](/assets/img/posts/folium_choropleth.png)