# v0·skills.sh 스킬 개발 정리

2026-11-19 기준으로 직접 확인한 내용이에요. 감자님은 3·4절(올리기, v0)을 꼭 보고, 나머지는 6절만 봐도 돼요.

## 1. 한눈에

엔진 4개를 Agent Skills 형식의 **스킬 하나**로 만들어 **공개 GitHub 저장소**에 올리면, skills.sh에 뜨고 v0·Claude Code·Cursor·Codex 어디서나 쓸 수 있어요.

| 누가 | 어디서 | 어떻게 쓰나 |
| --- | --- | --- |
| 비전공자 (첫 고객) | v0 | 입력창에서 `/` → 우리 스킬 고르기. 붙이면 그 대화 내내 적용 |
| 터미널 쓰는 사람 | Claude Code, Cursor, Codex 등 | `npx skills add prompatch-team/prompatch-skills` |

꼭 알아야 할 것 세 가지:

1. **v0로는 스킬만 가요.** 훅·에이전트는 따라가지 않으니 감지도 스킬 본문에 적어요.
2. **무료 v0는 하루 메시지 7개예요.** 되묻는 횟수를 줄여야 해요.
3. **skills.sh는 등록 버튼이 없어요.** 누가 CLI로 설치해야 목록에 오르므로, 올린 뒤 우리가 먼저 한 번 설치해요.

## 2. Agent Skills 표준 (agentskills.io)

Anthropic이 만들어 공개 표준으로 풀었고, v0·Claude Code·Cursor·Codex 등이 같은 형식을 써요.

```
skill-name/
├── SKILL.md      ← 필수: frontmatter + 지침
├── references/   ← 선택: 필요할 때만 읽는 문서
├── scripts/      ← 선택: 실행 코드
└── assets/       ← 선택: 템플릿, 이미지, 데이터
```

| 항목 | 필수 | 규칙 |
| --- | --- | --- |
| `name` | 예 | 1~64자. 소문자·숫자·하이픈만. 하이픈으로 시작·끝 안 됨, `--` 안 됨. **폴더 이름과 같아야 함** |
| `description` | 예 | 1~1024자. 무엇을 하는지 + 언제 쓰는지 + 찾을 만한 단어 |
| `license` | 아니오 | 라이선스 이름 또는 동봉 파일 이름 |
| `compatibility` | 아니오 | 500자 이내. 필요한 환경이 있을 때만 |
| `metadata` | 아니오 | 문자열 키-값 (예: `author`, `version`) |
| `allowed-tools` | 아니오 | 실험 기능이라 도구마다 다름. 우리는 안 씀 |

읽히는 순서:

1. **찾기:** 시작할 때는 모든 스킬의 `name`과 `description`만 읽어요(약 100토큰). 그래서 description이 가장 중요해요.
2. **켜기:** 요청이 description과 맞으면 `SKILL.md` 본문 전체를 읽어요. 본문은 **500줄 이하** 권장.
3. **실행:** `references/` 등은 필요할 때만 열어요.

파일을 가리킬 때는 스킬 폴더 기준 상대 경로(`references/detect.md`)로, `SKILL.md`에서 **한 단계 깊이까지만** 가리켜요. 검사는 `skills-ref validate ./skills/prompatch`로 해요.

## 3. skills.sh: 올리는 법

Vercel이 만든 공개 스킬 목록과 CLI(`npx skills`)예요.

1. 공개 GitHub 저장소에 스킬 폴더를 둬요.
2. 누군가 `npx skills add 조직/저장소`로 설치하면, CLI의 익명 사용 기록으로 **자동으로** 목록에 오르고 설치 수로 순위가 매겨져요.
3. 그래서 올린 뒤 **우리가 먼저 한 번 설치**해요. 이때 사용 기록을 끄면(`DO_NOT_TRACK=1`, `DISABLE_TELEMETRY=1`) 목록에 안 올라갈 수 있어요.

| 명령 | 뜻 |
| --- | --- |
| `npx skills add 조직/저장소` | 저장소의 스킬 설치 (도구 고르는 창이 떠요) |
| `... --list` | 설치하지 않고 들어 있는 스킬만 보기 |
| `... --skill 이름` | 스킬 하나만 설치 |
| `... -a claude-code` | 특정 도구에만 설치 |
| `npx skills find 단어` | skills.sh에서 찾기 |

