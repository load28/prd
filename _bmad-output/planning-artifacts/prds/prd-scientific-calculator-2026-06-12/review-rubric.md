# PRD Quality Review — 공학용 계산기 (Scientific Calculator)

> **Calibration note:** 학습/취미용 저(低)stakes 학생용 웹 계산기. 간결함이 핵심 가치인 제품이므로, 7개 차원을 동일하게 엄격히 보지 않고 저stakes·간결 지향에 맞춰 calibrate했다. boilerplate 추가를 요구하는 지적은 배제했고, 오히려 간결함을 해치는 과형식화도 점검 대상에 포함했다.

## Overall verdict
저stakes 학생용 도구로서 이 PRD는 매우 견고하다. "간결함"이라는 단일 thesis가 Vision → Features → Non-Goals → Success Metrics까지 일관되게 흐르고, 제품명("공학용")과 실제 MVP 범위("기본 사칙연산") 사이의 긴장을 `[NOTE FOR PM]`로 정직하게 노출했다. FR마다 testable consequence가 붙어 있어 done-ness도 명확하다. 주요 리스크는 두 가지뿐이다: (1) 제품명과 MVP 범위의 불일치가 다운스트림에서 혼란을 줄 수 있고, (2) FR-1이 "괄호" 입력을 포함하지만 잘못된 괄호 짝 외 입력 검증 경계가 다소 모호하다. 둘 다 저stakes 맥락에서 치명적이지 않다.

## Decision-readiness — strong
의사결정자가 바로 행동할 수 있는 문서다. 핵심 trade-off("공학 함수를 MVP에서 빼고 사칙연산으로 좁힌다")가 §0·§1·§5·§6에서 결정으로 명시되어 있고, 포기한 것(제품명이 약속하는 공학 기능)을 §6.2 `[NOTE FOR PM]`에서 정직하게 드러낸다. §8 Open Questions는 실제로 열려 있는 질문(표시 자리수 정책, 단축키 매핑)이고, §8 하단에 해소된 결정(괄호 포함, 디스플레이 동시 표시)을 날짜와 함께 분리 기록한 것도 좋다 — 수사적 질문이 아니다.

### Findings
- **medium** 제품명 vs MVP 범위의 미해결 긴장 (§6.2, §8.3) — 제품명이 "공학용 계산기"인데 MVP는 사칙연산뿐이다. `[NOTE FOR PM]`로 표시되어 정직하지만, 결정 자체("이름을 바꿀지 / v2까지 이름을 유지할지")는 미뤄져 있다. 저stakes라 blocker는 아니나, 이름은 Vision 첫 줄(가제 표기 있음)과도 얽혀 다운스트림 혼란 요인. *Fix:* §8에 "MVP 출시 시 표시할 제품명" Open Question을 1줄 추가하거나, "v1은 '계산기'로, '공학용'은 v2 목표 명칭" 식으로 결정을 못박기.

## Substance over theater — strong
furniture가 거의 없다. 페르소나는 1명(지민)으로 UJ-1을 통해 실제로 FR-1/FR-2/FR-3을 구동한다 — 페르소나 극장 없음. Vision은 "간결함"이라는 구체적 베팅을 담고 있어 임의의 계산기 PRD에 그대로 붙지 않는다(swap test 통과). NFR도 boilerplate("scalable/secure/reliable")가 아니라 제품 특화된 2개("입력→화면 즉각 반응", "키보드만으로 핵심 동작 전부")로 절제되어 있다. 저stakes 도구에 차별화 섹션이나 시장 분석을 넣지 않은 것도 적절한 절제다.

(findings 없음 — furniture 부재가 이 PRD의 강점.)

## Strategic coherence — strong
명확한 thesis가 있다: "공학 기능을 욱여넣기보다 사칙연산을 빠르고 정확하고 안 멈추게." 기능 우선순위가 이 thesis에서 직접 도출된다(공학 함수·히스토리·테마를 의도적으로 뒤로). Success Metrics(SM-1 정확성, SM-2 안정성)가 thesis를 직접 검증하며, §7에서 "DAU 같은 정량 지표는 추적 안 함"을 `[ASSUMPTION]`으로 명시한 것은 thesis와 일치하는 정직한 선택이다 — 활동량 측정이 아니라 품질 측정. 저stakes 학습 도구에서 counter-metric 부재는 문제 아님.

### Findings
- **low** SM이 사용자 도달/채택을 측정하지 않음 (§7) — "정확하고 안 멈추는 계산기"로 성공을 정의한 것은 thesis 정합적이나, "학생이 실제로 새 탭에서 연다"(UJ-1의 핵심)는 검증되지 않는다. 저stakes에서 수용 가능. *Fix:* 불필요. 필요 시 "수동 사용성 체크 1회" 정도의 가벼운 정성 확인만 메모.

