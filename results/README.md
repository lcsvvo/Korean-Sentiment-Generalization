# Results

**실험별 점수·채택 여부·최종 평가 기록**.

| File | Content |
|---|---|
| [experiments_master.csv](experiments_master.csv) | 실험·기준 모델·최종 결과의 Validation·Public·Hidden 점수와 선택 사유 |
| [ensemble_3view_results.csv](ensemble_3view_results.csv) | 3-View 앙상블 가중 조합별 Validation MCC |

## Final Scores

| Evaluation | Model | Hidden MCC | Rank |
|---|---|---|---|
| Phase 1 | TF-IDF stacking | **0.7522** | **9 / 19** |
| Phase 2 | TF-IDF 기준 모델 | 0.665278 | — |
| Phase 2 | TF-IDF + e5 + KR-SBERT | **0.753970** | **3 / 18** |

- **동일 평가셋 비교:** Phase 2 기준 모델 대비 MCC **+0.088692**
- **평가셋 구분:** Phase 1·2 점수의 직접 차이를 개선폭으로 해석할 수 없음
- **빈 셀:** 해당 평가값이 기록되지 않은 항목
- 전체 로그·모델·임베딩 캐시는 공개 저장소에서 제외
