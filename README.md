# 🇰🇷 CC-Roundtable (원탁회의)

**다분야 전문가를 동적으로 소집해, 구조화된 토론으로 다각적 평가·제언을 만드는 Claude Code 플러그인.**

---

## 왜 Roundtable인가?

AI에게 "여러 전문가가 되어 토론해줘"라고 하면, 대체로 다음과 같은 일이 일어납니다.

- 처음 발언한 전문가의 의견에 나머지가 끌려간다 (**앵커링 효과**)
- 2-3턴이면 원만하게 합의해버린다 (**Groupthink / 집단사고**)
- 토론을 길게 시킬수록 소수 의견이 사라진다 (**Degeneration of Thought**)

결국 **5명이 있어도 1명이 5역을 연기하는 꼴**이 되기 쉽습니다.

Roundtable은 이 문제를 **구조**로 풉니다.

1. **처음에는 전원이 독립적으로 분석** — 서브 에이전트를 병렬로 띄워 물리적으로 격리된 컨텍스트에서 분석
2. **비판자(Devil's Advocate)를 반드시 포함** — 약점을 찾는 것만이 일인 1명
3. **소수 의견은 구조적으로 보호** — 기각할 거면 이유를 명기, "다수파니까"는 불가
4. **토론은 최대 2라운드** — 학술 연구에 기반한 하드 캡

---

## 설치

### 방법 1: GitHub 마켓플레이스에서 설치 (권장)

Claude Code 세션에서 아래 두 줄을 차례로 실행합니다.

```bash
# 1) 이 저장소를 마켓플레이스로 등록
/plugin marketplace add gaebalai/cc-roundtable

# 2) 플러그인 설치
/plugin install cc-roundtable@gaebalai-marketplace
```

> `gaebalai-marketplace`는 [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json)에 정의된 마켓플레이스 이름이고, `cc-roundtable`은 그 안에 등록된 플러그인 이름입니다.

### 방법 2: 로컬 경로로 설치 (직접 클론한 경우)

저장소를 이미 로컬에 받아둔 상태라면 로컬 경로를 마켓플레이스로 지정할 수 있습니다.

```bash
# 저장소 클론
git clone https://github.com/gaebalai/cc-roundtable.git

# Claude Code 세션에서
/plugin marketplace add /절대/경로/cc-roundtable
/plugin install cc-roundtable@gaebalai-marketplace
```

### 방법 3: 수동 복사 (최소 구성)

플러그인 관리 명령을 쓰지 않고 직접 배치하고 싶은 경우:

```bash
# 전역: 모든 프로젝트에서 사용
mkdir -p ~/.claude/plugins
cp -r cc-roundtable ~/.claude/plugins/

# 또는 프로젝트 로컬: 해당 프로젝트에서만 사용
mkdir -p .claude/plugins
cp -r cc-roundtable .claude/plugins/
```

### 업데이트 / 제거

```bash
/plugin update cc-roundtable@gaebalai-marketplace
/plugin uninstall cc-roundtable@gaebalai-marketplace
```

---

## 사용법

Claude Code 세션에서 다음과 같이 호출합니다.

### 기본 사용

```bash
# 웹사이트 평가
이 웹사이트를 평가해줘: https://example.com

# 설계 리뷰
지금 논의 중인 설계 방침을 다각도로 리뷰해줘

# 기획서 평가
이 기획서의 전략을 평가해줘 (파일 첨부)

# 명시적 호출
원탁회의를 열어서 이 API 설계를 검토해줘
```

### 심도(Depth) 지정

의제의 무게에 따라 토론 심도가 자동 판정되지만, 명시적으로 지정할 수도 있습니다.

```bash
# Quick: 독립 분석만으로 통합, 빠르게 확인
가볍게 이 로직을 검토해줘

# Standard: 최대 2라운드 토론 (기본)
이 아키텍처를 다각도로 평가해줘

# Deep: 최대 2라운드 + 세컨드 찬스, 가장 철저하게
이 사업 전략을 철저하게 분석해줘
```

---

## 🇰🇷 한국 개발자용 활용 예

### 예시 1: Velog / Tistory 초안 리뷰

```
[Velog에 올릴 글 초안을 붙여넣고]
이 기술 블로그 초안을 다각도로 평가해줘.
독자는 3-5년차 백엔드 개발자가 중심.
```

→ 편집자, 타깃 독자, SEO 전문가, Devil's Advocate 등의 패널로 토론

### 예시 2: 국내 서비스 런칭 리스크 분석

```
한국 시장을 대상으로 하는 B2C SaaS를 런칭하려 한다.
PIPA 준수, ISMS-P 취득 스케줄, 과금 체계를 포함해
사업 전략을 철저하게 분석해줘.
```

→ 전략 컨설턴트, 재무 애널리스트, 법무·컴플라이언스(PIPA 특화), Devil's Advocate 등

### 예시 3: 사내 개발 표준 리뷰

```
팀에서 책정한 AI 백엔드 개발 표준(첨부)에 대해
다각도로 리뷰해줘. 특히 운용 단계에서의 리스크를 중점적으로.
```

→ 아키텍트, SRE, 보안 엔지니어, Devil's Advocate 등

---

## 구조의 상세

### 4-Phase 파이프라인

```
Phase 0: 의제 분석·전문가 선정 (오케스트레이터 단독)
Phase 1: 독립 분석 (병렬. 각 전문가는 다른 의견을 보지 않음)
Phase 2: 구조화된 토론 (대립점만 최대 2라운드) ※ Standard / Deep
Phase 3: 통합·평결 (소수 의견 체크 필수)
Phase 4: 리포트 출력
```

### 출력 리포트의 구성

```markdown
# 원탁회의 리포트: [의제]

## 이그제큐티브 서머리
## 전문가 패널 (각 전문가의 종합 평가)
## 합의 사항
## 주요 쟁점과 결론 (찬성 측·반대 측·판단 이유)
## 소수 의견 (생략 불가)
## 권장 액션 (우선순위 포함)
## 토론의 한계
```

---

## 학술적 배경

이 플러그인은 다음 연구·이론을 참고하여 설계되었습니다:

- **델파이 기법 (Delphi Method)** — 독립된 의견 생성
- **Nominal Group Technique** — 구조화된 집단 의사결정
- **Groupthink (Janis 1972)** — 집단사고의 폐해와 회피 수단
- **Devil's Advocate** — 구조적 반대 역할의 필요성
- **Adversarial Collaboration (Kahneman)** — 적대적 협업
- **Multi-Agent Debate (Du et al. 2023)** — LLM의 멀티 에이전트 토론 연구
- **S2-MAD (NAACL 2025), CONSENSAGENT (ACL 2025)** — 최신 MAD 연구
- **Six Thinking Hats (de Bono 1985)** — 관점 다양화 기법

---

## 크레딧

- **제작자**: gaebalai
- **라이선스**: MIT

## 피드백

버그 리포트나 개선 제안은 리포지토리의 Issues로 올려주세요.
