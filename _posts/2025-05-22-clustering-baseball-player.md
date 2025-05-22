---
title: 군집(Clustering) - 프로야구 선수 군집화
date: 2025-05-22
categories: [Artificial Intelligence, Machine Learning]
tags: [Artificial Intelligence, Machine Learning]
---
## 프로야구 선수 군집화
한국 프로야구에서 타자 능력에 따른 선수들을 군집화하여 향후 전략 수립(경기, 훈련, 연봉 등)에 활용한다.


### 데이터 수집
KBO 제공 데이터 사용 - 2000~2001 / 2002~2013 / 2014  
__<span style="color:#337ea9">년도별 데이터가 달라 추가 작업 필요!</span>__

- __Python__  
  - 데이터 csv 파일 읽어오기  
  \- 2000~2001  
  ```python
  baseball1 = pd.read_csv('data/2000_2001_hitter.csv')
  ```  
  ![ ](/assets/img/posts/clustering/python_baseball_dataset_2000_2001.png)  
  \- 2002~2013  
  ```python
  baseball2 = pd.read_csv('data/2002_2013_hitter.csv')
  ```  
  ![ ](/assets/img/posts/clustering/python_baseball_dataset_2002_2013.png)  
  \- 2014  
  ```python
  baseball3 = pd.read_csv('data/2014_hitter.csv')
  ```  
  ![ ](/assets/img/posts/clustering/python_baseball_dataset_2014.png)  

  - 데이터 병합  
  ```python
  hitter = pd.concat([baseball1, baseball2, baseball3], ignore_index=True)
  ```  
  ![ ](/assets/img/posts/clustering/python_baseball_dataset_total.png)  

- __Azure__  
  - 등록한 데이터셋 가져오기 
![ ](/assets/img/posts/clustering/azure_baseball_dataset.png)  
  
  - 데이터 병합
![ ](/assets/img/posts/clustering/azure_baseball_dataset_add_rows.png)  

### 데이터 탐색(EDA)
- __Python__  
  - 데이터 정보 확인  
    ```python
    df.info()
    >> <class 'pandas.core.frame.DataFrame'>
       RangeIndex: 649 entries, 0 to 648
       Data columns (total 41 columns):
       #   Column    Non-Null Count  Dtype  
       ---  ------    --------------  -----  
        0   YrPlayer  649 non-null    object 
        1   Year      649 non-null    int64  
        2   Rank      649 non-null    int64  
        3   Player    649 non-null    object 
        4   Team      649 non-null    object 
        5   AVG       649 non-null    float64
        6   G         649 non-null    int64  
        7   PA        649 non-null    int64  
        8   AB        649 non-null    int64  
        9   H         649 non-null    int64  
        10  1B        649 non-null    int64  
        11  2B        649 non-null    int64  
        12  3B        649 non-null    int64  
        13  HR        649 non-null    int64  
        14  RBI       649 non-null    int64  
        15  SB        648 non-null    float64
        16  CS        648 non-null    float64
        17  BB        649 non-null    int64  
        18  HBP       649 non-null    int64  
        19  SO        649 non-null    int64  
        ...
        39  RISP      560 non-null    float64
        40  PH-BA     560 non-null    float64
        dtypes: float64(20), int64(18), object(3)
        memory usage: 208.0+ KB
    ```
    👉 __<span style="color:#337ea9">대부분 데이터는 숫자형</span>__  

    ```python
    hitter.isnull().sum().sort_values(ascending=False)
    >> E           560
       SH          560
       SAC          89
       R            89
       PH-BA        89
       RISP         89
       MH           89
       SB            1
       CS            1
       AB            0
       PA            0
       G             0
       AVG           0
       Team          0
       Player        0
       Rank          0
       Year          0
       YrPlayer      0
       HR            0
       BB            0
       RBI           0
       3B            0
       2B            0
       H             0
       1B            0
       ...
       OPS           0
       XR            0
       RC/27         0
       wOBA          0
       dtype: int64
    ```
    👉 __<span style="color:#337ea9">결측치O</span>__  

  - 시각화  
    ```python
    hitter.select_dtypes('number').plot(kind='box', figsize=(10, 10))
    ```  
    ![ ](/assets/img/posts/clustering/python_baseball_visualization.png)  
    👉 __<span style="color:#337ea9">수치형 데이터의 경우, 최소값과 최대값의 범위 차이가 있어 스케일링 필요</span>__  

