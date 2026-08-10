# Solution Definition — SOM 세그먼트를 위한 MVP 설계

> 07번 문서에서 확정한 SOM(수도권 × 고거래량 × High Workflow Fragmentation 개업공인중개사)을 대상으로, 1~6단계에서 검증한 Root Cause와 Pain을 뒤집지 않는 선에서 솔루션을 정의하는 단계다. 출발점은 "전자계약을 더 쓰게 하자"가 아니라 **거래 1건을 완료하는 데 발생하는 불필요한 운영 Touchpoint를 줄이자**다 — 국토교통부 전자계약시스템도 이미 계약작성·신고연계 기능을 제공하므로, 이를 단순 복제하는 신규 서비스는 차별성이 없다.

## Problem → Pain 재구조화

| Root Cause | 거래 단계 간 데이터·업무상태가 하나의 거래 단위로 이어지지 않아, 정보와 책임이 시스템·기관·커뮤니케이션 채널 사이에서 반복적으로 수동 이동한다 |
|---|---|

| Pain | 핵심 질문 |
|---|---|
| System Switching | 왜 같은 거래를 처리하기 위해 여러 화면을 계속 이동해야 하는가? |
| Manual Handoff | 왜 사람에게 다시 전화·문자·메신저로 전달해야 하는가? |
| Duplicate Entry | 왜 이미 입력한 정보를 다시 써야 하는가? |
| Follow-up Check | 왜 처리 여부를 확인하기 위해 다시 접속·연락해야 하는가? |
| Rework | 왜 누락·오류·상태불일치 때문에 다시 처리해야 하는가? |

## 솔루션 후보 7가지 비교

| Solution | 해결 Pain | 구현 난이도 | Pain 해결효과 | 구현 가능성 | 차별성 | 확장성 | 합계 |
|---|---|---|---:|---:|---:|---:|---:|
| **S1. Transaction Workspace** — 거래 단위 통합 업무공간 | Switching / Follow-up / Handoff | 낮음~중간 | 4.5 | 5 | 3.5 | 5 | **18.0** |
| **S2. Once-Only Data Entry** — 1회 입력 후 반복 재사용 | Duplicate Entry / Switching | 중간 | 4.5 | 4 | 4 | 4.5 | 17.0 |
| S3. Handoff Hub — 거래관계자 요청·전달 통합 | Manual Handoff | 중간 | 4 | 4 | 4 | 4 | 16.0 |
| S4. Follow-up & Exception Manager — 다음 확인사항 자동 관리 | Follow-up / Rework | 낮음~중간 | 4 | 5 | 3.5 | 4.5 | 17.0 |
| S5. Rework Guard — 거래 전 사전검증 | Rework | 중간 | 3.5 | 4.5 | 4 | 4 | 16.0 |
| S6. Integration Connector Layer — 외부시스템 Middleware | Switching + Entry + Follow-up 전체 | 매우 높음 | 5 | 2 | 5 | 5 | 17.0 |
| **S7. Transaction Workflow Copilot** — 위 요소를 묶는 Product Vision | 전체 | — | 5 | 3.5 | 4.5 | 5 | **18.0** |

> 점수는 시장에서 입증된 정량 점수가 아니라 프로젝트 내부 우선순위 설계 평가다. Pain 해결효과만 보면 S6(직접 시스템 통합)가 가장 강력하지만 MVP 단계 구현 가능성이 너무 낮고, 반대로 S1만 만들면 구현은 쉽지만 단순 CRM·Task Manager처럼 보일 위험이 있다.

## 최종 방향: Transaction Workflow Copilot (S1 + S2 + S4)

핵심 전제는 "AI가 아니라 **거래의 Context를 계속 유지하는 것**"이다. MVP는 S7 전체가 아니라 **S1(거래 단위 Workspace) + S2(정보 1회 입력) + S4(Follow-up 관리)** 세 개만 묶는다 — 외부 시스템의 완전한 API 통합 없이도 Pain 가설(A-H1)부터 검증할 수 있기 때문이다.

| 기존 Pain | MVP Solution |
|---|---|
| System Switching | Transaction Workspace |
| Duplicate Entry | Once-Only Data Entry |
| Manual Handoff | (초기에는 Task/Request 기록만) |
| Follow-up Check | Follow-up Manager |
| Rework | 상태·누락 표시 |

MVP에서 **의도적으로 제외**하는 것: 전자계약 자체 구현, 정부 신고 자동제출, 중개 플랫폼 API 전면 통합, 은행·보증기관 연동, 복잡한 AI Agent, 법률 자동판단, 전 거래유형 지원. 이런 기능부터 만들면 "문제가 진짜 있는지 검증하기 전에 Integration Project가 되어버리는 것"이 가장 큰 위험이다.

