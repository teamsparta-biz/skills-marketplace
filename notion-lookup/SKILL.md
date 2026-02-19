---
description: Notion 강의 대시보드를 조회하고 Markdown으로 변환합니다.
argument-hint: [옵션]
author: yunseokchoi-teamsparta
version: 2.0.0
tags:
  - data
  - notion
---

## 실행 방법

이 스킬은 MCP 서버(`notion-lecture`)로 제공됩니다. Bash 실행이 아닌 **MCP 도구 호출**로 사용합니다.

### 사용 가능한 도구

| 도구 | 설명 |
|------|------|
| `lecture_search` | 강의 목록 검색 (필터 조합 가능) |
| `lecture_detail` | 강의 상세 조회 (속성 + 본문 마크다운 + 제안서 파일) |
| `lecture_proposals` | 제안서 PDF 텍스트 추출 |

### lecture_search

강의 목록을 검색합니다. 모든 파라미터는 선택입니다.

| 파라미터 | 설명 |
|----------|------|
| `search` | 제목 키워드 |
| `status` | 강의 상태 (쉼표 구분 복수 가능) |
| `manager` | 강의 담당자(lecture_PIC) 이름 |
| `sales` | 영업 담당자(sales_PIC) 이름 |
| `target` | 교육 타겟 (실무자, 신입사원, 임원 등) |
| `job` | 직무 (마케터, 개발, 엔지니어 등) |
| `tool` | 사용 툴 (ChatGPT, Python, Excel 등) |
| `topic` | 주제 (데이터 분석, 업무 자동화 등) |
| `product` | 상품명 (AI Literacy, PBL 등) |
| `limit` | 반환 최대 건수 (기본 50) |

### lecture_detail

`lecture_search` 결과의 `id`를 입력하면 속성, 본문 마크다운, 제안서 파일 목록을 반환합니다.

| 파라미터 | 설명 |
|----------|------|
| `page_id` | 강의 페이지 ID (필수) |

### lecture_proposals

강의의 제안서 PDF에서 텍스트를 인메모리로 추출합니다.

| 파라미터 | 설명 |
|----------|------|
| `page_id` | 강의 페이지 ID (필수) |

## 자연어 해석 규칙

사용자가 자연어로 요청하면 아래 규칙에 따라 적절한 MCP 도구를 호출하세요.

**의도 판별:**
- "목록", "조회", "보여줘", "몇 개" → `lecture_search` 호출
- "상세", "내용", "본문", "마크다운" → `lecture_search`로 ID 확인 후 `lecture_detail` 호출
- "제안서", "proposal", "PDF" → `lecture_search`로 ID 확인 후 `lecture_proposals` 호출
- 의도가 불분명하면 `lecture_search`로 목록을 먼저 보여주세요

**키워드 매핑:**
- 회사명/교육명 언급 → `search` 파라미터
- 사람 이름 + "담당" → `manager` 파라미터
- 사람 이름 + "영업" → `sales` 파라미터
- 상태 관련 표현 → `status` 파라미터 (워크플로우 매핑 참조)
- 대상 언급 → `target`, 직무 → `job`, 툴 → `tool`, 주제 → `topic`, 상품 → `product`

### 상태값 워크플로우 (lecture_state)

```
① manager_assignment  매니저 배정
② client_call         고객사 컨택
③ draft_workbook      교안 초안
④ final_workbook      최종 교안
⑤ lecture_setting     교육 세팅
⑥ lecture_in_progress 교육 진행 중
⑦ lecture_finish      교육 종료
⑧ result_report       결과보고서
⑨ tax_invoice         세금계산서
※ lecture_stop        교육 중단 (예외)
```

### 자연어 매핑 테이블

상세 매핑은 [reference.md](reference.md)를 참조하세요.

## 실행 후

1. 실행 결과를 사용자에게 보고하세요 (검색된 건수, 주요 필드 등)
2. 에러가 발생하면 에러 내용을 보고하세요
