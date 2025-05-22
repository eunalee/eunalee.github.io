---
title: 분류(Classification) - 붓꽃 품종 예측
date: 2025-05-17
categories: [Artificial Intelligence, Machine Learning]
tags: [Artificial Intelligence, Machine Learning]
---
## 붓꽃 품종 예측
붓꽃의 특성(꾳잎의 길이와 너비, 꽃받침의 길이와 너비)을 기반으로 품종을 예측한다.


### 데이터 수집
- __Python__  
  - sklearn 에서 제공하는 샘플 데이터 활용  
  ```python
  from sklearn.datasets import load_iris
  iris = load_iris()
  df_iris = pd.DataFrame(iris.data, columns=iris.feature_names)
  df_iris['species'] = iris.target
  ```
  ![ ](/assets/img/posts/classification/python_iris_dataset.png)  

- __Azure__  
  - 등록한 데이터셋 가져오기 
![ ](/assets/img/posts/classification/azure_iris_dataset.png)  


### 데이터 탐색(EDA)
- __Python__
  - 데이터 정보 확인  
  ```python
  df_iris.info()
  >> <class 'pandas.core.frame.DataFrame'>
       RangeIndex: 150 entries, 0 to 149
       Data columns (total 5 columns):
       #   Column             Non-Null Count  Dtype  
       ---  ------             --------------  -----  
       0   sepal length (cm)  150 non-null    float64
       1   sepal width (cm)   150 non-null    float64
       2   petal length (cm)  150 non-null    float64
       3   petal width (cm)   150 non-null    float64
       4   species            150 non-null    int64  
       dtypes: float64(4), int64(1)
       memory usage: 6.0 KB
  ```
  👉 __<span style="color:#337ea9">모든 데이터는 수치형</span>__  
  👉 __<span style="color:#337ea9">결측치X</span>__
  
  - 시각화  
  ```python
  import matplotlib.pyplot as plt
  import seaborn as sns
  sns.pairplot(data=df_iris, hue='species', palette='muted')
  plt.show()
  ```
  ![ ](/assets/img/posts/classification/python_iris_visualization.png)  
  👉 __<span style="color:#337ea9">petal length, petal width 두 독립변수가 종속변수에 영향을 미칠 것으로 예상</span>__  
  👉 __<span style="color:#337ea9">0번 종의 붓꽃은 분류가 잘 될 것으로 예상</span>__  

- __Azure__  
  - 시각화  
  ![ ](/assets/img/posts/classification/azure_iris_dataset_outputs.png)  
  👉 __<span style="color:#337ea9">편향된 데이터X</span>__  
  👉 __<span style="color:#337ea9">결측치X</span>__  
  👉 __<span style="color:#337ea9">최소값과 최대값의 범위 차이가 있어 스케일링 필요</span>__  

### 데이터 전처리
- __Python__  
  - 데이터 선택  
    \- 독립변수(X) : sepal length (cm), sepal width (cm), petal length (cm)  
    \- 종속변수(y) : species  
    ```python
    X = df_iris.drop('species', axis=1)
    y = df_iris['species']
    ```  

  - 학습/테스트용 데이터 분리  
    \- 데이터 비율 : 학습용 데이터(70%), 테스트용 데이터(30%)  
    ```python
    from sklearn.model_selection import train_test_split

    # stratify=y : 종속변수 "species" 균등 분할
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, stratify=y)

    # 데이터 크기 확인
    print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)

    >>> ((105, 4), (45, 4), (105,), (45,))  
    ```  

  - 스케일링 (MinMax)
    ```python
    from sklearn.preprocessing import MinMaxScaler
    # 스케일러
    scaler = MinMaxScaler()

    # 학습용 데이터세트
    X_train_scaled = scaler.fit_transform(X_train)
    # 테스트용 데이터세트
    X_test_scaled = scaler.transform(X_test)

    >> array([[0.26470588, 0.31818182, 0.49152542, 0.54166667],
              [0.23529412, 0.81818182, 0.15254237, 0.125     ],
              [0.08823529, 0.63636364, 0.06779661, 0.08333333],
              [0.64705882, 0.45454545, 0.81355932, 0.875     ],
              [0.32352941, 0.63636364, 0.11864407, 0.04166667],
              ...
              [0.20588235, 0.72727273, 0.06779661, 0.04166667],
              [0.38235294, 0.22727273, 0.49152542, 0.41666667],
              [0.79411765, 0.54545455, 0.62711864, 0.54166667],
              [0.85294118, 0.72727273, 0.86440678, 1.        ],
              [0.20588235, 0.        , 0.42372881, 0.375     ]])
    ```

