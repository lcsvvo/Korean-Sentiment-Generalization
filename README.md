# Korean Sentiment Generalization

## Overview

한국어 영화 리뷰 감성 분류에서 **Public·Hidden 평가 성능의 차이**와 **문장 임베딩 결합 효과**를 비교한 개인 프로젝트.

- **Phase 1:** TF-IDF 기반 stacking 구성과 Public·Hidden 성능 비교
- **Phase 2:** 문장 임베딩과 5-fold OOF 예측을 추가한 확장 실험

![Python](https://img.shields.io/badge/Python-3776AB?style=flat&logo=python&logoColor=white) ![scikit-learn](https://img.shields.io/badge/scikit--learn-F7931E?style=flat&logo=scikitlearn&logoColor=white) ![Sentence Transformers](https://img.shields.io/badge/Sentence_Transformers-4051B5?style=flat) ![PyTorch](https://img.shields.io/badge/PyTorch-EE4C2C?style=flat&logo=pytorch&logoColor=white)

## Data

| Item | Details |
|---|---|
| Source | NSMC 계열 한국어 영화 리뷰 과제 데이터 |
| Train | **149,995건**, 긍정·부정 비율 약 49.9%·50.1% |
| Test | **49,997건**, 정답 라벨 미공개 |
| Task | 긍정·부정 이진 분류 |
| Metric | **MCC** — 예측과 실제 라벨의 일치도를 평가하는 상관계수 |

원자료는 저장소에 미포함. 과제용 파일 구성과 접근 조건은 [Data 안내](data/README.md) 참조.

## Research Design

- **평가 성능:** Public 점수와 순위가 Hidden 평가에서도 유지되는지 비교
- **표현 방식:** TF-IDF와 문장 임베딩을 결합한 모델을 동일 Hidden 평가셋의 기준 모델과 비교

```mermaid
flowchart TD
    Q["한국어 영화 리뷰 감성 분류"]
    Q --> P1["Phase 1 · TF-IDF stacking"]
    Q --> P2["Phase 2 · TF-IDF + 문장 임베딩"]
    P1 --> S["형태소·문자·감성 표현<br/>LogReg + Complement NB"]
    P2 --> D["Word·Char TF-IDF<br/>multilingual-e5 + KR-SBERT"]
    S --> M1["LogReg 메타 모델"]
    D --> M2["5-fold OOF 예측<br/>LogReg 메타 모델"]
    M1 --> R1["Public·Hidden 성능 차이"]
    M2 --> R2["Phase 2 동일 Hidden 평가셋<br/>기준 모델 대비 성능 차이"]
    classDef model fill:#ede9fe,stroke:#8b5cf6,color:#111827;
    class P1,P2,S,D,M1,M2 model;
```

OOF는 각 표본을 학습에 사용하지 않은 fold 모델의 예측값. Phase 2에서는 이 예측값을 메타 모델의 학습 입력으로 사용.

## Analysis Workflow

```mermaid
flowchart TD
    A["리뷰 데이터 로드"] --> B["텍스트 정제·특성 생성"]
    B --> C["Phase 1<br/>80:20 분할·기본 모델 학습"]
    B --> D["Phase 2<br/>임베딩 생성·5-fold 기본 모델 학습"]
    C --> E["Holdout 예측 + 메타 특성"]
    D --> F["OOF 예측 + 메타 특성"]
    E --> G["메타 모델 학습"]
    F --> H["메타 모델 학습·threshold 탐색"]
    G --> I["전체 학습 데이터로 기본 모델 재학습"]
    H --> I
    I --> J["Test 예측·제출"]
    J --> K["평가 결과 기록·비교"]
    classDef step fill:#ede9fe,stroke:#8b5cf6,color:#111827;
    class B,C,D,E,F,G,H,I step;
```

실행 환경·경로 설정·Notebook 안내는 [Notebooks](notebooks/README.md) 참조.

## Key Results

### Public vs. Hidden

- Phase 1 제출 모델의 **Public MCC 0.757·5위 → Hidden MCC 0.7522·9위**로 변화
- Public·Hidden 간 성능과 순위 차이 관찰. 단일 제출 결과이므로 다른 데이터에 대한 일반화 수준까지 판단하기는 어려움

<p align="center">
  <img src="figures/fig1_public_hidden_shakeup.png" width="820" alt="Phase 1 Public MCC 0.757과 Hidden MCC 0.7522 비교">
</p>
<p align="center"><sub>Phase 1의 서로 다른 평가 구간에서 측정한 MCC와 순위.</sub></p>

### Sparse vs. Dense

- Phase 2의 **동일 Hidden 평가셋**에서 TF-IDF 기준 모델 **0.665278 → 임베딩 결합 모델 0.753970**, MCC **+0.088692**
- 확장 모델의 최종 순위 **3 / 18**. 임베딩 추가와 검증 방식 변경을 함께 적용한 결과

<p align="center">
  <img src="figures/fig3_sparse_vs_dense.png" width="820" alt="Phase 2 동일 Hidden 평가셋에서 TF-IDF 기준 모델과 임베딩 결합 모델의 MCC 비교">
</p>
<p align="center"><sub>Phase 2의 TF-IDF 기준 모델과 확장 모델 비교. Phase 1 최종 점수 0.7522와는 평가셋이 다름.</sub></p>

| Evaluation | Model | Hidden MCC | Rank |
|---|---|---|---|
| Phase 1 | TF-IDF stacking | **0.7522** | **9 / 19** |
| Phase 2 | TF-IDF 기준 모델 | 0.665278 | — |
| Phase 2 | TF-IDF + e5 + KR-SBERT stacking | **0.753970** | **3 / 18** |

전체 실험 기록은 [Results](results/README.md) 참조.

## Limitations

- **평가셋 차이:** Phase 1·2 점수와 순위를 직접 비교해 개선폭으로 해석할 수 없음
- **비교 범위:** Phase 2는 표현·검증 방식을 함께 변경했으므로 임베딩만의 효과를 분리하지 못함
- **검증 범위:** 메타 모델 학습·threshold 탐색에 같은 메타 학습 표본 사용. 독립된 메타 OOF 평가는 미수행
- **불확실성:** 점수 차이의 신뢰구간·통계적 유의성 평가 미수행

## Repository Structure

```text
├── notebooks/   # Phase 1·2 분석 코드와 실행 안내
├── results/     # 실험 기록과 결과표
├── figures/     # 결과 그림
├── docs/        # 방법론과 상세 실험 문서
└── data/        # 데이터 배치 안내
```

[Notebooks](notebooks/README.md) · [Results](results/README.md) · [Figures](figures/README.md) · [Docs](docs/README.md)
