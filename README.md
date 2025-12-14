# Bitcoin Trading Strategy Project
[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/jeongjoo1217-code/Deep-learning-GIT/blob/main/my_bitcoin_strategy.ipynb)


# 나만의 비트코인 딥러닝 트레이딩 모델 (Advanced Strategy)

## 📝 학생 정보
- **이름**: 김정주
- **학번**: 202100964

---

## 1. 모델 및 전략 설계 설명 (Model & Strategy Design)

### 1.1 모델 아키텍처
- **모델**: GRU (Gated Recurrent Unit)
- **구조**: 
  - Input Layer (Feature Size)
  - GRU Layer 1 (Hidden Size: 128, Dropout: 0.3)
  - BatchNorm & Dropout
  - GRU Layer 2 (Hidden Size: 64, Dropout: 0.3)
  - Fully Connected Layer (32 nodes, ReLU)
  - Output Layer (1 node, Sigmoid)

### 1.2 선택 이유
- **GRU 선택**: 금융 시계열 데이터는 노이즈가 많고 비선형적입니다. GRU는 LSTM보다 파라미터 수가 적어 학습 속도가 빠르고, 적은 데이터셋에서도 과적합 위험이 상대적으로 적어 효율적입니다.
- **BatchNorm & Dropout**: 딥러닝 모델의 일반화 성능을 높이고 과적합을 방지하기 위해 배치 정규화와 드롭아웃을 적극적으로 사용했습니다.

### 1.3 트레이딩 전략
- **진입(Buy)**: 모델이 예측한 상승 확률이 **일정 임계값(Threshold)** 이상일 때만 진입하여 승률을 높입니다. (시나리오 분석을 통해 최적 임계값 도출)
- **청산(Sell)**: 
  1. 상승 확률이 50% 미만으로 떨어지면 이익 실현 또는 손절.
  2. **손절매(Stop-Loss)**: 매수 가격 대비 **3% 이상 하락 시 즉시 매도**하여 큰 손실을 방지합니다.
- **포지션 사이징**: 상승 확률에 비례하여 투자 금액을 조절합니다 (확률이 높을수록 더 많이 투자).

### 1.4 차별점 (vs 예제)
- **데이터**: 볼린저 밴드, 스토캐스틱, 모멘텀 등 기술적 지표를 추가하여 피처를 강화했습니다.
- **모델**: 단순 LSTM에서 2-Layer GRU로 고도화하고 정규화 기법을 추가했습니다.
- **전략**: 단순 확률 매매에 '손절매' 로직을 추가하고, 다양한 임계값을 테스트하여 최적의 수익 시나리오를 도출합니다.

---

## 3. 데이터 로딩 및 피처 엔지니어링 강화 (Feature Engineering)

기존 피처(이동평균, RSI, MACD)에 더해 **볼린저 밴드**와 **스토캐스틱**을 추가하여 시장의 과매수/과매도 구간을 더 정밀하게 포착합니다.

## 4. GRU 모델 구현 (Model Implementation)

LSTM보다 구조가 간단하여 학습 속도가 빠르고, 금융 시계열 데이터에서 종종 더 나은 성능을 보이는 **GRU(Gated Recurrent Unit)** 모델을 사용합니다.

## 5. 트레이딩 전략 고도화 (Strategy Implementation)

단순 확률 비례 투자가 아닌, **안전 장치(Stop-Loss)**와 **확신 진입(Threshold)**을 결합한 전략입니다.

1.  **진입 조건**: 상승 확률이 `buy_threshold` (예: 0.6) 이상일 때만 매수
2.  **청산 조건**:
    *   하락 확률이 높을 때 (`prob < 0.5`)
    *   **손절매 (Stop-Loss)**: 매수 가격 대비 `stop_loss_pct` (예: -3%) 이상 하락 시 즉시 매도
3.  **포지션 사이징**: 확률에 따라 투자 비중 조절 (Kelly Criterion 개념 일부 차용)


## 6. 성과 분석 (Performance Analysis) - Updated 2025-12-14

**데이터 기간**: 2018-01-01 ~ 2025-12-14
**테스트 기간**: 2024-10-14 ~ 2025-12-13

### 6.1 벤치마크 vs 전략 수익률 비교

| 구분 | 수익률 | 설명 |
| :--- | :--- | :--- |
| **Buy and Hold** | **36.72%** | 기간 내 단순 보유 시 수익률 (최근 급등장 반영) |
| **My Strategy (GRU)** | **3.37%** | 제안된 딥러닝 트레이딩 전략 수익률 (Threshold 0.75) |
| **초과 수익 (Alpha)** | **-33.35%p** | 벤치마크 대비 성과 |

### 6.2 상세 분석
- **최적 임계값 (Threshold)**: 0.75 (예측 확률 75% 이상일 때만 매수)
- **총 거래 횟수**: 6회 (신중한 진입 전략)
- **최종 자본**: $10,337.19 (초기 자본 $10,000)

**고찰**:
최근 비트코인의 급격한 상승장(Buy and Hold 36% 수익)에서는 보수적인 전략(거래 횟수 6회)이 시장 수익률을 따라가지 못하는 경향을 보였습니다. 
하지만 하락장이나 횡보장에서는 손절매 로직을 통해 자산을 방어하는 데 유리할 수 있습니다. 
향후 상승장에서도 수익을 극대화하기 위해 진입 임계값을 낮추거나(0.6 등), 추세 추종 지표의 비중을 높이는 개선이 필요합니다.


