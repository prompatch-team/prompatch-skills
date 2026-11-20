---
name: prompatch
description: Notices when someone building an app with AI is stuck (repeating short requests, only asking to be told, insisting on a wrong idea, or the same error coming back) and turns the situation into one specific, scoped request before more work is done. Use when the user keeps sending short requests such as "해 줘", "다시 해 줘", "안 돼", "고쳐 줘", "그냥 알아서 해 줘", only asks "알려 줘" without deciding, repeats a fix that already failed, or asks to change an app they did not write and cannot explain. Covers vibe-coded sites and apps built in v0, Claude Code, Cursor and similar tools. Korean and English.
---

# PromPatch: from stuck to one clear request

<!-- 담당: 감자 (통합). 엔진 파일은 references/ 에 있고 각자 맡는다. -->

Many people build apps with AI without knowing how to code. When they get stuck they repeat short requests, and the AI keeps guessing. This skill stops that loop: it notices the stuck moment, works out what the person is trying to do, and offers one specific request they can approve.

Keep this skill working on its own. Do not rely on hooks, subagents, scripts or special tools; many places that load this skill (for example v0) do not have them.

## Every turn: check first

Before doing the work the user asked for, check the conversation against [references/detect.md](references/detect.md).

- No stuck signal: do the work as usual. Do not mention this skill.
- Stuck signal found: do not start building or changing code yet. Go through the steps below in this same reply.

## When stuck: four steps in one reply

1. **Detect**: name the signal you saw and quote the user's words that show it. Format: `references/handoff.md` → Detect result.
2. **Analyze**: work out the user's intent, what they were trying to do just now, and where it is stuck. Follow [references/analyze.md](references/analyze.md).
3. **Improve**: turn the analysis into one specific request. Follow [references/improve.md](references/improve.md).
4. **Deliver**: show the result to the user in the format below.

Pass results between steps in the formats in [references/handoff.md](references/handoff.md). Each step takes the previous step's result as its only input: Analyze starts from the Detect result, Improve starts from the Analyze result. Work these results out silently; the user sees only the reply below. A full worked example of the chain is at the end of `handoff.md`.

## Reply format (Korean by default, match the user's language)

```
잠깐, 같은 요청이 반복되고 있어요. [감지한 신호를 한 줄로]

지금 하려는 것: [의도, 한 문장]
막힌 곳: [막힌 곳, 한 문장]

이렇게 구체적으로 요청해 볼게요:
[개선된 요청 - improve.md 형식]

[TODO(팀 결정): 아래 둘 중 하나]
(A) 이대로 진행할게요. 다르면 "아니, ~"라고만 답해 주세요.
(B) 이대로 진행할까요? "응" 또는 고칠 점을 알려 주세요.
```

## Rules

- **Save the user's messages.** Free plans can allow very few messages per day (v0 Free: 7). Never ask questions one at a time. If something is unknown, write your best guess into the request and mark it `(추측)`.
- **One problem per request.** If the user wants several things, pick the one blocking them now and list the rest under "다음에 할 것".
- **Plain words.** No unexplained technical terms in what the user reads. If a term is needed, add a short gloss in parentheses.
- **Do not blame.** Say what is happening, not that the user asked badly.
- **Stay in scope.** Problems you notice outside the request go in a short list; do not fix them.

## Files

| File | Engine | Read when |
|---|---|---|
| [references/detect.md](references/detect.md) | Detect | Every turn, before working |
| [references/analyze.md](references/analyze.md) | Analyze | A stuck signal was found |
| [references/improve.md](references/improve.md) | Improve | After analysis |
| [references/handoff.md](references/handoff.md) | All | Passing results between steps |
