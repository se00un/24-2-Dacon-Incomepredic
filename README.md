# 24-2-Dacon-Incomepredic

자세한 분석 과정은 블로그에 정리되어 있습니다. 
  <br/> 👉 자세한 분석 내용: https://blog.naver.com/develop0420/224081858523



  <br/> 인구 통계 데이터를 기반으로 개인의 소득이 $50K 이하인지 초과인지 분류하는 머신러닝 프로젝트입니다.
Competition
DACON – Income Classification
https://dacon.io/competitions/official/235892/overview/description


📅 진행 기간
2024.10.01 – 2024.10.05


---

### 데이터

Census 기반 인구 데이터로 구성되어 있으며 주요 변수는 다음과 같습니다.

- age : 나이  
- workclass : 직업 유형  
- education / education.num : 교육 수준  
- marital.status : 결혼 상태  
- occupation : 직업  
- race / sex : 인종, 성별  
- capital.gain / capital.loss : 자본 이익 및 손실  
- hours.per.week : 주당 근무 시간  
- native.country : 국적  
- target : 소득 (≤50K / >50K)

---

### EDA 및 데이터 전처리

이번 프로젝트에서는 **EDA의 정당성을 확보하는 것**을 주요 목표로 진행했습니다.  
각 전처리 과정은 가설을 세우고 검증하는 방식으로 진행되었습니다.

#### 1. 범주 재정리
- `marital.status`, `relationship` 변수의 의미가 겹치는 문제 확인
- Census 자료를 참고하여 카테고리를 재구성

#### 2. 교육 수준 재범주화
- `education.num`별 고소득 비율 분석
- 유사 패턴을 보이는 구간을 묶어 재범주화
- 재범주화 이후 **target과의 상관계수 증가 확인**

#### 3. 결측치 처리

결측치 존재 변수

- workclass  
- occupation  
- native.country  

처리 방법

- **IterativeImputer(MICE 기반 다중대치법)** 적용
- 변수 간 상관관계를 고려하여 결측치를 순차적으로 대치
- `native.country`는 여러 방법을 시도했으나 의미 있는 패턴이 없어 **Unknown 처리**

#### 4. 이상치 처리
- 수치형 변수는 정규성을 따르지 않음을 확인  
- Z-score 대신 **Isolation Forest 기반 이상치 탐지** 적용
- 이후 IQR 기준 이상치를 중앙값으로 대체

#### 5. 다중공선성 확인
- VIF 분석 결과 **유의미한 다중공선성 없음**

---

### 모델링

#### 모델 후보
- LightGBM  
- XGBoost  
- Gradient Boosting  

#### 모델 선택
PyCaret을 활용한 AutoML 기반 모델 비교

#### 하이퍼파라미터 튜닝
- RandomizedSearchCV 활용

#### 앙상블
- **Stacking Ensemble 적용**

---

### 결과

최종 모델 성능

**Accuracy : 0.8332**



