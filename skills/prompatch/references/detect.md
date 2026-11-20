# Detect: signs that the user is stuck

Summary: which patterns count as "stuck", with examples, and when not to trigger.
Read when: every turn, before doing the requested work.

<!-- 담당: 훈 (감지). 신호를 더하거나 뺄 때 예시와 근거를 같이 적는다. -->

Trigger only when at least one signal below is clearly present. When unsure, do not trigger; a false alarm is worse than a late one.

## Signals

| id | Signal | Looks like (Korean examples) | Priority |
|---|---|---|---|
| `repeat-short` | Two or more short requests in a row with no new information | "다시 해 줘", "해 줘", "안 돼", "또 안 돼" | High |
| `same-error` | The same error or broken result comes back after a fix | the user pastes the same error twice; "아까랑 똑같아" | High |
| `tell-me-only` | Keeps asking to be told but never decides | "뭐가 좋아? 알려 줘" three times, no choice made | Medium |
| `wrong-insist` | Pushes a fix based on a wrong idea after it failed | "CSS만 바꾸면 돼" when the cause is elsewhere | Medium |
| `do-everything` | Asks for many unrelated changes at once with no priority | "디자인도 바꾸고 로그인도 넣고 다 고쳐 줘" | Medium |
| `not-mine` | Wants to change an app they cannot explain | "남이 만든 건데 여기 고쳐 줘", "어디가 어딘지 몰라" | Low |

TODO(훈): 실제 바이브 코딩 대화에서 신호별 예시를 2개 이상 모으고, 우선순위를 근거로 정하기.

## Do not trigger when

- The first request in a conversation is short. Short alone is not stuck.
- The user gives new information each time, even briefly ("버튼 색만 파랑으로").
- The user says they want to explore freely ("아이디어 여러 개 보여 줘").

## Output

Write the Detect result in the format in `handoff.md`. Quote the user's words exactly; do not paraphrase the evidence.