## 최종 Solution Definition 요약

| 항목 | 정의 |
|---|---|
| **Target Customer** | 수도권에서 주거 중개거래를 반복 수행하며 Workflow Fragmentation이 높은 개업공인중개사 (Market Segment Map 기준 중·고거래량 × High Fragmentation) |
| **Problem** | 거래 과정의 System Switching·Manual Handoff·Duplicate Entry·Follow-up Check로 거래당 행정시간과 Rework 증가 |
| **Value Proposition** | "중개거래를 시스템별로 관리하지 않고, 거래 단위로 관리한다" — 거래 정보를 한 번만 정리하고, 하나의 거래 흐름 안에서 다음 업무와 처리상태를 관리해 반복 입력·확인·전달 시간을 줄인다 |
| **Core Solution** | Transaction Workflow Copilot = Deal Workspace + Single Transaction Data + Workflow Timeline + Next Action + Exception/Missing Task |
| **MVP** | 거래 생성 → 핵심정보 1회 입력 → Checklist 자동 구성 → 현재 상태/다음 업무 표시 → 미완료 업무만 별도 노출 |
| **한 문장 정의** | "중개사가 하나의 주거 거래를 끝내기 위해 여러 시스템과 채널을 반복해서 오가는 대신, 거래 정보를 한 번 입력하고 계약부터 후속업무까지 하나의 Workflow에서 관리하는 중개거래 운영 플랫폼" |

핵심 차별화는 **전자계약 플랫폼도, 또 하나의 부동산 정보 플랫폼도 아니라는 것**이다. '거래정보 검색'이 아니라 **'거래 실행 과정에서 발생하는 운영 Fragmentation'** 자체를 제품의 문제로 삼는다는 점이 1~7단계 분석과 가장 일관된 방향이다.

## MVP 프로토타입: 「거래 1건 Workflow Board」

```mermaid
flowchart LR
    S1["거래 생성<br/>목동 OO아파트 매매<br/>계약일 8/15 · 잔금일 10/20"] --> S2["핵심 거래정보<br/>1회 입력"]
    S2 --> S3["거래 Checklist<br/>자동 생성"]
    S3 --> S4["현재 필요한 업무 표시<br/>🔴 거래신고 미완료<br/>🟠 잔금 확인 필요"]
    S4 --> S5["거래 완료"]
```

## Success Metric — Before/After로 A-H1 재검증

MVP의 가장 중요한 역할은 기능이 아니라 **Logging**이다. 이를 통해 5단계에서 Weak로 남았던 A-H1(Workflow Fragmentation ↑ → Active Processing Time ↑)을 실제 데이터로 재검증할 수 있다.

| 구분 | Metric |
|---|---|
| **Primary (North Star)** | Administrative Active Minutes / Transaction |
| Supporting | System Switching / Transaction, Duplicate Entries / Transaction, Manual Handoffs / Transaction, Follow-up Checks / Transaction, Rework Events / Transaction |

**검증 설계**: Market Segment Map의 중·고거래량 × High Fragmentation 중개사를 대상으로, 동일 중개사의 **기존 방식 거래**(일상적 처리 + 지표 기록) vs **MVP 사용 거래**(Workflow Copilot 사용 + 동일 지표 기록)를 비교하는 Time-and-Motion Before/After Test.

**Go/No-Go 기준** (산업 benchmark가 아니라 프로젝트용 제안 threshold):
- Primary Success: Active Processing Time / Transaction ≥20% 감소 또는 거래당 10분 이상 감소
- Secondary Success: System Switching·Duplicate Entry·Manual Handoff·Follow-up Check·Rework 중 최소 2개에서 의미 있는 감소

## 확장 로드맵

| Phase | 내용 |
|---|---|
| Phase 1 — Workflow Visibility | Deal Workspace, Task, Status (MVP) |
| Phase 2 — Data Reuse | Once-Only Entry 고도화, 문서 자동생성, 입력보조 |
| Phase 3 — Handoff Automation | 고객·직원 요청, 서류수집, 상태공유 (S3) |
| Phase 4 — Rework Prevention | Validation, Exception Detection (S5) |
| Phase 5 — System Integration | 공공데이터 조회 API 등 가능한 외부 Connector (S6) |

Phase 5로 확장할 때도 주의가 필요하다 — 공공 실거래 정보는 조회용 Open API로 제공되지만, 이를 **행정 업무의 외부 쓰기·제출 연동**과 같은 수준으로 가정하면 안 된다. 각 기능 확장은 이전 Phase에서 Active Processing Time 감소가 실제로 확인된 뒤에만 진행한다 — MVP에서 유의미한 감소가 없다면 거대한 API Integration(S6)을 만들 이유도 없다.
