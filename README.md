# 제 2의 신약개발 경진대회를 위한 IC50 예측 모델 (IC50 Prediction Model for New Drug Development)

이 프로젝트는 **제 2의 신약개발 경진대회**에서 **SMILES** 데이터를 기반으로 **IC50 예측**을 위한 신경망 모델을 구현하고, 성능을 평가하는 데 중점을 두었습니다. **약한 결합기 피처**와 **강한 결합기 피처**를 활용한 두 가지 신경망 모델을 결합한 **앙상블 모델**을 통해 과적합을 방지하고, 더 나은 예측 성능을 얻었습니다..

---





## 목차 (Table of Contents)
1. [프로젝트 개요 (Project Overview)](#프로젝트-개요-project-overview)
2. [EDA 분석 결과 (EDA Results)](#eda-분석-결과-eda-results)
3. [신경망 아키텍처 설명 (Neural Network Architecture Explanation)](#신경망-아키텍처-설명-neural-network-architecture-explanation)
   - [모델 1: 약한 결합기 피처 기반](#모델-1-약한-결합기-피처-기반)
   - [모델 2: 강한 결합기 피처 기반](#모델-2-강한-결합기-피처-기반)
4. [모델 성능 비교 (Model Performance Comparison)](#모델-성능-비교-model-performance-comparison)
5. [참고 논문 (Reference Papers)](#참고-논문-reference-papers)

---

## 프로젝트 개요 (Project Overview)

- 이 프로젝트는 **제 2의 신약개발 경진대회**를 위해 **IC50 예측 모델**을 구축하는 것이 목표였습니다.
- **약한 결합기 피처**와 **강한 결합기 피처**를 각각 사용하여 두 가지 신경망 모델을 학습시킨 후, **앙상블 모델**을 통해 성능을 향상시켰습니다.
- 피처 선택을 통해 **과적합을 방지**하고, 모델의 일반화 성능을 높였습니다.
- **데이콘(Dacon) 대회에서 상위 10%** 성과를 달성하였습니다.

---

## EDA 분석 결과 (EDA Results)

EDA(탐색적 데이터 분석)를 통해 **데이터 불균형** 문제를 확인하였습니다.

1. **pIC50 값의 분포**: pIC50 값이 불균형하게 분포되어 있으며, 특정 값에 데이터가 집중된 현상이 나타났습니다.
   - 예를 들어, **pIC50 = 8.52**인 데이터가 40개로 가장 많이 존재하며, 그 외에도 특정 값에 데이터가 집중되어 있습니다.
2. **데이터 불균형 문제 해결**:
   - **약한 결합기 피처**와 **강한 결합기 피처**를 각각 활용하여 두 가지 모델을 학습한 후, **앙상블 모델**을 사용함으로써 데이터 불균형 문제를 극복할 수 있었습니다.
   - 이 두 모델은 서로 다른 피처를 사용하여 데이터를 보완하고, 앙상블을 통해 더 나은 성능을 보여주었습니다.
  
<img src="https://github.com/user-attachments/assets/37f5947b-1b10-46e7-9499-7f83a37dfef7" alt="Distribution of pIC50 Values" width="300" height="200">

---


## 신경망 아키텍처 설명 (Neural Network Architecture Explanation)

### 모델 1: 약한 결합기 피처 기반
- **약한 결합기 피처**는 결합 강도가 상대적으로 낮은 피처들로, **LogP**, **AromaticProportion**, **NumAliphaticRings**, **BalabanJ**, 그리고 원소 빈도(**C, F, H, N, S, P**)를 사용하였습니다.
- 이 피처들은 화합물의 **약한 결합 특성**을 설명하며, 모델 1에서 사용된 간단한 신경망 아키텍처는 이러한 피처들을 바탕으로 학습하였습니다.
- **활성화 함수**: 
  - 모델 1에서는 **ReLU**(Rectified Linear Unit) 활성화 함수를 주로 사용했습니다. ReLU는 음수 입력을 0으로 만들고, 양수 입력은 그대로 출력하는 간단한 함수로, 신경망 학습의 효율성을 높이는 데 중요한 역할을 합니다.
  - **일부 층에서는 Swish 활성화 함수**가 사용되었습니다. Swish는 ReLU보다 더 복잡한 패턴을 학습할 수 있으며, 일부 층에서만 사용함으로써 모델의 계산 효율성을 높이면서도 더 정교한 패턴을 학습할 수 있습니다.

<img src="https://github.com/user-attachments/assets/93836bde-095c-43a7-bbaa-2355f3d4a1dd" alt="Model 1 Structure" width="200" height="400">

### 모델 2: 강한 결합기 피처 기반
- **강한 결합기 피처**는 **TPSA**, **NumRotatableBonds**, **NumAromaticRings**, **MolWt**, **MolWt_HBDonors**, **MolWt_HBAcceptors**, **SASA** 등과 같은 물리적 특성으로, 화합물의 **강한 결합 특성**을 설명합니다.
- 모델 2는 더 복잡한 신경망 구조를 가지며, **배치 정규화(Batch Normalization)**를 포함하여 학습의 안정성을 높였습니다.
- **활성화 함수**:
  - 모델 2에서도 주로 **ReLU** 활성화 함수가 사용되었습니다. ReLU는 연산이 간단하고, 특히 깊은 신경망에서 **기울기 소실 문제**를 줄이는 데 효과적입니다.
  - **일부 층에서 Swish** 활성화 함수가 사용되었습니다. Swish는 음수 영역을 포함하여 더 복잡한 패턴을 학습할 수 있도록 돕습니다. Swish를 일부 층에서만 사용함으로써 계산 효율성과 복잡한 비선형성 학습 사이의 균형을 맞췄습니다.

<img src="https://github.com/user-attachments/assets/4da271fe-42a4-4add-8585-9480f8747c31" alt="Model 2 Structure" width="200" height="700">

### 신경망 아키텍처 이미지 설명
- 아래그림은 신경망의 전체적인 구조를 나타냅니다. **입력층**, **은닉층**, 그리고 **출력층**으로 구성되며, 각 노드 간 연결을 통해 데이터가 흐르고 학습이 이루어집니다.
- **ReLU 활성화 함수**는 주로 입력 데이터가 흐르는 초기 은닉층에서 사용되어 **계산 효율성**을 높이고, **Swish 활성화 함수**는 더 깊은 층에서 사용되어 **복잡한 패턴을 학습**하는 데 도움이 됩니다.
- **은닉층의 다양성**을 통해 모델은 다양한 피처들의 상호작용을 학습하며, 결과적으로 최종 출력층에서 **IC50** 예측을 위한 값을 산출하게 됩니다.

<img src="https://github.com/user-attachments/assets/75e76720-36c4-401e-8b0b-9ec537f05d5e" alt="Neural Network Architecture" width="400" height="400">



---

## 모델 성능 비교 (Model Performance Comparison)

두 모델의 성능을 비교한 결과는 다음과 같습니다. **약한 결합기 피처**와 **강한 결합기 피처**를 각각 사용한 모델과 이들의 앙상블 성능을 비교하였습니다.

| Model     | RMSE        | Normalized RMSE (A) | Correct Ratio (B) | Final Score |
|-----------|-------------|---------------------|-------------------|-------------|
| Model 1   | 1638.146    | 2.976               | 0.578             | 0.289       |
| Model 2   | 1802.333    | 3.274               | 0.581             | 0.290       |
| Ensemble  | 1595.605    | 2.899               | 0.642             | 0.321       |

- **모델 1 (약한 결합기 피처 기반)**: 간단한 신경망 구조에도 불구하고, 약한 결합 성질을 적절히 반영하여 중간 성능을 보였습니다.
- **모델 2 (강한 결합기 피처 기반)**: 더 복잡한 구조와 강한 결합 피처를 사용하여 조금 더 나은 Correct Ratio를 보였지만, RMSE는 모델 1보다 조금 높았습니다.
- **앙상블 모델**: 두 모델을 결합하여 가장 높은 성능을 보였습니다. 이는 약한 결합기 피처와 강한 결합기 피처를 함께 사용할 때 모델 성능이 향상될 수 있음을 보여줍니다.

**성과**: 이 모델들은 데이콘(Dacon) 대회에서 상위 10%에 도달하는 성과를 냈습니다.

---

## 참고 논문 (Reference Papers)

- **Backpropagation Applied to Handwritten Zip Code Recognition** - Yann LeCun et al. (1989)
   - 역전파 알고리즘을 활용한 신경망 학습에 대한 설명.
   - [논문 링크 (Paper Link)](http://yann.lecun.com/exdb/publis/pdf/lecun-89e.pdf)

- **Rectified Linear Units Improve Restricted Boltzmann Machines** - Vinod Nair, Geoffrey E. Hinton (2010)
   - ReLU 활성화 함수의 사용과 그 효과에 대한 논문.
   - [논문 링크 (Paper Link)](https://www.cs.toronto.edu/~hinton/absps/reluICML.pdf)

- **Batch Normalization: Accelerating Deep Network Training by Reducing Internal Covariate Shift** - Sergey Ioffe, Christian Szegedy (2015)
   - 배치 정규화(Batch Normalization)의 개념과 효과를 설명한 논문.
   - [논문 링크 (Paper Link)](https://arxiv.org/pdf/1502.03167.pdf)

- **The Elements of Statistical Learning: Data Mining, Inference, and Prediction** - Trevor Hastie, Robert Tibshirani, Jerome Friedman (2009)
   - 피처 선택과 과적합 방지에 관한 중요한 내용이 포함된 책.
   - [논문 링크 (Paper Link)](https://web.stanford.edu/~hastie/ElemStatLearn/)

---

## 라이센스 (License)
이 프로젝트는 MIT 라이센스에 따라 배포됩니다.

This project is licensed under the MIT License.
