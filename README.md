# PromPatch

바이브 코딩을 하다 막혔을 때, 같은 요청을 반복하는 대신 **구체적인 요청 하나**로 바꿔 주는 스킬이에요.

"해 줘", "다시 해 줘", "안 돼"만 반복하거나 같은 오류가 계속 나면, PromPatch가 그 순간을 알아채고 지금 하려는 것, 막힌 곳, 다시 보낼 요청을 한 번에 보여 줘요.

When you are stuck while building with AI, PromPatch notices the loop (repeated short requests, the same error again) and turns it into one specific, scoped request you can approve.

## 쓰는 법

### v0

입력창에서 `/`를 누르고 `prompatch`를 찾아 붙여요. 붙이면 그 대화 내내 적용돼요.

### Claude Code, Cursor, Codex 등

```bash
npx skills add prompatch-team/prompatch-skills
```

## 하는 일

1. **감지**: 짧은 요청 반복, 같은 오류 반복, 결정 없이 "알려 줘"만 하기 등
2. **분석**: 지금 하려는 것과 막힌 곳 정리 (모르는 건 추측으로 표시)
3. **개선**: 대상, 지금 상태, 원하는 것, 건드리지 말 것, 확인 방법을 담은 요청 하나
4. **전달**: 위 내용을 한 번의 답으로 보여 줌. 질문을 여러 번 하지 않아요

막히지 않았을 때는 아무것도 하지 않아요.

## 구조

```
skills/prompatch/
├── SKILL.md              흐름과 답하는 형식
└── references/
    ├── detect.md         막힘 신호
    ├── analyze.md        의도와 막힌 곳 정리
    ├── improve.md        요청 다시 쓰기
    └── handoff.md        단계 사이에 넘기는 형식
```

## 만든 사람

PRE-SOMA 부산 PromPatch 팀 ([prompatch-team](https://github.com/prompatch-team)). 문제나 제안은 이 저장소의 Issues에 남겨 주세요.

## License

MIT
