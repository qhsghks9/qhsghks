# 액센츄어의 BPO 기반 FDE 운영 모델 — 사례 기반 정리

> 작성일: 2026-11-09
> 근거 표시: **[공개]**는 공식 발표·보도로 확인한 내용, **[해석]**은 공개 정보를 바탕으로 정리한 구조 분석, **[가상 적용]**은 테마파크 AX에 맞춰 설계한 예시입니다.

---

## 1. 한 줄 요약

액센츄어는 **"운영(BPO/Managed Services)을 계약으로 맡고, 그 운영 현장 안에 FDE(Forward Deployed Engineer, 액센츄어 명칭 'Reinvention Deployed Engineer')를 상주시켜 AI 에이전트를 실제 업무에 넣고, 개선 성과를 운영 KPI로 회수하는"** 모델로 움직이고 있습니다.
기존 BPO가 "사람으로 프로세스를 대신 수행"했다면, 이 모델은 "프로세스를 수행하면서 동시에 에이전트로 재설계"합니다.

---

## 2. 개념 정리

| 구분 | 전통 BPO | 전통 SI/컨설팅 | **BPO + FDE 모델** |
|---|---|---|---|
| 계약 단위 | FTE·건수 | 프로젝트(기간·산출물) | **운영 성과(SLA·비용·처리시간)** |
| 현장 접근 | 운영 인력만 상주 | 구축 기간 한정 | **운영 인력 + 엔지니어 상시 상주** |
| AI 적용 | 부분 자동화(RPA) | PoC 후 이관 | **운영 데이터로 에이전트를 만들고 바로 운영에 투입** |
| 개선 동기 | 낮음(FTE 감소 = 매출 감소) | 종료 후 소멸 | **성과 공유형 계약으로 개선 동기 내재화** |
| 가장 큰 리스크 | 정체 | PoC의 늪 | 계약 구조·데이터 접근·조직 저항 |

**FDE란?** [공개] 원래 Palantir가 만든 역할로, 고객사 현장에 들어가 실제 데이터·시스템·업무 담당자와 붙어서 제품을 "작동하게" 만드는 엔지니어입니다. 액센츄어는 이를 **"Reinvention Deployed Engineer"**라고 부르며, Claude 등 AI를 고객 환경 안에 내재화하는 역할로 정의했습니다.

---

## 3. 공개 사례

### 사례 ① Anthropic 파트너십 — "Accenture Anthropic Business Group" (2025.12) [공개]
- 액센츄어 인력 **약 3만 명을 Claude로 교육**, 그 안에 FDE("reinvention deployed engineers")를 포함해 **고객 환경 안에 Claude를 내재화**하는 역할을 부여.
- Claude Code 기반 **CIO 대상 공동 오퍼링**, Claude CoE, 이노베이션 허브 구성.
- 초기 타깃: **금융, 생명과학, 헬스케어, 공공** 등 규제 산업.
- 시사점 [해석]: 규제 산업은 BPO 비중이 높은 영역(콜센터, 청구·심사, 백오피스)이라, 운영 계약 위에 FDE를 얹는 구조와 잘 맞습니다.

### 사례 ② Google Cloud — "Accenture Gemini Enterprise Business Group" (2026.09) [공개]
- **FDE 1,000명 확보 목표**를 내걸고 Gemini Enterprise 기반 에이전트 도입을 지원.
- 시사점 [해석]: FDE를 "인원수"로 공시한다는 것은 FDE가 일회성 프로젝트 인력이 아니라 **상시 운영 가능한 역량 풀**이 되었다는 의미입니다.

### 사례 ③ ServiceNow — FDE 공동 프로그램 (2026.05) [공개]
- ServiceNow 엔지니어 + 액센츄어 산업 전문가가 **고객의 운영 시스템 안에서** 에이전틱 워크플로를 공동 설계·구축.
- 목표로 운영 속도 향상, 비용 절감, 고객 경험 개선을 명시. 이후 ServiceNow 기반 **매니지드 보안 서비스**도 공동 출시.
- 시사점 [해석]: IT 운영(ITSM/보안 관제)은 액센츄어 매니지드 서비스의 핵심이라, **"플랫폼 벤더 FDE + 액센츄어 운영 조직"** 결합 구조의 대표 사례입니다.

### 사례 ④ Workday — HR·재무 FDE 프로그램 (2026.10) [공개]
- 고객의 **HR·재무 팀 옆에 엔지니어를 붙여** Workday 에이전트를 실제 프로덕션에 올리는 프로그램.
- 시사점 [해석]: HR·재무는 액센츄어 BPO의 전통 영역(급여, AP/AR, 결산)이므로 **BPO 운영 + FDE 모델에 가장 가까운 사례**입니다.