- __Azure__  
  - 시각화  
    \- 2000~2001
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2000_2001_outputs_1.png)  
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2000_2001_outputs_2.png)  
    \- 2002~2013
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2002_2013_outputs_1.png)  
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2002_2013_outputs_2.png) 
    \- 2014
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2014_outputs_1.png)  
      ![ ](/assets/img/posts/clustering/azure_baseball_dataset_2014_outputs_2.png) 
    👉 __<span style="color:#337ea9">독립변수들이 전체적으로 양의 왜도를 가짐</span>__  
    👉 __<span style="color:#337ea9">수치형 데이터의 경우, 최소값과 최대값의 범위 차이가 있어 스케일링 필요</span>__  
    👉 __<span style="color:#337ea9">결측치X</span>__  


### 데이터 전처리
- __Python__  
  - 데이터 선택  
    \- 독립변수(X) : OPS, ISO, SECA, TA, RC, RC/27, wOBA, XR  
    \- 종속변수(y) : YrPlayer (선수 이름 군집화X)  
    \- 종속변수 정답 레이블로 사용X  
    ```python
    X = hitter[['OPS', 'ISO', 'SECA', 'TA', 'RC', 'RC/27', 'wOBA', 'XR']]
    y = hitter['YrPlayer']
    ```  

  - 스케일링  
    ```python
    from sklearn.preprocessing import StandardScaler
    # 스케일러
    scaler = StandardScaler()
    X_scaled = scaler.fit_transform(X)
    >> array([[ 0.97614887, -0.07422939,  0.10417649, ...,  1.20484091,
                1.11880719,  1.21433377],
              [ 1.89711625,  1.67989079,  1.05416505, ...,  1.75823146,
                1.89486616,  1.85744936],
              [ 1.10761611,  0.61991005, -0.16048453, ...,  1.1714146 ,
                1.13904003,  0.42420225],
              ...,
              [-0.42233048, -0.05169927,  0.77350431, ..., -0.32541846,
               -0.43538549, -0.33597768],
              [-1.46085838, -1.39601302, -1.62726689, ..., -1.37885116,
               -1.47288829, -1.11957047],
              [-1.28467954, -1.24146838, -0.64887581, ..., -1.19083305,
               -1.21125882, -1.3345048 ]], shape=(649, 8))
    ```  

    \- 독립변수에 값 할당   
    ```python
    X.loc[:, 'OPS':'XR'] = X_scaled
    ```  

  - 주성분 분석  
    \- 8개로 차원 축소
    ```python
    from sklearn.decomposition import PCA
    pca = PCA(n_components=8)
    X_pca = pca.fit_transform(X)
    >> array([[ 2.48864775, -1.29871071, -0.18369185, ...,  0.11512947,
               -0.05012084, -0.00542238],
              [ 4.8638416 , -0.32843484,  0.04378699, ...,  0.02815584,
               -0.05298553,  0.00816747],
              [ 2.05850718, -0.4397998 , -0.79470761, ...,  0.02887054,
               -0.04552095, -0.02742987],
              ...,
              [-0.3971905 ,  0.69084294,  0.14211226, ...,  0.09606087,
               -0.0169874 ,  0.04994242],
              [-3.94087277, -0.42488303,  0.27501545, ..., -0.11415355,
                0.00769498,  0.00666951],
              [-3.33227219,  0.22550401, -0.14325161, ..., -0.0513692 ,
                0.01871363, -0.02078552]], shape=(649, 8))
    ```  

    \- 각 컬럼별 분산 비율 확인  
    ```python
    pca.explained_variance_ratio_
    >> array([0.90514573, 0.05062617, 0.02188854, 0.0184561 , 0.00299299, 0.00055624, 0.00018392, 0.00015031])
    ```  

    \- 독립변수에 사용할 값 할당   
    __<span style="color:#337ea9">OPS, ISO 만으로도 분산 비율이 95% 이상이므로 2가지 특성만 사용!</span>__
    ```python
    X.loc[:, 'OPS':'ISO'] = X_pca[:, :2]
    ```  

  - 최적의 군집 갯수 찾기(elbow)    
    \- 군집 갯수를 2부터 7까지 늘려가며 이너셔 찾기  
    👉__<span style="color:#337ea9">3개 or 4개로 군집화!</span>__
    ```python
    from sklearn.cluster import KMeans
    inertia = []
    for n in range(2, 7) :
        km = KMeans(n)
        km.fit(X)
        inertia.append(km.inertia_)
    
    # 이너셔 시각화
    plt.plot(range(2, 7), inertia, marker='o')
    ```  
    ![ ](/assets/img/posts/clustering/python_baseball_elbow.png) 

