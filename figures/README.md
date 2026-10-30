# Figures

**분석 결과와 모델 구성 그림**.

| Figure | Content |
|---|---|
| [Public vs. Hidden](fig1_public_hidden_shakeup.png) | Phase 1의 Public, Hidden MCC와 순위 |
| [Sparse vs. Dense](fig3_sparse_vs_dense.png) | Phase 2 동일 Hidden 평가셋의 모델 비교 |
| [Public Score Changes](fig2_lb_evolution.png) | Phase 1 모델 구성별 Public MCC |
| [Validation vs. Public](fig5_generalization_gap.png) | 실험별 Validation, Public 점수 차이 |
| [Model Pipeline](fig4_pipeline.png) | Phase 1 stacking 구성 |
| [Threshold Curve](fig6_threshold_curve.png) | Phase 2 메타 학습 표본의 threshold별 MCC |
| [Phase Summary](fig0_improvement_journey.png) | 서로 다른 평가셋에서 얻은 Phase별 결과 요약 |
| [Social Preview](social_preview.png) | 저장소 미리보기 이미지 |

## Notes

- **점수 출처:** [실험 기록](../results/experiments_master.csv)
- **Phase 구분:** Phase 1, 2 순위는 참가자 수와 평가셋이 달라 직접 개선 비교 불가
- **점수 차이:** Validation, Public 차이만으로 과적합을 확정할 수 없음
- **Threshold:** 메타 모델 학습 표본에서 계산한 값이며 독립된 검증 성능은 아님

