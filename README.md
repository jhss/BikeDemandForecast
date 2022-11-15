## Problem Description

서울시에서 2017년 1월 12일부터 2018년 12월 30일까지 1시간 단위로 자전거 대여 정보가 주어졌을 때, 마지막 28일 데이터를 테스트 데이터로 사용해서 시계열 데이터를 예측하는데, Regression 모델과 ARIMA 모델을 비교하는 프로젝트입니다.

This is the project that compares regression and ARIMA model when predicting the last 28 days data using bicycle rental information given every hour from January 12, 2017 to December 30, 2018.

## Dataset


https://www.kaggle.com/datasets/saurabhshahane/seoul-bike-sharing-demand-prediction

Kaggle에 있는 서울 자전거 데이터셋을 했습니다.

The dataset was brought from the Seoul bike dataset in Kaggle.

## Method

전처리 후에 8,764개 데이터가 남았습니다. 마지막 28일 데이터를 테스트 데이터셋 (28일 * 24시간 = 672개), 나머지 데이터를 학습 데이터셋으로 삼았습니다.

### (1) Regression
- LightGBM을 Baseline으로 두고, Recursive Feature Elimination을 통해 필요한 feature를 선택했습니다.
- GridSearchCV를 통해 하이퍼 파라미터를 튜닝했습니다.


### (2) ARIMA

- ADF Test를 통해 데이터가 stationary 성질을 갖고있는지 확인했고, 그 조건을 만족시키기 위해 log scale과 differencing technique을 사용했습니다.
- [pmdarima](https://github.com/alkaline-ml/pmdarima) 에서 auto_arima를 사용해서 하이퍼파라미터를 튜닝하였고, 그렇게 얻은 최적의 하이퍼파라미터를 statsmodels의 ARIMA에 적용하여서 모델을 훈련시켰습니다. 
- 훈련시킬 때 feature는 exogeneous variable으로 활용하였고, 자전거 수요 추이를 학습을 했습니다.


## Conclusion

- 마지막 28일 데이터를 각 모델로 예측했을 때 결과입니다.

### (1) Regression

![regression](./img/regression.PNG)
ㄴ
**R2-Score: 70.03%**

### (2) Seasonal ARIMA

![arima](./img/ARIMA.PNG)

**R2-Score: 63.53%**

|   | LightGBM  | SARIMA  | 
|---|---|---|
|R2 score   | 70.03  | 63.53  |  
|Train + Inference Time   | 1031 (s)  | 1642 (s)  | 
|   |   |   |  

전체적으로 두 모델이 학습 데이터를 비슷하게 예측하는 경향이 있다. 하지만 Seasonal ARIMA에서 하이퍼파라미터를 탐색할 때 서치 시간이 많이 소모된다. (탐색할 하이퍼파라미터가 8,000개 정도 되는데, 다 탐색하지 못하고 중간에 RAM 용량 부족으로 인해 멈추었다. Colab RAM 용량이 작아서 세션이 계속 다운돼서 GCP에서 32GB RAM 사용을 했는데 GCP에서도 중간에 프로그램이 Killed 되었다.) 그래서 그때까지 탐색한 하이퍼파라미터 중에 AIC 값이 가장 작은 모델을 선택했다.

R2 score값은 많이 차이가 나지 않지만, 학습할 때 시간에 차이가 있기 때문에 효율적인 측면에서 Regression 모델을 사용하는게 좋다고 생각한다. 좀 더 정확한 모델을 원한다면 충분한 RAM을 확보하고 시간을 들여서 SARIMA 모델의 하이퍼파라미터를 찾는게 좋아보인다.