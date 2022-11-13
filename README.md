## Problem Description

서울시에서 2017년 1월 12일부터 2018년 12월 30일까지 1시간 단위로 자전거 대여 정보가 주어졌을 때, 마지막 28일 데이터를 테스트 데이터로 사용해서 시계열 데이터를 예측하는 프로젝트입니다.

This is the project that predicts the last 28 days data using bicycle rental information given every hour from January 12, 2017 to December 30, 2018.

## Dataset


https://www.kaggle.com/datasets/saurabhshahane/seoul-bike-sharing-demand-prediction

Kaggle에 있는 서울 자전거 데이터셋을 했습니다.

The dataset was brought from the Seoul bike dataset in Kaggle.

## Method

- LightGBM을 Baseline으로 두고, Recursive Feature Elimination을 통해 필요한 feature를 선택했습니다.
- GridSearchCV를 통해 하이퍼 파라미터를 튜닝했습니다.
