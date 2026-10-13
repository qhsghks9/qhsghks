# AI 도입 사례 주간 리서치 (n8n 워크플로)

`ai_case_weekly_research.json` 을 n8n 에 import 하면, 매주 월요일 08:47(KST)에 최근 7일간 공개된 AI 도입 사례를 Claude(웹 검색·원문 확인)로 리서치하고, 구글 시트 DB에 누적한 뒤 HTML 리포트를 메일로 보냅니다.

## 흐름

```
[매주 월 08:47] ─┐
[수동 실행]  ────┴─> Config ─> Read Existing DB(구글 시트) ─> Build Request
     ┌──────────────────────────────────────────────────────────┘
     └─> Request State ─> Claude Research(web_search + web_fetch) ─> Check Continuation ─> Needs Continuation?
              ▲                                                                           │ 예(pause_turn)
              └───────────────────────────────────────────────────────────────────────────┘
                                                                                          │ 아니오
                                     Parse & Build Report ─┬─> Split Rows ─> Append to DB(구글 시트)
                                                           └─> Send Weekly Report(Gmail)
```

| 요구사항 | 구현 위치 |
|---|---|
| 최근 7일 사례 | `Build Request` 가 KST 기준 기간을 계산해 프롬프트에 넣고, `Parse & Build Report` 가 기간 밖 발표일을 한 번 더 걸러냄 |
| 표 항목(산업군·업무 영역·기업명·적용 내용·사용 기술·정량 성과·출처 링크·발표일) | 프롬프트의 JSON 스키마 → 시트 열 / 메일 표 |
| 실제 효과가 드러난 사례 우선 | 시스템 프롬프트 "수집 원칙 2"; 기술 소개·MOU·파일럿 계획은 제외 |
| 출처 불분명·홍보성 기사 제외 | 시스템 프롬프트 "수집 원칙 3", 제외 목록은 메일 하단에 사유와 함께 표시 |
| 이전 수집분과 중복 제외 | ① 기존 시트 최근 300건을 프롬프트에 전달 ② 코드에서 출처 URL·(기업명+업무영역+발표일)로 재검사 |
| 수치는 원문만, 추정은 '추정' 표기 | 프롬프트 지시 + 코드에서 `metrics_basis=추정` 이면 "추정: " 접두어 강제 |
| 산업군별 정리, 주목 사례 3건, 신뢰도 상·중·하 | 리포트 상단에 주목 사례 3건, 이후 산업군별 표(신뢰도·발표일 순 정렬) |

## 설정 방법

1. **구글 시트 준비** — 새 시트를 만들고 탭 이름을 `AI_Cases` 로 바꾼 뒤, 1행에 아래 헤더를 그대로 입력합니다.

   ```
   수집일	발표일	산업군	업무영역	기업명	국가	적용내용	사용기술	정량성과	성과근거	신뢰도	신뢰도근거	출처명	출처링크	주목사례
   ```

   (기존에 수작업으로 모은 사례가 있다면 같은 열 형식으로 붙여 넣어 두면 중복 제외에 활용됩니다.)

2. **n8n 에 import** — Workflows → Import from File → `ai_case_weekly_research.json`.

3. **자격 증명(Credentials) 연결**
   - `Claude Research (web search)` 노드: *Header Auth* 자격 증명 생성 → Name `x-api-key`, Value 에 Anthropic API 키.
     Anthropic 콘솔(Settings → Privacy / Web search)에서 웹 검색이 허용되어 있어야 합니다.
   - `Read Existing DB`, `Append to DB`: Google Sheets OAuth2.
   - `Send Weekly Report`: Gmail OAuth2.

4. **`Config` 노드 값 수정**

   | 키 | 설명 | 기본값 |
   |---|---|---|
   | `recipient` | 리포트 받을 이메일(쉼표로 여러 명) | `YOUR_EMAIL@example.com` |
   | `sheetId` | 시트 URL `/d/` 와 `/edit` 사이의 ID | `YOUR_GOOGLE_SHEET_ID` |
   | `sheetName` | 탭 이름 | `AI_Cases` |
   | `model` | Claude 모델 | `claude-opus-5-5` |
   | `effort` | 추론 강도(`medium`이면 더 빠르고 저렴, `xhigh`면 더 꼼꼼) | `high` |
   | `targetCases` | 주당 목표 사례 수 | `15` |
   | `regions` | 우선 지역 | 한국·미국·유럽·일본·중국 |
   | `focusIndustries` | 특히 보고 싶은 산업군(비우면 전 산업) | (빈 값) |
   | `maxSearches` / `maxFetches` | 1회 실행당 웹 검색 / 원문 열람 최대 횟수 | `25` / `15` |
   | `maxContinuations` | 서버측 검색 루프가 멈췄을 때(pause_turn) 이어서 요청하는 최대 횟수 | `6` |

5. **`Manual Test Run` 으로 한 번 실행**해 메일과 시트를 확인한 뒤, 우측 상단 *Active* 를 켭니다. 스케줄 시각은 `Weekly Schedule` 노드에서 바꿀 수 있습니다(워크플로 타임존은 `Asia/Seoul`).

## 참고

- 1회 실행은 보통 수 분 걸립니다(HTTP 타임아웃 10분, 실패 시 5초 간격 3회 재시도).
- 비용은 실행당 모델 토큰 + 웹 검색 사용량이 부과됩니다. 줄이려면 `effort` 를 `medium`, `maxSearches`/`maxFetches` 를 낮추세요.
- 요청에 `fallbacks: "default"` 가 켜져 있어, 드물게 안전 분류기가 요청을 거절하면 서버가 다른 모델로 자동 재시도합니다. 끄려면 `Build Request` 코드에서 `fallbacks` 줄과 HTTP 노드의 `anthropic-beta` 헤더를 지우세요.
- 신뢰도·분류 기준, 제외 기준 등 프롬프트는 `Build Request` 노드의 `system` 문자열에서 수정합니다.