## Done-ness clarity — strong
엔지니어가 "done"을 알 수 있다. FR-1~FR-4 모두 testable consequence를 가지며, 특히 검증 가능한 구체 수치 예시가 박혀 있다: `2 + 3 × 4` = `14`(우선순위), `1.2.3` 방지(소수점), `5 ÷ 0` 오류 표시. "graceful/reasonable/user-friendly" 같은 측정 불가 형용사는 거의 없다 — 단 NFR의 "체감되지 않을 것(즉각 반응)"은 형용사형이나, 저stakes에서 latency 임계 수치까지 요구하는 것은 과형식화이므로 지적하지 않는다.

### Findings
- **medium** FR-2의 결과 표시 정밀도가 미정의 (§4.1 FR-2) — "부동소수점 결과는 합리적 자리수로 표시"가 `[ASSUMPTION]`+§8 Open Question 1로 위임되어 있다. 이는 `0.1 + 0.2` 같은 흔한 입력에서 바로 드러나는 동작이라 구현자가 임의 결정하게 되면 SM-1(정확성 0건) 판정 기준이 흔들린다. *Fix:* §8 Q1을 MVP 착수 전 해소 대상으로 명시(예: "유효숫자 12자리, 그 이상은 반올림"). 저stakes라 정밀 정책까지는 불필요하나 기본값 1줄은 필요.
- **low** FR-1의 입력 검증 경계 모호 (§4.1 FR-1) — 소수점 중복(`1.2.3`)은 막지만, 연산자 연속 입력(`5 ++ 3`)이나 빈 괄호(`()`) 같은 다른 잘못된 입력의 처리는 FR-3("미완성 수식")에 흡수되는지 불명확. *Fix:* FR-3 consequence에 "연산자/괄호 문법 오류도 오류 상태로 처리" 1줄 추가하면 경계가 닫힌다.

## Scope honesty — strong
이 PRD의 최강 차원이다. §5 Non-Goals가 명시적이고, `[NON-GOAL for MVP]`로 공학 함수·각도 모드·상수·메모리를 v2 후보로 못박았다. de-scoping(괄호는 포함, 히스토리는 제외)이 이유와 함께 드러나 있고 silent하지 않다. `[ASSUMPTION]` 3건이 §9 Assumptions Index에 모두 roundtrip되고, 인덱스 하단에 "확정 완료" 항목까지 날짜로 분리 기록했다. open-items 밀도(Open Q 3 + ASSUMPTION 3 + NOTE 1)는 저stakes 도구 기준 적정 — 과하지 않다.

(critical/high findings 없음.)

## Downstream usability — adequate
이 PRD는 chain-top(UX → 아키텍처 → 스토리로 흐름)이므로 다운스트림 추출 가능성이 중요하다. Glossary(§3)가 존재하고 핵심 명사(수식/결과/디스플레이/기본 사칙연산)가 FR·UJ 전반에서 일관 사용된다. FR-1~4, UJ-1, SM-1~2 ID가 연속·고유하며 "Realizes UJ-1" / "Validates FR-2" 교차참조가 해소된다. UJ-1은 명명된 protagonist(지민)를 갖고 floating UJ가 없다.

### Findings
- **low** UJ가 1개뿐이라 일부 FR이 UJ로 추적되지 않음 (§2.3, §4.1) — FR-4(초기화/삭제)는 어떤 UJ도 명시적으로 실현하지 않는다(UJ-1은 입력·평가·오류만 다룸). 저stakes·간결 지향에서 UJ를 더 만드는 것은 과형식화이므로 추가 권장 안 함. *Fix:* 불필요. FR-4는 자명한 보조 기능이라 UJ 없이도 done-ness가 명확함.

## Shape fit — strong
shape가 제품과 잘 맞는다. 저stakes 소비자/학생 대상이라 UJ 1개 + 가벼운 페르소나가 load-bearing하면서도 과하지 않다. 차별화·시장·다수 이해관계자 섹션을 넣지 않은 것은 hobby/solo·저stakes 형태에 정확히 부합한다 — under-formalized도 over-formalized도 아니다. §0 문서 목적에서 "최대한 간결" 방향을 선언하고 실제로 그 분량을 지킨 것이 shape fit의 증거다.

(findings 없음.)

## Mechanical notes
- **Glossary drift:** 없음. "디스플레이/수식/결과/기본 사칙연산"이 §3 정의와 본문에서 일관 사용. ×/÷ 기호와 키보드 `*`/`/` 매핑도 FR-1에서 명시적으로 연결됨.
- **ID 연속성:** FR-1~FR-4 연속·고유, UJ-1 단일, SM-1~SM-2 연속. 교차참조("Realizes UJ-1", "Validates FR-2/FR-3") 모두 해소됨. 갭/중복 없음.
- **Assumptions Index roundtrip:** 인라인 `[ASSUMPTION]` 3건(§2.1, §4.1 FR-2, §7)이 모두 §9에 인덱싱됨. 역방향도 일치. roundtrip 완전.
- **`[NON-GOAL for MVP]` / `[NOTE FOR PM]`:** §5·§6.2에 실제 긴장 지점(제품명 vs 범위)에 정확히 배치됨 — 안전한 체크포인트에 형식적으로 붙인 것이 아님.
- **필수 섹션:** 저stakes·간결 지향 기준 모두 충족. 누락 없음.
