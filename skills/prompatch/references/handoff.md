# Handoff formats between engines

Summary: the shape each engine hands to the next. Change this file only with the whole team; every engine depends on it.
Read when: passing a result from one step to the next.

<!-- 담당: 전원. 바꾸려면 디스코드에 먼저 알리고 네 명이 확인한다. -->

## Detect result (detect.md → analyze.md)

```
signal: <one id from detect.md, e.g. repeat-short>
evidence: "<the user's own words, quoted>"
turns: <how many recent turns show it>
```

## Analyze result (analyze.md → improve.md)

```
intent: <what the user ultimately wants, one sentence>
trying_now: <what they tried in the last few turns, one sentence>
stuck_at: <where it breaks: screen / feature / error / unclear goal>
known: <facts from the conversation or project, short list>
guessed: <anything inferred, marked (추측)>
```

## Improve result (improve.md → reply)

```
request: <the rewritten request, in the improve.md format>
why: <one sentence: what changed compared to the user's request>
later: <other things the user wanted, short list, may be empty>
```

## Rules

- Keep field names in English and values in the user's language.
- Every field is one line or a short list. If a step cannot fill a field, write `모름` instead of inventing.
- These results are internal. The user sees only the reply format in `SKILL.md`.

## Worked example: one chain from start to end

Conversation so far: the user built a site in v0. Their last three messages were "공유 버튼 만들어 줘", "안 돼", "다시 해 줘".

Detect result:
```
signal: repeat-short
evidence: "안 돼", "다시 해 줘"
turns: 2
```

Analyze result (starts from the Detect result):
```
intent: 친구에게 사이트 링크를 쉽게 보내고 싶다 (추측)
trying_now: 공유 버튼을 만들었지만 눌러도 반응이 없어 다시 만들어 달라고 함
stuck_at: feature - 첫 화면 아래 '공유' 버튼이 동작하지 않음
known: 버튼은 화면에 보임, 누르면 아무 일 없음
guessed: 카카오톡으로 보내려는 것 (추측)
```

Improve result (starts from the Analyze result):
```
request: [대상] 첫 화면 아래 '공유' 버튼 / [지금] 눌러도 아무 일 없음 / [원하는 것] 누르면 휴대폰 공유 창이 뜨고 주소가 복사됨 / [건드리지 말 것] 버튼 위치와 색, 다른 화면 / [확인 방법] 휴대폰에서 공유 버튼을 누르면 공유 창이 뜬다
why: "다시 해 줘" 대신 무엇이 안 되고 어떻게 되면 되는지를 적음
later: 모름
```

Reply to the user: the `SKILL.md` reply format, filled from the three results above.
