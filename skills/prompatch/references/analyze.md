# Analyze: what the user is trying to do and where it breaks

Summary: how to turn the Detect result into the five Analyze fields (intent, trying_now, stuck_at, known, guessed) from what the conversation shows, keeping facts and guesses apart.
Read when: a stuck signal was found.

<!-- 담당: 카티 (분석). 질문 대신 추측을 쓰고 (추측)으로 표시하는 원칙을 지킨다. -->

## Input

The Detect result is the only input: `signal`, `evidence`, `turns`. Start from the quoted `evidence` and read back through the conversation around it:

- the user's own words, including any error text they pasted. These are the facts.
- what the AI said it did in earlier replies ("첫 화면 아래에 공유 버튼을 추가했어요"). This shows what was attempted and where, not that it works.

Nothing else is a source. Do not open project files to hunt for the cause: many places that load this skill cannot, and what matters here is how the result differs from what the user expected, not which line of code is wrong.

## Fill the five fields, in order

1. **intent**: the result the user wants to see or have, not the code change. It is usually in the first request, before the short repeats began. Example: "친구에게 사이트 링크를 쉽게 보내고 싶다 (추측)".
2. **trying_now**: what they asked for over the last few turns and how it went. Example: "공유 버튼을 만들었지만 눌러도 반응이 없어 다시 만들어 달라고 함".
3. **stuck_at**: one kind from the table below, then ` - `, then the specific place, named the way the conversation names it.
4. **known**: what the conversation states. See "known and guessed".
5. **guessed**: what you inferred, each item ending with `(추측)`.

Every field is one line or a short list, written in the user's language.

## Choosing stuck_at

Pick exactly one kind. Describe what the user sees, never the cause.

| Kind | Choose when | The user says things like | Example |
|---|---|---|---|
| `screen` | Something is there and runs, but looks wrong: position, size, colour, text, layout, a different device | "글자가 잘려", "휴대폰에서 깨져" | `screen - 휴대폰에서 첫 화면 제목이 잘려 보임` |
| `feature` | The user does something (presses, types, submits) and the expected thing does not happen, or the thing is missing, with no error text | "눌러도 반응이 없어", "저장이 안 돼" | `feature - 첫 화면 아래 '공유' 버튼이 동작하지 않음` |
| `error` | The user pasted or described an error message, a red or white screen, or the app does not open at all | "빨간 글씨가 떠", "하얀 화면만 나와" | `error - '예약하기'를 누르면 "Failed to fetch"라는 글이 뜸` |
| `goal` | No single place can be named: the user cannot say what result they want, keeps changing it, or asks for many unrelated things at once | "그냥 알아서 해 줘", "다 고쳐 줘" | `goal - 디자인과 로그인 중 무엇을 먼저 할지 정해지지 않음` |

If more than one kind fits, take the first that applies in this order: `error`, `feature`, `screen`. An error shown after pressing a button is `error`, because the error text is the most exact fact the next step can use. Choose `goal` only when the conversation does not point to one place.

The signal is a hint, not a rule: `same-error` usually means `error`; `tell-me-only` and `do-everything` usually mean `goal`; `repeat-short`, `wrong-insist` and `not-mine` can be any kind. What the user described decides.

If `stuck_at` is `goal`, intent comes first: the improved request should fix the goal, not the code.

## known and guessed

Each thing you write belongs to one of three groups.

- **Stated → `known`.** The user said it, pasted it, or the AI's earlier reply said it. You must be able to point to the line. Keep the user's wording and copy error text exactly (the key part if it is long).
- **Inferred → `guessed`, ending with `(추측)`.** Why they want it, what "안 돼" means, which device, what causes the problem. A guess needs something in the conversation to stand on: a word the user chose, the kind of app, the language they write in. Keep the one or two guesses the next request depends on.
- **No basis → `모름`.** Write `모름` instead of a plausible story. An unmarked invention reaches the user looking like a fact, and they will not know to correct it.

Details that are easy to get wrong:

- A cause is always a guess, however likely. Nothing in a conversation proves it.
- The user's own diagnosis is known only as their belief: `사용자는 CSS 문제라고 생각함`.
- "안 돼" alone does not say how it fails. Look for the turn where the user described it; if there is none, how it fails is a guess or `모름`.
- If `intent` or the place in `stuck_at` is itself inferred, end that line with `(추측)` too.
- Do not swap the user's words for technical ones. "빨간 글씨" turned into "런타임 오류" is already a guess about the cause.

When the conversation says almost nothing, the result stays thin. `stuck_at` still needs one kind, so pick the likeliest and mark it. For "예약하기 만들어 줘" → "안 돼" → "다시 해 줘", with no description and no error text:

```
intent: 손님이 사이트에서 예약을 남길 수 있게 하고 싶다 (추측)
trying_now: 예약하기를 만들어 달라고 한 뒤 "안 돼"라며 다시 해 달라고 함
stuck_at: feature - '예약하기'가 되지 않음 (추측)
known: 예약하기를 요청함, 만든 뒤 "안 돼"라고 함
guessed: 모름
```

How it fails has no basis here, so `guessed` is `모름` rather than an invented symptom.

## Rules

- **Do not ask the user anything here.** Some plans allow only a few messages a day, and a question spends one. A marked guess costs nothing: the user sees it in the reply and can correct it in one line.
- **One stuck point.** If several things are broken, analyze the one the `evidence` is about.

## Example 1: a feature that does not respond

The share-button situation from the worked example in `handoff.md`, shown with the AI's replies and one more user turn so that every `known` item has a line behind it.

```
사용자: 공유 버튼 만들어 줘
AI: 첫 화면 아래에 공유 버튼을 추가했어요.
사용자: 버튼은 생겼는데 눌러도 아무 일도 없어
AI: 공유 버튼을 고쳤어요.
사용자: 안 돼
사용자: 다시 해 줘
```

Detect result:
```
signal: repeat-short
evidence: "안 돼", "다시 해 줘"
turns: 2
```

Analyze result:
```
intent: 친구에게 사이트 링크를 쉽게 보내고 싶다 (추측)
trying_now: 공유 버튼을 만들었지만 눌러도 반응이 없어 다시 만들어 달라고 함
stuck_at: feature - 첫 화면 아래 '공유' 버튼이 동작하지 않음
known: 버튼은 화면에 보임, 누르면 아무 일 없음
guessed: 카카오톡으로 보내려는 것 (추측)
```

Why: pressing does nothing and there is no error text, so the kind is `feature`. Both `known` items come from "버튼은 생겼는데 눌러도 아무 일도 없어"; the place comes from the AI's reply. The user never said why they want the button or where they share, so both are marked `(추측)`; they stand on the word "공유", which in a Korean conversation most often means sending a link through a messenger. Why the button does nothing is not written anywhere: nothing in the conversation supports a cause.

## Example 2: the same error comes back

```
사용자: 예약하기 누르면 이런 게 떠. Error: Failed to fetch
AI: 예약을 저장하는 부분을 고쳤어요.
사용자: 아까랑 똑같아. Error: Failed to fetch
```

Detect result:
```
signal: same-error
evidence: "아까랑 똑같아. Error: Failed to fetch"
turns: 2
```

Analyze result:
```
intent: 손님이 사이트에서 예약을 남길 수 있게 하고 싶다 (추측)
trying_now: 예약하기를 누르면 뜨는 오류를 고쳐 달라고 했지만 고친 뒤에도 같은 오류가 뜸
stuck_at: error - '예약하기'를 누르면 "Failed to fetch"라는 글이 뜸
known: 예약하기를 누를 때 뜸, 글은 "Error: Failed to fetch", 한 번 고친 뒤에도 똑같음
guessed: 예약 내용을 저장하는 곳과 연결이 안 된 것 (추측)
```

Why: a button is involved, but there is error text, so the kind is `error`, not `feature`. The error text is copied exactly into `known`. The cause stays in `guessed` even though it is likely.

## Output

Write the Analyze result in the format in `handoff.md`.
