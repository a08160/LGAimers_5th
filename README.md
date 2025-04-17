# LG AIMERS 5th: 자동차 디스플레이 제품 이상 여부 판별

> LG AIMERS 해커톤 - Sub Assembly Line 데이터 기반 불량 예측 AI 모델 개발 프로젝트

---

## 📌 프로젝트 개요

자동차 디스플레이 생산 라인(Sub Assembly Line)에서 수집된 공정 데이터를 기반으로, **제품의 불량 여부를 예측하는 머신러닝 모델**을 개발합니다.  
기포, Misalignment, Crack 등 다양한 불량 유형을 학습하여, 생산 품질을 사전에 예측/판별하는 것이 목표입니다.

---

## 🎯 프로젝트 목표

- 불량 여부를 예측하는 AI 모델 설계 및 최적화
- 데이터 전처리, 이상치 제거, 정규화 등 **통합적인 ETL 파이프라인 구축**
- **SMOTE** 기반 불균형 데이터 보정
- **CatBoostClassifier + GridSearchCV** 기반 모델 튜닝
- Feature Importance 시각화를 통한 인사이트 확보
- 향후 **Total Assembly Line 확장**을 고려한 범용적인 모델 구조 설계

---

## 🏭 데이터 및 공정 설명

### 📋 공정 요약

| 공정명     | 설명                           |
|------------|--------------------------------|
| Dam + Fill | 레진 도포 및 반경화 단계       |
| 합착 공정  | 글라스와 접합                  |
| 탈포 공정  | 기포 제거 및 챔버 안정화       |

### 🧾 주요 Feature 예시

- 레진 도포 좌표, UV 경화 시간
- 합착 GAP, Glass 이동 속도
- 챔버 온습도, 탈포 압력/시간 등

---

## 🔧 전처리 및 모델링 파이프라인

### 1️⃣ 데이터 전처리

- `OK` 값 → `NaN` 처리
- 단일값 컬럼 및 중복 컬럼 제거
- 수치형 컬럼 형변환 및 정규화 (`StandardScaler`)
- Z-score 기반 이상치 제거
- 상관관계가 높은 컬럼(0.8 이상) 제거

### 2️⃣ 불균형 처리

- `SMOTE`를 활용한 minority class 샘플 보강

### 3️⃣ 모델 학습

- 모델: `CatBoostClassifier`
- 튜닝: `GridSearchCV`를 사용해 `depth`, `learning_rate`, `iterations` 최적화

### 4️⃣ 시각화 및 분석

| 시각화 항목             | 파일명                              |
|-------------------------|--------------------------------------|
| 결측치 히트맵           | `missing_values_heatmap.png`         |
| 상관관계 히트맵         | `correlation_heatmap.png`            |
| Feature Importance       | `feature_importance_optimized.png`   |

### 5️⃣ 예측 및 제출

- 테스트 데이터셋 예측 후 `submission-optimized.csv` 저장

---

## 📈 성능 평가 지표

- **Accuracy**, **F1 Score (macro)**
- 다중 원인 불량 발생 가능성을 고려하여 **모델 해석력 및 일반화 성능**이 중요

---

## 🔮 향후 확장 가능성

- Sub Assembly에서 Total Assembly Line 전체로 확장 가능
- 예측 대상 확대 (1순위 불량 외 다중 불량 예측)
- 다중 클래스 또는 멀티레이블 분류로의 구조 전환 고려

---

## 🧠 주요 기술 스택

- Python (pandas, numpy, seaborn, matplotlib)
- Scikit-learn
- Imbalanced-learn (SMOTE)
- CatBoost
- GridSearchCV

