# 자연어 매핑 테이블

SKILL.md의 자연어 해석 규칙에서 참조하는 매핑 테이블입니다.

## 자연어 → 상태 매핑 (lecture_state)

| 자연어 표현 | CLI 값 | 범위 |
|-------------|--------|------|
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

## 자연어 → 타겟 매핑 (target_name)

| 자연어 표현 | CLI 값 |
|-------------|--------|
| 실무자, 현업, 직원 | `"실무자"` |
| 신입, 신입사원 | `"신입사원"` |
| 임원, 직책자, 리더 | `"임원/직책자"` |
| 전사, 전직원, 전체 | `"전사"` |
| 고위 임원, C레벨, 사장 | `"고위급 임원"` |

## 자연어 → 직무 매핑 (job_name)

| 자연어 표현 | CLI 값 |
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

## 자연어 → 주제 매핑 (topic_name)

| 자연어 표현 | CLI 값 |
|-------------|--------|
| AI 기초, 리터러시, AI 입문 | `"AI 리터러시 기초"` |
| 데이터, 데이터 분석, 분석 | `"데이터 분석"` |
| 이미지, 영상, 콘텐츠 제작 | `"이미지/영상 제작"` |
| 자동화, RPA, 업무 자동화 | `"업무 자동화"` |
| AI 심화, 고급 | `"AI 리터러시 심화"` |
| AI 리더십, 리더 | `"AI 리더십"` |
| 보안 | `"AI 보안"` |

## 자연어 → 툴 매핑 (using_tools_name)

| 자연어 표현 | CLI 값 |
|-------------|--------|
| GPT, ChatGPT | `"ChatGPT"` |
| 엑셀, Excel, VBA | `"Excel VBA"` |
| 제미나이, Gemini | `"Gemini"` |
| 파이썬, Python | `"Python"` |
| 젠스파크, Genspark | `"Genspark"` |
| GPT API | `"GPT API"` |
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

## 자연어 → 상품 매핑 (product)

| 자연어 표현 | CLI 값 |
|-------------|--------|
| 리터러시, AI Literacy | `"AI Literacy"` |
| PBL 라이트, 미니 PBL | `"PBL Lite"` |
| PBL 프로, PBL | `"PBL Pro"` (또는 `"PBL"`로 Lite+Pro 모두 매칭) |
| 세미나, 특강 | `"Seminar"` |
| 아카데미 해커톤 | `"Academy_해커톤"` |
| 아카데미 장기 | `"Academy_장기"` |
| 온라인 기존 | `"Online_기존"` |
| 온라인 제작 | `"Online_제작"` |

## 예시

- "LG 완료된 교육 목록" → `--search "LG" --status "lecture_finish,result_report,tax_invoice" --list-only`
- "이다은 담당 진행 중인 교육" → `--manager "이다은" --status "lecture_in_progress" --list-only`
- "준비 중인 교육 보여줘" → `--status "manager_assignment,client_call,draft_workbook,final_workbook,lecture_setting" --list-only`
- "신입사원 대상 Python 교육" → `--target "신입사원" --tool "Python" --list-only`
- "마케터 대상 데이터 분석 교육" → `--job "마케터" --topic "데이터 분석" --list-only`
- "PBL 상품 목록" → `--product "PBL" --list-only`
- "코스맥스 교육 MD로 변환해줘" → `--search "코스맥스"`
