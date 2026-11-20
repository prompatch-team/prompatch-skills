# Improve: turning the analysis into one clear request

Summary: the shape of a good request and the rules for rewriting the user's request into it.
Read when: after analysis, before replying.

<!-- 담당: 리릭 (개선점). 사용자가 읽고 바로 "응"이라고 할 수 있는지가 기준이다. -->

## Request format

```
[대상] 어느 화면·기능 하나
[지금] 지금 어떻게 보이거나 동작하는지
[원하는 것] 바뀐 뒤 어떻게 보이거나 동작해야 하는지
[건드리지 말 것] 이번에 바꾸지 않을 것
[확인 방법] 고쳐졌는지 사용자가 직접 확인하는 방법 하나
```

## Rules

- **One target.** If the analysis has several, choose the one blocking the user now; put the rest in `later`.
- **Observable result.** "예쁘게" → "버튼 글자를 크게, 화면 가운데에". Say what the user would see.
- **Always fill 건드리지 말 것.** This is what keeps the AI from changing more than asked.
- **Check the user can verify it.** 확인 방법 must be something they can do without code ("휴대폰으로 열어서 공유 버튼을 누르면 카카오톡 창이 뜬다").
- **Mark guesses.** Anything from `guessed` keeps its `(추측)` mark so the user can correct it.
- **User's words.** Reuse the user's own words for things they named; do not rename their screens.

## Example

User (third time): "공유 버튼 다시 만들어 줘"

```
[대상] 첫 화면 아래쪽 '공유' 버튼
[지금] 눌러도 아무 일이 없음
[원하는 것] 누르면 휴대폰 공유 창이 뜨고, 주소가 복사됨 (추측: 카카오톡으로 보내려는 것)
[건드리지 말 것] 버튼 위치와 색, 다른 화면
[확인 방법] 휴대폰으로 사이트를 열어 공유 버튼을 누르면 공유 창이 뜬다
```

TODO(리릭): 사용자가 이 형식을 읽고 이해하는지 기준으로 칸 이름과 순서 다듬기, 예시 2개 더.

## Output

Write the Improve result in the format in `handoff.md`.
