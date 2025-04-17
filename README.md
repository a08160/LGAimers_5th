# LGAimers_5th
LG AIMERS 5기 온라인 해커톤 | 자동차 디스플레이 제품 이상 여부 판별 프로젝트

## 프로젝트 소개

본 프로젝트는 LG Aimers 해커톤에서 제공된 **자동차 디스플레이 생산 라인(Sub Assembly Line)** 데이터를 기반으로, 불량 여부를 예측하는 AI 모델을 개발하기 위한 것입니다. 주어진 데이터는 자동차 디스플레이 제조 공정에서 발생할 수 있는 다양한 불량 유형(기포, Misalignment, Crack 등)을 포함하고 있으며, 이를 활용해 불량 예측 모델을 구축하고 시각화 및 최적화된 성능 평가를 수행합니다.

---

## 프로젝트 목표

- Sub Assembly Line 데이터 기반 **불량 여부 예측 모델 개발**
- **데이터 전처리, 이상치 제거, 정규화, 특성 선택** 등 통합적인 ETL 파이프라인 설계
- **SMOTE를 활용한 불균형 데이터 처리**
- **CatBoostClassifier + GridSearchCV** 기반 모델 튜닝 및 성능 최적화
- **Feature Importance 시각화 및 데이터 통찰 확보**
- 향후 Total Assembly Line 범위로 확장 가능한 모델 구조 고려

---

## 데이터 설명

### 공정 개요

- **Dam + Fill 공정**: 레진 도포 및 반경화
- **합착 공정**: 글라스와의 접합
- **탈포 공정**: 남아있는 기포 제거

### 주요 Feature 목록 (일부 예시)

- 레진 도포 좌표, 속도, 시간, UV 경화 시간 및 위치 등
- Glass 이동 속도, 합착 GAP, 공정 온도 및 습도 등
- 탈포 압력 및 시간, 챔버 온습도, 공정 소요 시간 등

각 Feature는 단일 공정뿐 아니라 전체 조립 공정의 품질과도 밀접하게 연관되므로, 불량 원인 추정 및 사전 방지에도 중요한 역할을 합니다.

---

## 전처리 및 모델링 파이프라인

1. **데이터 클렌징 및 통합**
   - 결측값("OK") → NaN 처리
   - 중복 컬럼 제거 및 상관관계 기반 컬럼 축소
   - 수치형 변환, 이상치 제거(Z-score), 정규화(StandardScaler)

2. **불균형 처리**
   - SMOTE 기법을 통해 minority class 샘플 보강

3. **모델 학습 및 튜닝**
   - `CatBoostClassifier` 기반 예측 모델 학습
   - `GridSearchCV`를 통해 최적 하이퍼파라미터 탐색 (`depth`, `learning_rate`, `iterations`)

4. **시각화 및 해석**
   - 결측값 히트맵(`missing_values_heatmap.png`)
   - 상관관계 히트맵(`correlation_heatmap.png`)
   - Feature Importance 시각화(`feature_importance_optimized.png`)

5. **예측 및 제출**
   - 테스트 데이터셋에 대해 예측 수행 후 제출 파일 생성(`submission-optimized.csv`)

---

## 성능 지표

- **Accuracy** 및 **F1 Score (macro)** 기준으로 평가
- 불량이 다중 원인에 의해 발생할 수 있으므로 **모델의 일반화 성능과 해석 가능성**이 중요함

---

## 향후 확장성

- Component 단위에서 Total Assembly Line 전체로 확대 적용 가능
- 예측 대상 범위를 1순위 불량뿐 아니라 2, 3순위까지 확장
- 다중 클래스 분류 혹은 멀티레이블 예측 구조로 전환 가능

---

## 프로젝트 구조

