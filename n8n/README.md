# AI 도입 사례 주간 리서치 (n8n 워크플로 · Gemini)

`ai_case_weekly_research.json` 을 n8n 에 import 하면, 매주 월요일 08:47(KST)에 최근 7일간 공개된 AI 도입 사례를 Gemini(Google 검색 그라운딩 + URL context)로 리서치합니다. 결과는 구글 시트 DB에 쌓이고, HTML 리포트가 메일로 발송됩니다.

## 흐름

```
[매주 월 08:47] ─┐
[수동 실행]  ────┴─> Config ─> Read Existing DB(구글 시트) ─> Build Request
   ─> Gemini Research(Google 검색 + URL context) ─> Parse Results(사례별 item)
   ─> Verify Source Link(출처 URL 실제 접속 확인) ─> Build Report ─┬─> Split Rows ─> Append to DB(구글 시트)
                                                               └─> Send Weekly Report(Gmail)
```

| 요구사항 | 구현 위치 |
|---|---|
| 최근 7일 사례 | `Build Request` 가 KST 기준 기간을 계산해 프롬프트에 넣고, `Parse Results` 가 기간 밖 발표일을 한 번 더 걸러냄 |
| 표 항목(산업군·업무 영역·기업명·적용 내용·사용 기술·정량 성과·출처 링크·발표일) | 프롬프트의 JSON 스키마 → 시트 열 / 메일 표 |
| 실제 효과가 드러난 사례 우선 | 시스템 프롬프트 "수집 원칙 2". 기술 소개·MOU·파일럿 계획은 제외 |
| 출처 불분명·홍보성 기사 제외 | 시스템 프롬프트 "수집 원칙 3". 제외 목록은 메일 하단에 사유와 함께 표시 |
| 출처 링크 신뢰성 | `Verify Source Link` 가 실제로 접속해 봄. 404·접속 불가 링크(지어낸 URL)는 제외하고, 사이트가 자동 접속을 막은 경우(403 등)는 '확인 필요'로 표시 |
| 이전 수집분과 중복 제외 | ① 기존 시트 최근 300건을 프롬프트에 전달 ② 코드에서 출처 URL·(기업명+업무영역+발표일)로 재검사 |
| 수치는 원문만, 추정은 '추정' 표기 | 프롬프트 지시 + 코드에서 `metrics_basis=추정` 이면 "추정: " 접두어 강제 |
| 산업군별 정리, 주목 사례 3건, 신뢰도 상·중·하 | 리포트 상단에 주목 사례 3건, 이후 산업군별 표(신뢰도·발표일 순 정렬) |

## 설정 방법

1. **구글 시트 준비**
   - n8n 에 연결할 구글 계정으로 새 시트를 만들고, 탭 이름을 `AI_Cases` 로 바꿉니다.
   - A1 셀에 아래 한 줄을 붙여넣습니다(탭 구분이라 16칸에 나뉘어 들어갑니다).

   ```
   수집일	발표일	산업군	업무영역	기업명	국가	적용내용	사용기술	정량성과	성과근거	신뢰도	신뢰도근거	출처명	출처링크	링크확인	주목사례
   ```

2. **n8n 에 import**: Workflows → Import from File → `ai_case_weekly_research.json`.

3. **자격 증명(Credentials) 연결**
   - `Gemini Research (Google Search)` 노드: **Connect to Header Auth** → Name `x-goog-api-key`, Value 에 Gemini API 키를 넣습니다.
     키는 [Google AI Studio](https://aistudio.google.com/apikey) 에서 발급합니다. Google 검색 그라운딩은 결제(유료 등급)가 연결된 프로젝트에서 사용하는 것을 권장합니다.
   - `Read Existing DB`, `Append to DB`: Google Sheets OAuth2.
   - `Send Weekly Report`: Gmail OAuth2.

4. **`Config` 노드 값 수정**

   | 키 | 설명 | 기본값 |
   |---|---|---|
   | `recipient` | 리포트를 받을 이메일(쉼표로 여러 명) | `YOUR_EMAIL@example.com` |
   | `sheetId` | 시트 URL 에서 `/d/` 와 `/edit` 사이의 ID | `YOUR_GOOGLE_SHEET_ID` |
   | `sheetName` | 탭 이름 | `AI_Cases` |
   | `model` | Gemini 모델 ID | `gemini-3.1-pro-preview` |
   | `targetCases` | 주당 목표 사례 수 | `15` |
   | `regions` | 우선 지역 | 한국·미국·유럽·일본·중국 |
   | `focusIndustries` | 특히 보고 싶은 산업군(비우면 전 산업) | (빈 값) |
   | `maxOutputTokens` | 응답 최대 길이 | `32768` |

   모델 ID 는 자주 바뀝니다. AI Studio 모델 목록에서 현재 이름을 확인하세요. 더 빠르고 저렴하게 돌리려면 Flash 계열(예: `gemini-3.8-flash`)로 바꾸면 됩니다.

5. **`Manual Test Run` 으로 한 번 실행**해 시트와 메일을 확인한 뒤, 우측 상단 *Active* 를 켭니다.
   - 개별 노드만 Execute step 으로 돌리면 앞 단계 데이터가 없어 실패하니, 처음부터 실행하세요.

## 문제 해결

| 오류 | 원인 / 조치 |
|---|---|
| `400/403 API key not valid` | Header Auth 의 Name 이 `x-goog-api-key` 인지, 키 앞뒤에 공백이 없는지 확인 |
| `404 models/... is not found` | `Config` 의 `model` 이름이 바뀌었거나 단종됨 → AI Studio 에서 현재 모델 ID 로 교체 |
| `429 RESOURCE_EXHAUSTED` | 무료 한도 초과 → 결제 연결 또는 실행 간격 조정 |
| `<result_json> 블록을 찾지 못했습니다` | 모델이 형식을 어김 → 다시 실행하거나 Pro 모델 사용 |
| `maxOutputTokens 에서 잘렸습니다` | `targetCases` 를 줄이거나 `maxOutputTokens` 를 늘림 |
| `Read Existing DB` 에서 not found | 시트 ID·탭 이름 확인. 시트를 만든 계정과 n8n 자격 증명 계정이 같은지 확인 |

신뢰도·분류 기준, 제외 기준 같은 프롬프트는 `Build Request` 노드의 `system` 문자열에서 수정합니다.
