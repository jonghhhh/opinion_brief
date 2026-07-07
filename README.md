# 오늘의 여론 — 사설·여론 키워드 브리핑

주요 언론의 사설·논평과 온라인 커뮤니티 여론을 매일 아침 수집해, 유사 주제끼리 키워드로 묶어 보여주는 GitHub Pages 대시보드입니다. 각 키워드를 펼치면 언론(남색)과 여론(다홍)의 제목이 나란히 나오고, 제목을 클릭하면 원문으로 이동합니다. 데이터 생성은 Claude Code 루틴이 담당합니다.

## 저장소 구조

```
index.html            대시보드 (정적, 의존성 없음)
data/
  YYYY-MM-DD.json     일자별 브리핑 데이터
  latest.json         최신 브리핑 (해당 일자 파일과 동일 내용)
  index.json          날짜 목록 { "dates": ["최신", ..., "과거"] }  최대 90개
```

## 데이터 스키마 (요약)

```json
{
  "date": "YYYY-MM-DD",
  "generated_at": "ISO8601 (+09:00)",
  "headline": "한 줄 요약",
  "stats": { "editorial_count": 0, "opinion_count": 0, "keyword_count": 0 },
  "keywords": [
    {
      "keyword": "1~3단어",
      "rank": 1,
      "gist": "요지 1문장 (선택)",
      "items": [
        { "type": "editorial", "source": "매체명", "title": "사설 제목", "url": "https://..." },
        { "type": "opinion", "source": "플랫폼명", "title": "게시물 요지(재서술)", "url": "https://..." }
      ]
    }
  ],
  "etc": [ { "type": "...", "source": "...", "title": "...", "url": "" } ],
  "method_note": "수집 범위·한계 고지"
}
```

- `type`은 `editorial`(언론) / `opinion`(여론) 두 값만 사용합니다.
- `url`이 빈 문자열이면 대시보드는 링크 없는 일반 텍스트로 표시합니다.
- 대시보드는 이 키 이름들에 의존하므로 스키마 변경 시 index.html도 함께 수정해야 합니다.

## 설정 방법

1. **GitHub Pages 활성화**: Settings → Pages → Branch를 `main`(root)으로 설정.
2. **루틴 등록**: claude.ai/code/routines → New routine → 이 저장소 연결 → 루틴 프롬프트(`오늘의여론_루틴_프롬프트.md` 참조) 붙여넣기.
3. **트리거**: Scheduled → Daily → 07:00 → Asia/Seoul.
4. main 직접 푸시를 원하면 "Allow unrestricted branch pushes" 활성화. 아니면 루틴이 PR을 생성합니다.
5. 저장 후 **Run now**로 1회 시험 실행 → Pages URL에서 결과 확인. `data/` 안의 샘플 파일은 첫 실행 때 자동으로 교체·추가됩니다.

## 운영 메모

- 첫 실행 후 `method_note`를 확인해 fetch가 막힌 커뮤니티가 있으면 루틴 프롬프트의 진입점 목록에서 제외하세요.
- 배지 규칙: 키워드에 언론·여론 items가 모두 있으면 **언론+여론**, 한쪽만 있으면 **언론만** / **여론만**. 여론만 배지가 붙은 키워드가 곧 플랫폼 자생 의제입니다.
- 여론 게시물 제목은 원문 복사가 아니라 재서술된 요지입니다(대시보드 각주에도 고지됨).