- __Azure__
  - 결측치 처리  
    \- 결측치가 있는 행 제거  
    ![ ](/assets/img/posts/classification/azure_iris_clean_missing_data.png)  
    __<span style="color:#337ea9">결측치는 없으나 새로 들어올 데이터를 위해 진행!</span>__  

  - 스케일링 (MinMax)  
    \- 정답 레이블 (species) 제외  
    ![ ](/assets/img/posts/classification/azure_iris_normalize_data.png)  

    \- 스케일링 결과  
    ![ ](/assets/img/posts/classification/azure_iris_normalize_results.png)  

  - 학습/테스트용 데이터 분리  
    \- 데이터 비율 : 학습용 데이터(70%), 테스트용 데이터(30%)  
    ![ ](/assets/img/posts/classification/azure_iris_split_data.png)  

### 데이터 모델링
- __Python__  
  - 모델 생성  
    \- 알고리즘 : RandomForestClassifier  
    ```python
    from sklearn.ensemble import RandomForestClassifier
    model = RandomForestClassifier(n_estimators=32, max_depth=8, min_samples_leaf=1, bootstrap=True)
    ```
  
  - 모델 학습  
  ```python
  model.fit(X_train_scaled, y_train)
  ```

- __Azure__
  - 모델 생성  
    \- 알고리즘 : Multiclass Decision Forest  
    ![ ](/assets/img/posts/classification/azure_iris_algorithm.png)  
  
  - 모델 학습  
    \- 정답 레이블 (species) 지정  
    ![ ](/assets/img/posts/classification/azure_iris_train_model.png)  

### 모델 성능 평가
- __Python__  
  - 모델 테스트  
  ```python
  pred = model.predict(X_test_scaled)
  print(f'예측값: {pred}')
  print(f'실제값: {np.array(y_test)}')
  >> 예측값: [2 1 1 1 2 0 2 0 2 0 1 2 0 0 2 2 1 1 1 1 2 1 0 0 1 0 0 1 0 1 2 1 2 2 0 0 1 0 1 1 0 2 2 0 1]
       실제값: [2 1 1 1 2 0 2 0 2 0 1 2 0 0 2 2 1 2 1 1 2 1 0 0 1 0 0 1 0 1 2 1 2 2 0 0 1 0 1 1 0 2 2 0 2]
  ```

  - 모델 평가  
  ```python
  from sklearn.metrics import accuracy_score
  print(f'예측 정확도(accuracy_score): {accuracy_score(y_test, pred):.2f}')
  print(f'예측 정확도(model.score): {model.score(X_test_scaled, y_test):.2f}')
  >> 예측 정확도(accuracy_score): 0.96
       예측 정확도(model.score): 0.96
  ```
  
  - 평가지표 확인  
  ```python
  from sklearn.metrics import classification_report
  print(classification_report(y_test, pred))
  >>                precision    recall  f1-score   support
  
               0       1.00      1.00      1.00        15
               1       0.88      1.00      0.94        15
               2       1.00      0.87      0.93        15
           
        accuracy                           0.96        45
       macro avg       0.96      0.96      0.96        45
    weighted avg       0.96      0.96      0.96        45
  ```
- __Azure__
  - 모델 테스트  
  ![ ](/assets/img/posts/classification/azure_iris_score_model.png)  

  - 모델 평가  
  ![ ](/assets/img/posts/classification/azure_iris_evaluate_model.png)  

  - 평가지표 확인  
  ![ ](/assets/img/posts/classification/azure_iris_model_metrics.png)  