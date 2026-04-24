# cc-roundtable 플러그인 개발 룰

## 개요

다분야의 세계 최고 수준 전문가를 동적으로 선정하고, 에이전트 팀으로 구조화된 토론을 수행하여,
다각적인 평가·제언을 정리하는 플러그인. 코드 리뷰에 한정하지 않고, 웹사이트 평가, 사업 전략,
디자인, 조직 설계 등 모든 도메인에 대응합니다.

## 중요 파일

| 파일 | 역할 |
|---|---|
| `skills/start/SKILL.md` | 스킬 본체. 4페이즈 파이프라인 정의 |
| `skills/start/references/expert-archetypes.md` | 전문가 아키타입 집 (도메인→권장 전문가) |
| `skills/start/references/deliberation-rules.md` | 토론 룰 (수렴 조건, 가중치, 소수 의견 보호) |
| `skills/start/references/output-format.md` | 출력 포맷 |

## 설계 사상

- **"그저 토론시키기만 해서는 가치가 없다. 구조 설계가 전부"** (ICLR 2025 MAD 서베이)
- 독립된 의견 생성을 먼저 한다 (델파이 기법·NGT)
- 반대 의견을 구조적으로 보장한다 (Devil's Advocate)
- 반복은 최대 2라운드 (3 이상은 역효과: Degeneration of Thought)
- 소수 의견을 구조적으로 보호한다 (Groupthink 회피)
- 자기 신고의 확신도는 사용하지 않는다 (연구에서 과신이 확인됨)

## 개발 방침

- SKILL.md가 파이프라인의 정(正)
- expert-archetypes.md는 원리 베이스(키워드 리스트가 아님)로 설계
- "지나친 동의 문제"에 대한 대책을 항상 의식
- Claude Code / Cursor 양쪽 대응. SKILL.md에서는 툴 고유의 구현을 지정하지 않음

## 테스트 방법

다음 도메인에서 테스트:
- 기술계: 웹사이트 평가, 코드 아키텍처
- 비기술계: 사업 전략, 디자인 평가, 조직 설계
- 대립이 생기는 케이스에서 Phase 2가 정상 동작하는지 확인
- 소수 의견이 최종 리포트에 포함되는지 확인

## 🇰🇷 한국어 기준 추가사항

- `expert-archetypes.md`에 한국 시장 특유의 규제 (PIPA, ISMS-P, 전자상거래법 등)와 플랫폼 (Naver, Kakao, Velog/Tistory 등)에 대한 보완 박스 추가
- 본문의 톤을 한국 개발자에게 자연스러운 "-습니다" 정중체로 통일
- 전문 용어는 원어(Sycophancy, Groupthink, Devil's Advocate 등)를 유지하되 최초 등장 시 한글 설명 병기
