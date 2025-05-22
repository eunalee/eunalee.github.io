---
title: 회귀(Regression) - 자전거 대여량 예측
date: 2025-05-22
categories: [Artificial Intelligence, Machine Learning]
tags: [Artificial Intelligence, Machine Learning]
---
## 자전거 수요 예측
여러 요인(기온, 습도, 바람, 계절 등)을 바탕으로 공공 자전거 대여량을 예측한다.


### 데이터 수집
UCI 제공 데이터 사용

- __Python__  
  - 데이터 csv 파일 읽어오기  
  ```python
  df = pd.read_csv('data/bike_sharing_demand.csv', parse_dates=['datetime'])
  ```
  ![ ](/assets/img/posts/regression/python_bike_rental_dataset.png)  

- __Azure__  
  - 등록한 데이터셋 가져오기 
![ ](/assets/img/posts/regression/azure_bike_rental_dataset.png)  


### 데이터 탐색(EDA)
- __Python__  
  - 데이터 정보 확인  
  ```python
  df.info()
  >> <class 'pandas.core.frame.DataFrame'>
       RangeIndex: 10886 entries, 0 to 10885
       Data columns (total 12 columns):
        #   Column      Non-Null Count  Dtype         
       ---  ------      --------------  -----         
        0   datetime    10886 non-null  datetime64[ns]
        1   season      10886 non-null  int64         
        2   holiday     10886 non-null  int64         
        3   workingday  10886 non-null  int64         
        4   weather     10886 non-null  int64         
        5   temp        10886 non-null  float64       
        6   atemp       10886 non-null  float64       
        7   humidity    10886 non-null  int64         
        8   windspeed   10886 non-null  float64       
        9   casual      10886 non-null  int64         
        10  registered  10886 non-null  int64         
        11  count       10886 non-null  int64         
        dtypes: datetime64[ns](1), float64(3), int64(8)
        memory usage: 1020.7 KB
  ```