- __Azure__
  - 스케일링 (ZScore)    
    \- 수치형 데이터 (YrPlayer 제외)  
    ![ ](/assets/img/posts/clustering/azure_baseball_normalize_data.png) 
    \- 스케일링 결과    
    ![ ](/assets/img/posts/clustering/azure_baseball_normalize_data_results.png) 

  - 주성분 분석  
    \- 8개로 차원 축소  
    ![ ](/assets/img/posts/clustering/azure_baseball_pca.png) 
    \- 차원 축소 결과   
    ![ ](/assets/img/posts/clustering/azure_baseball_pca_results.png)  
    \- 각 컬럼별 표준편차 확인  
    __<span style="color:#337ea9">OPS, ISO 만으로도 분산 비율이 95% 이상이므로 2가지 특성만 사용!</span>__   
    <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/azure_baseball_pca_results_OPS.png"></td>
          <td><img src="/assets/img/posts/clustering/azure_baseball_pca_results_ISO.png"></td>
          <td><img src="/assets/img/posts/clustering/azure_baseball_pca_results_SECA.png"></td>
      </tr>
    </table>


  - 데이터 선택  
    \- 독립변수(X) : OPS, ISO  
    \- 종속변수(y) : YrPlayer (선수 이름 군집화X)  
    \- 종속변수 테스트용 데이터로 사용  
    ![ ](/assets/img/posts/clustering/azure_baseball_select_columns.png) 

  - 학습/테스트용 데이터 분리    
    \- 데이터 비율 : 학습용 데이터(2000~2013), 테스트용 데이터(2014)    
    __<span style="color:#337ea9">YrPlayer 컬럼에서 2014로 시작하지 않는 행 선택(정규식) : \"YrPlayer" ^(?!2014).*</span>__  
    ![ ](/assets/img/posts/clustering/azure_baseball_split_data.png) 


### 데이터 모델링
- __Python__  
  - 모델 생성  
    \- 알고리즘 : K-means clustering  
    ```python
    from sklearn.cluster import KMeans
    # 군집 갯수 : 3개
    km = KMeans(n_clusters=3)
    # 군집 갯수 : 4개
    km = KMeans(n_clusters=4)
    ```
  
  - 모델 학습  
  ```python
  km.fit_transform(X)
  ```

- __Azure__
  - 모델 생성  
    \- 알고리즘 : K-means clustering  
    \- 군집 갯수 : 3개  
    ![ ](/assets/img/posts/clustering/azure_baseball_algorithm_3.png) 
    \- 군집 갯수 : 4개  
    ![ ](/assets/img/posts/clustering/azure_baseball_algorithm_4.png) 
  
  - 모델 학습  
    \- 군집화 할 레이블 (OPS, ISO) 지정    
    ![ ](/assets/img/posts/clustering/azure_baseball_train_clustering_model.png) 

