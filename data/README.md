# Data

**한국어 영화 리뷰 감성 분류**에 사용한 과제용 데이터 안내.

## Source

- **NSMC 계열** 한국어 영화 리뷰 데이터
- 원본 출처: [Naver Sentiment Movie Corpus](https://github.com/e9t/nsmc)

## Files

| File | Rows | Columns |
|---|---:|---|
| `public_train.csv` | **149,995** | `row_id`, `text`, `label` |
| `public_test.csv` | **49,997** | `row_id`, `text` |

- **label:** `POSITIVE` 또는 `NEGATIVE`
- Test 정답 라벨은 입력 파일에 미포함

## Paths

- 두 CSV를 같은 폴더에 배치하고 각 Notebook의 **`DATA_DIR`를 해당 폴더로 설정**
- Colab에서는 파일을 둔 Google Drive 경로로 변경
- 실행 순서는 [Notebooks](../notebooks/README.md) 참조

## Access

- 실행에 필요한 **과제용 CSV는 별도 확보 필요**
- 원자료는 재배포 제약으로 저장소에 미포함
