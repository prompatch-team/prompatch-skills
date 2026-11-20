# Analyze: what the user is trying to do and where it breaks

Summary: how to work out intent, the current attempt and the stuck point from the conversation and the project, without guessing silently.
Read when: a stuck signal was found.

<!-- 담당: 카티 (분석). 질문 대신 추측을 쓰고 (추측)으로 표시하는 원칙을 지킨다. -->

## Fill these three, in order

1. **intent**: the result the user wants to see or have, not the code change. Example: "친구에게 링크를 보내 같이 들어오게 하고 싶다".
2. **trying_now**: what they have been asking for in the last few turns. Example: "공유 버튼을 눌러도 아무 일이 없어서 다시 만들어 달라고 함".
3. **stuck_at**: one of these kinds, plus the specific place.
   - `screen`: a screen or part looks or behaves wrong
   - `feature`: a feature is missing or does not run
   - `error`: an error message or crash
   - `goal`: the goal itself is unclear or keeps changing

## Where to look

- The user's own words first, then the latest result and any error they pasted.
- The project files only to confirm what you read in the conversation. Do not read the whole project.

## Rules

- Separate `known` from `guessed`. Anything not stated or visible goes in `guessed` with `(추측)`.
- Do not ask questions here. Guess, mark it, and let the user correct it in one reply.
- If `stuck_at` is `goal`, intent comes first: the improved request should fix the goal, not the code.

TODO(카티): 막힌 곳 종류별 예시 하나씩, fix-bug의 "쉬운 질문 4개"에서 가져올 수 있는 것 정리.

## Output

Write the Analyze result in the format in `handoff.md`.
