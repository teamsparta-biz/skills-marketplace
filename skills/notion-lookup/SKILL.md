---
name: notion-lookup
description: >
  Notion 강의 대시보드를 조회하고 Markdown으로 변환합니다.
  MCP 서버를 통해 lecture_search, lecture_detail, lecture_proposals 도구를 제공합니다.
metadata:
  author: yunseokchoi-teamsparta
  version: "2.0.0"
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
| `database` | 데이터베이스 이름 (기본: lecture_dashboard) |
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

### 자연어 → 상태 매핑 (lecture_state)

| 자연어 표현 | status 값 | 범위 |
|-------------|-----------|------|
| 완료, 종료, 끝난, 마무리 | `"lecture_finish,result_report,tax_invoice"` | ⑦⑧⑨ 전부 |
| 진행 중 | `"lecture_in_progress"` | ⑥만 |
| 준비 중, 진행 전, 교육 전 | `"manager_assignment,client_call,draft_workbook,final_workbook,lecture_setting"` | ①~⑤ 전부 |
| 교안 단계 | `"draft_workbook,final_workbook"` | ③④ |
| 매니저 배정, 담당자 배정 | `"manager_assignment"` | ①만 |
| 고객 연락, 영업 단계 | `"client_call"` | ②만 |
| 교안 초안, 워크북 작성 | `"draft_workbook"` | ③만 |
| 교안 완성, 최종 교안 | `"final_workbook"` | ④만 |
| 세팅 | `"lecture_setting"` | ⑤만 |
| 결과보고서, 결과 보고 | `"result_report"` | ⑧만 |
| 세금계산서, 정산 | `"tax_invoice"` | ⑨만 |
| 중단, 취소 | `"lecture_stop"` | 예외 |

### 자연어 → 타겟 매핑 (target_name)

| 자연어 표현 | target 값 |
|-------------|-----------|
| 실무자, 현업, 직원 | `"실무자"` |
| 신입, 신입사원 | `"신입사원"` |
| 임원, 직책자, 리더 | `"임원/직책자"` |
| 전사, 전직원, 전체 | `"전사"` |
| 고위 임원, C레벨, 사장 | `"고위급 임원"` |

### 자연어 → 직무 매핑 (job_name)

| 자연어 표현 | job 값 |
|-------------|--------|
| 사무직, 공통, 일반 | `"일반 사무직"` |
| 마케팅, 마케터, 홍보 | `"마케터"` |
| 엔지니어, 기술직 | `"엔지니어"` |
| 개발, 개발자 | `"개발"` |
| 연구, R&D, 연구직 | `"R&D/연구직"` |
| 인사, HR, 노무 | `"인사/노무(HR)"` |
| PM, PO, 기획 | `"PM/PO"` |
| 재무, 회계, 경리 | `"재무/회계"` |
| 디자인, 디자이너 | `"디자이너"` |

### 자연어 → 주제 매핑 (topic_name)

| 자연어 표현 | topic 값 |
|-------------|----------|
| AI 기초, 리터러시, AI 입문 | `"AI 리터러시 기초"` |
| 데이터, 데이터 분석, 분석 | `"데이터 분석"` |
| 이미지, 영상, 콘텐츠 제작 | `"이미지/영상 제작"` |
| 자동화, RPA, 업무 자동화 | `"업무 자동화"` |
| AI 심화, 고급 | `"AI 리터러시 심화"` |
| AI 리더십, 리더 | `"AI 리더십"` |
| 보안 | `"AI 보안"` |

### 자연어 → 툴 매핑 (using_tools_name)

| 자연어 표현 | tool 값 |
|-------------|---------|
| GPT, ChatGPT | `"ChatGPT"` |
| 엑셀, Excel, VBA | `"Excel VBA"` |
| 제미나이, Gemini | `"Gemini"` |
| 파이썬, Python | `"Python"` |
| 젠스파크, Genspark | `"Genspark"` |
| 소라, Sora | `"Sora"` |
| 런웨이, Runway | `"Runway"` |
| 미드저니, Midjourney | `"Midjourney"` |
| 클로드, Claude | `"Claude"` |
| 구글 워크스페이스 | `"google workspace"` |
| Copilot, 코파일럿 | `"Copilot"` |
| LangChain, 랭체인 | `"LangChain"` |
| N8N | `"N8N"` |
| Streamlit | `"Streamlit"` |
| Gradio | `"Gradio"` |
| 커서, Cursor | `"커서"` |

### 자연어 → 상품 매핑 (product)

| 자연어 표현 | product 값 |
|-------------|------------|
| 리터러시, AI Literacy | `"AI Literacy"` |
| PBL 라이트, 미니 PBL | `"PBL Lite"` |
| PBL 프로, PBL | `"PBL Pro"` (또는 `"PBL"`로 Lite+Pro 모두 매칭) |
| 세미나, 특강 | `"Seminar"` |
| 아카데미 해커톤 | `"Academy_해커톤"` |
| 아카데미 장기 | `"Academy_장기"` |
| 온라인 기존 | `"Online_기존"` |
| 온라인 제작 | `"Online_제작"` |

## 실행 후

1. 실행 결과를 사용자에게 보고하세요 (검색된 건수, 주요 필드 등)
2. 에러가 발생하면 에러 내용을 보고하세요