### 모델 성능 평가
- __Python__  
  - 모델 테스트  
  ```python
    from sklearn.metrics import silhouette_score
    # 평균 실루엣 계수 : silhouette_score(학습 데이터, 군집 결과)
    silhouette_score(X, km.labels_)
    
    # 군집 갯수 : 3개
    >> 0.47741718612536477
    
    # 군집 갯수 : 4개
    >> 0.5190652496748217
  ```  

  - 평가지표 확인  
  1) 실루엣 차트 : 외부 파이썬 함수 모듈 import 해서 차트 그리기 
  ```python
    import silhouette_analysis as s
    # 군집 갯수 : 3개
    s.silhouette_plot(X, 3)
    
    # 군집 갯수 : 4개
    s.silhouette_plot(X, 4)
  ```  
  <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/python_baseball_silhouette_3.png"></td>
          <td><img src="/assets/img/posts/clustering/python_baseball_silhouette_4.png"></td>
      </tr>
      <tr>
          <td>군집 갯수 : 3개</td>
          <td>군집 갯수 : 4개</td>
      </tr>
  </table>
  
  2) 산점도 
  ```python
    # 학습 데이터에 군집 결과 추가
    X['kmeans_cluster'] = km.labels_
  ```  
  <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/python_baseball_train_results_3.png"></td>
          <td><img src="/assets/img/posts/clustering/python_baseball_train_results_4.png"></td>
      </tr>
      <tr>
          <td>군집 갯수 : 3개</td>
          <td>군집 갯수 : 4개</td>
      </tr>
  </table>
   ```python
    # 군집화 결과 시각화
    sns.scatterplot(data=X, x='OPS', y='ISO', hue='kmeans_cluster', palette='muted')
    
    # 개별 군집의 중심 좌표
    sns.scatterplot(x=km.cluster_centers_[:, 0], y=km.cluster_centers_[:, 1], marker='X', color='black')
  ```  
  <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/python_baseball_scatter_3.png"></td>
          <td><img src="/assets/img/posts/clustering/python_baseball_scatter_4.png"></td>
      </tr>
      <tr>
          <td>군집 갯수 : 3개</td>
          <td>군집 갯수 : 4개</td>
      </tr>
  </table>

- __Azure__
  - 모델 테스트  
    \- 2014년 데이터로 군집 예측
    ![ ](/assets/img/posts/clustering/azure_baseball_assign_data_to_clusters.png) 

  - 모델 평가  
    ![ ](/assets/img/posts/clustering/azure_baseball_evaluate_model.png) 

  - 평가지표 확인  
    \- 군집 갯수 : 3개  
    __<span style="color:#337ea9">군집 예측 결과 및 군집 중심과의 거리</span>__  
    ![ ](/assets/img/posts/clustering/azure_baseball_model_metrics_3_1.png) 

    __<span style="color:#337ea9">분리도(다른 군집과의 거리), 응집도(군집 중심과의 거리), 군집 내 데이터 갯수, 분산</span>__  
    ![ ](/assets/img/posts/clustering/azure_baseball_model_metrics_3_2.png) 

    \- 군집 갯수 : 4개  
    __<span style="color:#337ea9">군집 예측 결과 및 군집 중심과의 거리</span>__  
    ![ ](/assets/img/posts/clustering/azure_baseball_model_metrics_4_1.png) 
    
    __<span style="color:#337ea9">분리도(다른 군집과의 거리), 응집도(군집 중심과의 거리), 군집 내 데이터 갯수, 분산</span>__  
    ![ ](/assets/img/posts/clustering/azure_baseball_model_metrics_4_1.png) 

    \- 시각화  
    학습 데이터에 군집 결과 추가  
    ![ ](/assets/img/posts/clustering/azure_baseball_train_results_add_rows.png)  

    1) 실루엣 차트  
    ![ ](/assets/img/posts/clustering/azure_baseball_train_results_silhouette.png)  
    <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/azure_baseball_silhouette_3.png"></td>
          <td><img src="/assets/img/posts/clustering/azure_baseball_silhouette_4.png"></td>
      </tr>
      <tr>
        <td>군집 갯수 : 3개</td>
        <td>군집 갯수 : 4개</td>
      </tr>
    </table>

    2) 산점도  
    ![ ](/assets/img/posts/clustering/azure_baseball_train_results_scatter.png)  
    <table>
      <tr>
          <td><img src="/assets/img/posts/clustering/azure_baseball_scatter_3.png"></td>
          <td><img src="/assets/img/posts/clustering/azure_baseball_scatter_4.png"></td>
      </tr>
      <tr>
        <td>군집 갯수 : 3개</td>
        <td>군집 갯수 : 4개</td>
      </tr>
    </table>