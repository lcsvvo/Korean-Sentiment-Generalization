# Notebooks

**Phase 1, 2 분석 코드**와 실행 안내.

| Notebook | Content | Environment |
|---|---|---|
| [Phase 1](phase1_sklearn_stacking.ipynb) | TF-IDF, Complement NB, LogReg stacking | Google Colab 기준, CPU 실행 가능 |
| [Phase 2](phase2_embedding_stacking.ipynb) | TF-IDF, 문장 임베딩, 5-fold stacking | GPU 권장, CPU에서는 임베딩 생성이 느림 |

## Setup

- **Python 3.11** 기준. 저장소 루트에서 의존성 설치

```bash
pip install -r requirements.txt
```

- [Data 안내](../data/README.md)에 맞는 `public_train.csv`, `public_test.csv` 확보
- 두 Notebook은 **각각 실행 가능**. Phase 1 실행 결과가 Phase 2의 필수 입력은 아님
- `SEED=42` 사용. Notebook 내 셀은 위에서 아래로 실행

## Paths

- **Phase 1:** Colab Drive 마운트 후 `DATA_DIR`를 실제 CSV 폴더로 변경. 로컬 실행 시 Drive 마운트 셀 생략과 경로 변경 필요
- **Phase 2:** `DATA_DIR`와 `CACHE_DIR`를 실제 입력, 캐시 폴더로 변경. 로컬 기본 입력 경로는 현재 작업 폴더
- 캐시와 모델, 제출 파일은 로컬에 저장. 공개 결과표는 [Results](../results/README.md) 참조

## Evaluation

- Phase 1은 **80:20 분할**, Phase 2 기본 모델은 **5-fold OOF 예측** 사용
- Phase 2 메타 모델과 threshold는 같은 메타 학습 표본을 사용하므로 해당 점수는 독립된 메타 검증 점수가 아님
- Hidden 점수는 외부 평가 기록. 정답 라벨이 없어 공개 파일만으로 동일 점수 재계산 불가

