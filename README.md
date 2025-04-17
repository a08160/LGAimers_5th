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

- **불균형 처리: `SMOTE (Synthetic Minority Over-sampling Technique)`**
  - ✅ **사용 이유**:
    - 정상/불량 데이터의 클래스 불균형으로 인해 모델이 소수 클래스(불량)를 무시할 가능성 존재
    - 단순 복제 방식과 달리, **소수 클래스 주변 데이터를 기반으로 새로운 합성 데이터를 생성**
    - 모델이 소수 클래스의 패턴을 더 잘 학습할 수 있도록 도와 **F1 Score 향상**에 기여
  
### 3️⃣ 모델 학습

- **모델: `CatBoostClassifier`**
  - ✅ **선택 이유**:
    - 범주형 변수 처리에 특화된 Gradient Boosting 모델로, 별도의 인코딩 없이도 높은 예측 성능을 제공
    - 결측값 및 이상치에 비교적 강건하며, 학습 속도와 정확도 모두 우수
    - 트리 기반 알고리즘으로 공정 변수 간의 **비선형 관계**까지 효과적으로 학습 가능
    - **과적합 방지**, **범용성 높은 예측력**, **모델 해석 가능성**을 모두 만족시킬 수 있어 본 프로젝트에 적합

- **하이퍼파라미터 튜닝: `GridSearchCV`**
  - ✅ **사용 이유**:
    - `depth`, `learning_rate`, `iterations` 등의 주요 파라미터를 체계적으로 탐색하여 최적 조합을 찾기 위함
    - `cv=3` 교차 검증을 통해 일반화 성능을 높이고, 편향되지 않은 평가 가능
    - `f1_macro` 지표를 통해 불균형 클래스 간의 성능 균형을 고려

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
- ** 최종 Accuracy: 0.2710321 **
- ** 최종 F1 Score (macro): 0.45321**

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