### 사례 ⑤ SAP — FDE 프로그램 (2026.06) [공개]
- SAP 플랫폼 위 AI 유스케이스를 발굴·구현하며, **도입 이후의 지속 운영(ongoing operations)**까지 범위에 포함.

### 사례 ⑥ AI 내재 매니지드 서비스 고객 [공개, 상세 수치 미공개]
- 실적 발표 관련 보도에서 **Bath & Body Works**(에이전틱 AI를 운영 전반에 넣은 통합 매니지드 서비스), **BT Group, Cox Communications, Banco Bradesco**를 AI 기반 매니지드 서비스 고객으로 언급.
- 운영 플랫폼 측면에서는 **SynOps**(사람·봇·AI에 업무를 배분하는 운영 엔진)에 **AI Refinery**를 넣어 "자율 운영"을 지향한다고 밝힘.
- ⚠️ 이 고객사들의 구체적 개선 수치는 공개 자료에서 확인되지 않았습니다. 대외 자료에 인용할 때는 수치를 만들어 넣지 마세요.

---

## 4. 운영 모델 구조 [해석]

```
┌──────────────────────── 고객사 ────────────────────────┐
│  업무 오너(현업)   ←→   운영 거버넌스(주간/월간 SteerCo)  │
└───────────────▲────────────────────────▲──────────────┘
                │ KPI·SLA 보고            │ 개선 승인
┌───────────────┴──── 액센츄어 운영 Pod ───┴─────────────┐
│ ① BPO 운영팀      : 실제 업무 처리, 예외 처리, 품질 관리   │
│ ② FDE 셀(2~5명)   : 에이전트 개발·배포, 시스템 연동        │
│ ③ 프로세스/산업 전문가 : 업무 재설계, 규정·리스크 검토     │
│ ④ 플랫폼 벤더 FDE  : Anthropic/Google/ServiceNow/SAP 등   │
└────────────────────────────────────────────────────────┘
          ▲ 공통 자산: SynOps(업무 배분), AI Refinery, 산업별 에이전트 라이브러리
```

### 운영 루프 (분기 단위 반복)
1. **관찰** — BPO 운영팀이 처리 로그·예외·병목 데이터를 축적 (FDE가 바로 접근 가능)
2. **선정** — 건수 × 처리시간 × 규칙성으로 에이전트화 후보 우선순위 결정
3. **구축** — FDE가 2~6주 단위로 에이전트 개발, 운영팀이 "사람 검토(HITL)" 단계로 병행 운영
4. **전환** — 정확도·SLA 기준 충족 시 자동 처리 비중 확대, 인력은 예외 처리·고난도 업무로 재배치
5. **회수** — 절감분을 계약 구조에 따라 고객과 공유 → 다음 개선 재원으로 재투자

### 계약 구조 포인트
- **FTE 기반 → 성과 기반 전환**: 처리 건당 단가, 또는 기준 비용 대비 절감분 공유(Gain-share)
- **FDE 비용 포지셔닝**: 별도 프로젝트비가 아니라 운영 계약에 내장 (고객 입장에선 "PoC 비용" 부담 감소)
- **자산 귀속**: 에이전트/프롬프트/워크플로의 IP 귀속과 계약 종료 시 이관 조건을 사전 명시

---

## 5. 왜 이 모델이 작동하는가 [해석]

| 요인 | 설명 |
|---|---|
| 데이터 접근 | BPO가 이미 실제 업무 데이터를 처리하고 있어 FDE의 "현장 진입 비용"이 거의 없음 |
| 즉시 검증 | 만든 에이전트를 다음 날 운영에 투입해 효과를 KPI로 바로 확인 |
| PoC 함정 회피 | 구축과 운영이 같은 조직이라 "이관 실패"가 구조적으로 사라짐 |
| 카니발리제이션 해소 | 성과형 계약으로 바꾸면 자동화가 매출 잠식이 아니라 마진 개선이 됨 |
| 벤더 레버리지 | Anthropic·Google·ServiceNow·SAP·Workday FDE를 동시에 끌어와 플랫폼 중립적 조합 가능 |

### 한계·리스크
- **FDE 인력 희소성**: 공시 목표(1,000명 등) 대비 실제 숙련 인력 확보 속도가 병목
- **고객 종속 우려**: 운영과 AI 자산을 한 벤더가 쥐면 전환 비용이 커짐 → 고객은 자산 이관 조항 요구
- **성과 측정 분쟁**: 기준선(Baseline) 정의가 모호하면 Gain-share 정산에서 갈등
- **규제·보안**: 금융·헬스케어는 에이전트 판단 근거 기록과 사람 승인 단계가 필수