`skills.sh.json`은 저장소 맨 위에 두면 skills.sh 페이지에서 스킬을 묶어 보여 주는 설정이에요. 설치에는 영향이 없고, 누군가 설치한 뒤에 읽혀요.

## 4. v0에서 스킬 쓰기

v0는 2026년 6월부터 스킬을 지원해요. 입력창에서 붙이면 그 대화의 모든 생성에 적용돼요.

| 종류 | 어디서 가져오나 | 우리에게 |
| --- | --- | --- |
| remote | skills.sh의 공개 스킬 | **사용자가 우리 스킬을 붙이는 길** |
| memory | 개인 또는 팀에 저장한 스킬 | 만들면서 써 보는 용도 |
| project | 대화에 연결한 GitHub 저장소 안의 스킬 | 저장소를 연결한 사람만 |

- 붙이는 법: 입력창 도구 메뉴에서 고르거나, `/`를 치거나, 요청에서 이름을 말해요.
- v0에는 훅이 없어요. 대신 Rules(맞춤 지침), MCP 연결, 통합(DB·결제 등)이 있어요.
- 요금(11/19 기준): Free는 하루 메시지 7개, Plus는 사용자당 월 $30. 스킬 자체는 무료라 Free에서도 skills.sh 스킬을 쓸 수 있어요.

## 5. 공식 예시에서 배울 점

**vercel-labs/agent-skills**
- description 틀: ① 무엇을 하는지 한 문단 ② "Use when:" 뒤에 사용자가 실제로 할 말 ③ 다루는 범위
- 규칙에 중요도(Critical, High, Medium 등)를 붙여 큰 것부터 보게 해요.
- 규칙을 바로 판단할 수 있게 구체적으로 써요.

**anthropics/skills**
- `skills/`에 스킬을 두고 `.claude-plugin/`을 더해, 한 저장소를 skills.sh와 Claude Code 플러그인 양쪽에 써요. 우리도 나중에 이렇게 하면 돼요.
- 대부분 Apache 2.0이에요. **문장은 가져오지 않고 구조와 원칙만 참고해요.**

## 6. 우리 스킬에 적용할 것

- **스킬 하나**에 엔진 4개를 담고, 엔진마다 `references/` 파일을 하나씩 둬요.
- frontmatter는 `name`과 `description`만 써요.
- 훅, 에이전트, `${CLAUDE_PLUGIN_ROOT}` 같은 Claude Code 전용 기능은 쓰지 않아요. v0에서 깨져요.
- 사용자에게 여러 번 묻지 않아요. 모르는 건 추측하고 `(추측)`으로 표시해요.
- 규칙에는 이유와 예시를 붙여요. 규칙만 나열하면 AI가 덜 따라요.
- 공개 저장소라 키·토큰·웹훅 주소·개인 정보는 넣지 않아요.

올리기 전 확인표:

- [ ] 폴더 이름 = `name` (소문자·숫자·하이픈, 64자 이내)
- [ ] `description` 1024자 이내, 한국어 표현 포함
- [ ] `SKILL.md` 500줄 이하
- [ ] `skills-ref validate`로 검사
- [ ] README(무엇을 하는지, 설치법, v0에서 붙이는 법, 한계)와 LICENSE
- [ ] 공개 저장소로 push → `npx skills add`로 한 번 설치(사용 기록 켜고) → skills.sh에 뜨는지 확인
- [ ] v0에서 `/`로 검색해 붙여지는지 확인

## 출처

- Agent Skills 개요와 Specification: https://agentskills.io
- skills.sh 첫 페이지, FAQ, Customize 문서: https://skills.sh
- vercel-labs/skills README: https://github.com/vercel-labs/skills
- v0 Design Systems 2.0 문서와 Platform API 가이드: https://v0.app/docs
- v0 가격 페이지: https://v0.app/pricing
- vercel-labs/agent-skills: https://github.com/vercel-labs/agent-skills
- anthropics/skills: https://github.com/anthropics/skills