👉 __<span style="color:#337ea9">datetime 을 제외한 나머지 모든 데이터는 숫자형</span>__  
👉 __<span style="color:#337ea9">결측치X</span>__
  
  - 시각화  
    \- 시간대별 데이터  
    ```python
    plt.figure(figsize=(15,8))
    
    features = ['year','month','day','hour','dayofweek']
    for i, feature in enumerate(features):
        plt.subplot(2,3,i+1)
        sns.barplot(data=df, x=feature, y='count', estimator='mean', hue='year', palette='muted')
        plt.title(feature + '-count')
    
    plt.tight_layout()
    ```
    ![ ](/assets/img/posts/regression/python_bike_rental_visualization_datetime.png)  
    👉 __<span style="color:#337ea9">년도 데이터는 2개((2011, 2012)이지만 대여량 증가하는 추세</span>__  
    👉 __<span style="color:#337ea9">여름에 대여량 증가</span>__  
    👉 __<span style="color:#337ea9">날짜는 19일까지만 존재 (19일 이후는 테스트 데이터로 사용) ⇒ 완전하지 않은 데이터</span>__  

    \- 시간대 요일별 대여량  
    ```python
    sns.lineplot(data=df, x='hour', y='count', marker='o', hue='dayofweek', estimator='sum', palette='muted')
    plt.title('hour-dayofweek count')
    plt.xticks(range(0,24))
    plt.grid(ls=':')
    plt.show()
    ```
    ![ ](/assets/img/posts/regression/python_bike_rental_visualization_hour_dayofweek_count.png)  
    👉 __<span style="color:#337ea9">평일(0~4)과 주말(5~6)의 시간대별 대여 패턴이 다름</span>__  

    \- 공휴일  
    ```python
    plt.figure(figsize=(15,4))

    plt.subplot(131)
    plt.pie(df['holiday'].value_counts(), autopct='%.1f%%', labels=['0','1'])
    plt.title('holiday rate')

    plt.subplot(132)
    sns.barplot(data=df, x='holiday', y='count', errorbar=None, estimator='sum')
    plt.title('holiday sum count')

    plt.subplot(133)
    sns.barplot(data=df, x='holiday', y='count', errorbar=None, estimator='mean')
    plt.title('holiday mean count')
    
    plt.show()
    ```
    ![ ](/assets/img/posts/regression/python_bike_rental_visualization_holiday.png)  
    👉 __<span style="color:#337ea9">1년 중 공휴일의 비율은 2.9% 로 날짜수가 차이가 있으므로 평균 대여량으로 비교</span>__   
    
    \- 날씨  
    ```python
    plt.figure(figsize=(15,4))

    plt.subplot(131)
    plt.pie(df['weather'].value_counts(), autopct='%.1f%%', labels=[1,2,3,4])
    plt.title('weather rate')

    plt.subplot(132)
    sns.barplot(data=df, x='weather', y='count', errorbar=None, estimator='sum')
    plt.title('weather - sum count')

    plt.subplot(133)
    sns.barplot(data=df, x='weather', y='count', errorbar=None, estimator='mean')
    plt.title('weather - mean count')

    plt.show()
    ```
    ![ ](/assets/img/posts/regression/python_bike_rental_visualization_weather.png)  
    👉 __<span style="color:#337ea9">날씨가 좋은 날 대여량이 높음</span>__  

    \- 상관계수  
    ```python
    corr = df[['temp', 'atemp', 'humidity', 'windspeed', 'count']].corr()
    sns.heatmap(abs(corr), cmap='Blues', annot=True)
    ```
    ![ ](/assets/img/posts/regression/python_bike_rental_visualization_heatmap.png)  
    👉 __<span style="color:#337ea9">종속변수(count)와 독립변수 간 상관관계 확인</span>__  
    👉 __<span style="color:#337ea9">독립변수들간의 상관관계 확인</span>__  

- __Azure__  
  - 시각화  
![ ](/assets/img/posts/regression/azure_bike_rental_dataset_outputs_1.png)
![ ](/assets/img/posts/regression/azure_bike_rental_dataset_outputs_2.png)
👉 __<span style="color:#337ea9">월, 계절, 공휴일, 요일, 날씨 데이터는 수치형이지만 범주형 성격을 지님</span>__  
👉 __<span style="color:#337ea9">수치형 데이터의 경우, 최소값과 최대값의 범위 차이가 있어 스케일링 필요</span>__  

### 데이터 전처리
- __Python__  
  - 데이터 변형  
    \- datetime 파생컬럼 추가 
    ```python
    # 연, 월, 일, 시, 요일(월: 0 ~ 일: 6)
    df['year'] = df['datetime'].dt.year
    df['month'] = df['datetime'].dt.month
    df['day'] = df['datetime'].dt.day
    df['hour'] = df['datetime'].dt.hour
    df['dayofweek'] = df['datetime'].dt.dayofweek
    ```  
  ![ ](/assets/img/posts/regression/python_bike_rental_add_columns_datetime.png)

  - 데이터 선택  
    \- 독립변수(X) : season, holiday, workingday, weather, temp, humidity, windspeed, month, hour, dayofweek  
    \- casual, registered 는 독립변수 성격X  
    \- datetime 사용 불가  
    \- day 데이터는 불완전  
    \- temp, atemp 중 하나만 선택  
    \- 종속변수(y) : count  
    ```python
    X = df.drop(['count', 'casual', 'registered', 'datetime', 'day', 'atemp', 'year'], axis=1)
    y = df['count']
    ```  

  - 학습/테스트용 데이터 분리  
    \- 데이터 비율 : 학습용 데이터(70%), 테스트용 데이터(30%)  
    ```python
    from sklearn.model_selection import train_test_split
    
    # 데이터 분할
    X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3)

    # 데이터 크기 확인
    print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)
    
    >>> (7620, 10) (3266, 10) (7620,) (3266,)
    ```  

  - 스케일링 (MinMax)  
    \- 수치형 데이터 : temp, humidity, windspeed
    ```python
    from sklearn.preprocessing import StandardScaler
    # 수치형 변수 컬럼
    numerical_features = ['temp', 'humidity', 'windspeed']

    # 스케일러
    scaler = StandardScaler()

    # 학습용 데이터세트
    X_train_scaled = scaler.fit_transform(X_train[numerical_features])
    # 테스트용 데이터세트
    X_test_scaled = scaler.transform(X_test[numerical_features])

    >> array([[-0.28202412,  1.35910165,  0.27036039],
              [ 0.66751753,  0.37355282, -0.70427403],
              [-0.80954725, -0.45638303, -1.55728321],
              ...,
              [ 0.77302216,  0.37355282,  0.27036039],
              [ 1.3005453 ,  0.63290778, -0.46102356],
              [-1.54807965, -0.56012502,  0.51361085]], shape=(7620, 3))
    ```

  - One-Hot 인코딩  
    \- 범주형 데이터 : season, holiday, workingday, weather, month, hour, dayofweek
    ```python
    from sklearn.preprocessing import OneHotEncoder
    # 범주형 변수 컬럼
    categorical_features = X_train.drop(numerical_features, axis=1).columns
    
    # One-Hot 인코딩
    encoder = OneHotEncoder(handle_unknown='ignore', sparse_output=False)
    
    # 학습용 데이터세트
    X_train_ohe = encoder.fit_transform(X_train[categorical_features])
    # 테스트용 데이터세트
    X_test_ohe = encoder.transform(X_test[categorical_features])

    >> array([[0., 0., 0., ..., 0., 0., 0.],
              [0., 0., 1., ..., 0., 1., 0.],
              [1., 0., 0., ..., 0., 0., 0.],
              ...,
              [0., 0., 1., ..., 0., 0., 0.],
              [0., 0., 1., ..., 0., 0., 1.],
              [1., 0., 0., ..., 1., 0., 0.]], shape=(7620, 55))
    ```

  - 학습/테스트용 데이터세트 최종본 생성  
    \- 수치형, 범주형 데이터 합치기
    ```python
    # ---------------------------
    # 수치형 데이터
    # ---------------------------
    # 학습용 데이터세트
    X_train_scaled = pd.DataFrame(X_train_scaled, columns=numerical_features, index=X_train.index)
    # 테스트용 데이터세트
    X_test_scaled = pd.DataFrame(X_test_scaled, columns=numerical_features, index=X_test.index)
    
    # ---------------------------
    # 범주형 데이터
    # ---------------------------
    # One-Hot 인코딩 후, 생성된 컬럼명
    ohe_columns = encoder.get_feature_names_out(categorical_features)
    # 학습용 데이터세트
    X_train_ohe = pd.DataFrame(X_train_ohe, columns=ohe_columns, index=X_train.index)
    
    # 테스트용 데이터세트
    X_test_ohe = pd.DataFrame(X_test_ohe, columns=ohe_columns, index=X_test.index)
    
    # ---------------------------
    # 학습용 데이터 최종본
    # ---------------------------
    # 학습용 데이터세트
    X_train_preprocessed = pd.concat([X_train_scaled, X_train_ohe], axis=1)
    # 테스트용 데이터세트
    X_test_preprocessed = pd.concat([X_test_scaled, X_test_ohe], axis=1)
    ```

- __Azure__
  - 데이터 선택  
    \- 독립변수 : season, holiday, workingday, weather, temp, humidity, windspeed, mnth, hour  
    \- 종속변수 : rentals  
 ![ ](/assets/img/posts/regression/azure_bike_rental_select_columns.png)

  - 결측치 처리  
    \- 결측치가 있는 행 제거  
 ![ ](/assets/img/posts/regression/azure_bike_rental_clean_missing_data.png)
    __<span style="color:#337ea9">결측치는 없으나 새로 들어올 데이터를 위해 진행!</span>__  

  - 스케일링 (ZScore)  
    \- 수치형 데이터 : temp, hum, windspeed  
    ![ ](/assets/img/posts/regression/azure_bike_rental_normalize_data.png)

    \- 스케일링 결과  
    ![ ](/assets/img/posts/regression/azure_bike_rental_normalize_results.png)

  - One-Hot 인코딩  
    \- 범주형 데이터 : mnth, season, holiday, weekday, workingday, weathersit  
    \- 숫자형 데이터를 범주형으로 변환  
    ![ ](/assets/img/posts/regression/azure_bike_rental_edit_metadata.png)

    \- 변환된 범주형 데이터 인코딩  
    ![ ](/assets/img/posts/regression/azure_bike_rental_convert_to_indicator_values.png)
    
    \- One-Hot 인코딩 결과     
    ![ ](/assets/img/posts/regression/azure_bike_rental_convert_to_indicator_values_results.png)

  - 학습/테스트용 데이터 분리  
    \- 데이터 비율 : 학습용 데이터(70%), 테스트용 데이터(30%)  
    ![ ](/assets/img/posts/regression/azure_bike_rental_split_data.png)

### 데이터 모델링
- __Python__  
  - 모델 생성  
    \- 알고리즘 : Ridge  
    ```python
    from sklearn.linear_model import Ridge
    model = Ridge()
    ```
  
  - 모델 학습  
  ```python
  model.fit(X_train_preprocessed, y_train)
  ```

- __Azure__
  - 모델 생성  
    \- 알고리즘 : Linear Regression  
    ![ ](/assets/img/posts/regression/azure_bike_rental_algorithm.png)
  
  - 모델 학습  
    \- 정답 레이블 (rentals) 지정    
    ![ ](/assets/img/posts/regression/azure_bike_rental_train_model.png)

### 모델 성능 평가
- __Python__  
  - 모델 테스트  
  ```python
    pred = model.predict(X_test_preprocessed)
  ```

  - 모델 평가  
  ```python
    print(f'모델 평가: {model.score(X_test_preprocessed, y_test)}')
    >> 모델 평가: 0.628224234782814
  ```
  
  - 평가지표 확인  
  ```python
    from sklearn.metrics import mean_squared_error, mean_absolute_error, root_mean_squared_error, r2_score
    print(f'R2: {r2_score(y_test, pred):.2f}')
    print(f'RMSE: {root_mean_squared_error(y_test, pred):.2f}')
    print(f'MAE: {mean_absolute_error(y_test, pred):.2f}')
    print(f'MSE: {mean_squared_error(y_test, pred):.2f}')  
    >> R2: 0.63
       RMSE: 106.63
       MAE: 78.02
       MSE: 11369.98
  ```

- __Azure__
  - 모델 테스트  
  ![ ](/assets/img/posts/regression/azure_bike_rental_score_model.png)  

  - 모델 평가  
  ![ ](/assets/img/posts/regression/azure_bike_rental_evaluate_model.png)  

  - 평가지표 확인  
  ![ ](/assets/img/posts/regression/azure_bike_rental_model_metrics.png)  