---

## 6. 테마파크 적용 예시 [가상 적용]

> 아래는 공개 사례가 아니라, 본 저장소의 테마파크 AX 과제 맥락에 맞춰 위 모델을 적용해 본 **가상 설계안**입니다. 수치는 목표 예시이며 실측값이 아닙니다.

### 대상 운영 영역
| 운영 영역 (BPO 범위) | FDE가 만드는 에이전트 | 사람이 남는 역할 |
|---|---|---|
| 고객센터(전화·채팅·VOC) | 예약/환불/분실물 문의 자동 응대, VOC 자동 분류·요약 | 클레임·감정적 응대, 예외 승인 |
| 단체·제휴 예약 백오피스 | 견적·계약서 초안 생성, 정산 대사 자동화 | 가격 협상, 계약 최종 승인 |
| 시설·어트랙션 운영 지원 | 점검일지·고장 이력 분석, 정비 우선순위 추천 | 안전 판단, 현장 조치 |
| 마케팅 운영 | 시즌 프로모션 콘텐츠 초안, 성과 리포트 자동화 | 브랜드 검수, 전략 결정 |

### 6주 단위 1사이클 예시
| 주차 | 활동 | 산출물 |
|---|---|---|
| 1 | 운영 로그·VOC 데이터 분석, 후보 업무 선정 | 에이전트화 우선순위 맵 |
| 2~3 | FDE가 1순위 에이전트 개발, 운영팀과 시나리오 테스트 | 동작하는 에이전트 v0.1 |
| 4~5 | HITL 병행 운영(에이전트 초안 → 상담원 승인) | 정확도·처리시간 비교 데이터 |
| 6 | 자동 처리 범위 확대 결정, 다음 후보 착수 | SteerCo 보고, 다음 사이클 계획 |

### KPI 예시
- 문의 1건당 평균 처리시간(AHT), 1차 해결률(FCR)
- 에이전트 자동 처리 비중, 사람 수정률(에이전트 초안을 고친 비율)
- 성수기 피크 대응 시 추가 인력 투입 규모
- 고객 만족도(CSAT), VOC 처리 리드타임

---

## 7. 시사점 — 국내 기업이 참고할 점

1. **"AI 프로젝트"가 아니라 "AI가 들어간 운영 계약"으로 사라** — 구축 후 이관 실패 리스크가 사라짐
2. **FDE는 소수 정예로 운영 현장에 상주** — 본사 TF가 아니라 운영 데이터 옆에 둘 것
3. **기준선과 성과 공유 방식을 계약 전에 확정** — 측정 분쟁이 이 모델의 가장 흔한 실패 원인
4. **자산 귀속·이관 조항으로 종속 리스크 관리** — 에이전트, 프롬프트, 평가 데이터셋 포함
5. **HITL 단계를 정식 운영 단계로 설계** — 안전·고객 접점 업무는 사람 승인 단계를 남길 것

---

## 출처
- [Anthropic — Anthropic & Accenture partnership](https://www.anthropic.com/news/anthropic-accenture-partnership)
- [Accenture Newsroom — Accenture and Anthropic Launch Multi-Year Partnership (2025.12.09)](https://newsroom.accenture.com/news/2025/accenture-and-anthropic-launch-multi-year-partnership-to-drive-enterprise-ai-innovation-and-value-across-industries)
- [CIO Dive — Accenture, Google Cloud Gemini Enterprise, forward deployed engineers](https://www.ciodive.com/news/accenture-google-cloud-gemini-enterprise-forward-deployed-engineers/829988/)
- [Pulse 2.0 — ServiceNow and Accenture FDE Program](https://pulse2.com/servicenow-and-accenture-forward-deployed-engineering-program-launched-to-scale-agentic-ai/amp/)
- [Analytics India Magazine — Accenture, Workday FDE programme](https://analyticsindiamag.com/ai-news/accenture-workday-launch-forward-deployed-engineering-programme-for-ai-agents)
- [Accenture Newsroom — FDE program with SAP (2026.06.08)](https://newsroom.accenture.com/blogs/2026/accenture-launches-forward-deployed-engineering-program-with-sap)
- [National CIO Review — Accenture bets on AI partners (managed services clients)](https://nationalcioreview.com/articles-insights/cio-field-notes/accenture-bets-on-ai-partners-and-ot-security-as-enterprise-demand-builds/)
- [RTInsights — Accenture launches SynOps](https://www.rtinsights.com/accenture-synops/)
