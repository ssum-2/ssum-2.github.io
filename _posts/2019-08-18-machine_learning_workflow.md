---
title: "머신러닝 워크플로우"
classes: wide
categories:
  - Study
tags:
  - AI
  - ML
author_profile : true
use_math : true
sidebar:
  - title: "Another Title"
    text: "More text here."
    nav: sidebar-sample
last_modified_at: 2019-08-18
---

## 머신러닝 워크플로우

------

![img](https://wikidocs.net/images/page/31947/%EB%A8%B8%EC%8B%A0_%EB%9F%AC%EB%8B%9D_%EC%9B%8C%ED%81%AC%ED%94%8C%EB%A1%9C%EC%9A%B0.PNG)

1. 수집(Acquisition)
   - 자연어 데이터; **corpus(코퍼스)**; 조사나 연구 목적에 의해서 특정 도메인으로부터 수집된 텍스트 집합
   - 텍스트데이터 파일; txt, csv, xml 등... 음성 데이터, 웹 수집기 등을 통해 수집된 데이터
2. 점검 및 탐색(Inspection and exploration)
   - 데이터의 구조, 노이즈 데이터, 머신러닝 적용을 위한 데이터 정제법 등 검토
   - **탐색적 데이터 분석(Exploratory Data Analysis, EDA) 단계**: 독립 변수, 종속 변수, 변수 유형, 변수의 데이터 타입 등을 점검하며 데이터의 특징과 내재하는 구조적 관계를 알아내는 과정을 의미
   - 시각화와 간단한 통계 테스트 진행
3. 전처리 및 정제(Preprocessing and Cleaning)
   - 자연어처리; 토큰화, 정제, 정규화, 불용어 제거 등
   - 까다로운 전처리의 경우 머신러닝 사용
4. 모델링 및 훈련(Modeling and Training)
   - 머신러닝 코드 작성단계
   - 적절한 머신러닝 알고리즘을 선택하여 모델링 -> 전처리된 데이터를 기계에 학습 -> 학습 후 원하는 Task를 수행
   - 데이터 중 일부는 테스트데이터로 남기고 훈련용데이터만 훈련에 사용 -> 성능측정 및 과적합(overfiting) 상황 방지
   - 데이터양이 충분한 경우 훈련용(학습지), 검증용(모의고사; 제대로 학습되었는지 여부를 판단, 모델 성능 개선용), 테스트용(수능시험; 모델의 최종성능 평가용, 성능 수치화) 데이터로 나눌 수 있음
   - ![img](https://wikidocs.net/images/page/31947/%EB%8D%B0%EC%9D%B4%ED%84%B0.PNG)
5. 평가(Evaluation)
   - 테스트용 데이터로 성능을 평가
   - 예측한 데이터와 테스트용 데이터의 실제 정답과 얼마나 가까운지 측정
6. 배포(Deployment)
   - 완성된 모델을 배포하는 단계
   - 모델을 변경해야할 경우 처음으로 돌아가야함