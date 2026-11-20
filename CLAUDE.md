# PromPatch 공개 스킬 저장소: Claude 작업 지침

이 저장소에서 일하는 Claude Code가 매 세션 읽는 지침이다. 스킬 형식·skills.sh·v0에 대해 확실하지 않으면 `docs/skill-guide.md`를 먼저 읽는다.

## 1. 무엇을 만드는가

바이브 코딩 중 막힌 순간을 감지해 구체적인 요청 하나로 바꿔 주는 **공개 스킬 하나**(`skills/prompatch/`)를 만든다. 이 스킬은 skills.sh에 올라가고, v0·Claude Code·Cursor·Codex 등에서 쓰인다. 첫 고객은 바이브 코딩으로 만들었지만 유지보수가 어려운 비전공자(예비 초기 창업가 포함)다.

엔진 4개는 파일 하나씩이다: 감지 `references/detect.md`, 분석 `references/analyze.md`, 개선점 `references/improve.md`, 통합 `SKILL.md`. 엔진 사이 형식은 `references/handoff.md`.

## 2. 공개 스킬 규칙 (Claude Code 전용 기능 금지)

이 스킬은 v0처럼 훅·에이전트·도구가 없는 곳에서도 돌아야 한다.

- frontmatter는 `name`과 `description`만 쓴다. `disable-model-invocation`, `argument-hint`, `allowed-tools`, `model` 등은 넣지 않는다.
- 파일은 스킬 폴더 기준 상대 경로로 가리킨다 (`references/detect.md`). `${CLAUDE_PLUGIN_ROOT}`나 절대 경로를 쓰지 않는다.
- `SKILL.md`에서 참고 파일로 한 단계만 간다. 참고 파일끼리 서로 읽게 하지 않는다.
- 훅, 서브에이전트, AskUserQuestion, 스크립트 실행을 전제로 쓰지 않는다. 감지도 본문 지침으로 한다.
- 사용자에게 여러 번 묻지 않는다 (무료 v0는 하루 메시지 7개). 모르는 것은 추측하고 `(추측)`으로 표시한다.
- `name`은 폴더 이름과 같고, 소문자·숫자·하이픈 64자 이내. `description`은 1024자 이내이고 한국어 예시 표현을 넣는다.
- `SKILL.md`는 500줄 이하. 길어지면 `references/`로 나눈다.
- 지시문은 영어, 사용자가 읽는 말과 예시는 한국어.

## 3. 작업 원칙

1. **자기 담당 파일만 바꾼다.** 담당은 각 파일 맨 위 주석에 있다. 다른 파일에서 문제를 찾으면 고치지 말고 보고 끝에 목록으로 남긴다.
2. **`handoff.md`는 혼자 바꾸지 않는다.** 바꿔야 하면 `TODO(팀 결정)`으로 남긴다.
3. **팀이 정할 일은 묻는다.** 스킬 이름, 새 파일, 새 엔진, 폴더 구조 변경은 임의로 하지 않는다.
4. **비밀 정보를 넣지 않는다.** 공개 저장소다. 키, 토큰, 웹훅 주소, 개인 정보, 개인 컴퓨터 경로를 파일·커밋에 쓰지 않는다.
5. **다른 사람의 글을 옮기지 않는다.** 공개 스킬 예시(vercel-labs, anthropics)는 구조만 참고하고 문장은 새로 쓴다.
6. **모르는 것은 지어내지 않는다.** 확실하지 않으면 `(확인 필요)`로 표시한다.

## 4. 검증

| 무엇을 | 어떻게 |
|---|---|
| frontmatter 규칙 | `name` = 폴더 이름, 글자 수 제한 확인. 가능하면 `skills-ref validate ./skills/prompatch` |
| 줄 수 | `wc -l skills/prompatch/SKILL.md` (500 이하) |
| 비밀값 | `git diff`에 키·주소가 없는지 눈으로 확인 |
| 설치 | 공개 후 `npx skills add prompatch-team/prompatch-skills --list`로 스킬이 보이는지 |

## 5. 보고 형식

작업이 끝나면 다음 형식으로 보고한다.

- 바꾼 파일과 이유
- 확인한 방법과 결과 (못 했으면 못 했다고)
- 고치지 않은 문제 목록
- 팀 결정이 필요한 것

사용자가 "오늘 정리", "작업 보고"를 요청하면 위 내용에 더해 쓴 기술과 이유, 누구의 근거로 무엇이 바뀌었나, 발표에 쓸 거리를 1,800자 이내로 정리해 Big-O에 붙여 넣을 수 있게 코드 블록으로 준다. 파일로 저장하면 `daily/`(커밋 안 함)에 둔다.
