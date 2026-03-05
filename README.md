# Game Fuzz Trophies

Bugs found by [game-fuzz](https://github.com/SecurityLab-UCD/game-fuzz), a coverage-guided fuzzing framework for video games.

## Status Legend

| Symbol | Meaning                              |
| ------ | ------------------------------------ |
| ✅     | Confirmed, not yet reported upstream |
| 📬     | Reported upstream                    |
| 🔧     | Fixed upstream                       |

## Summary

| Game | Source                                                            | Language | Bugs |
| ---- | ----------------------------------------------------------------- | -------- | ---- |
| zel  | [zel](https://github.com/superjer/tinyc.games/tree/main/zel-game) | C        | 1    |

## zel

| #   | Bug ID     | Type  | Description                               | Location                            | Repro | Median TTF | Status |
| --- | ---------- | ----- | ----------------------------------------- | ----------------------------------- | ----- | ---------- | ------ |
| 1   | `7b589be6` | UBSan | array index out of bounds (`int[11][15]`) | `player.c:191` in `update_player()` | 5/5   | 9.1m       | ✅     |

<details>
<summary>Bug 1 — full stack trace</summary>

```
player.c:191:13: runtime error: index 11 out of bounds for type 'int[11][15]'
    #0 0x519e52 in update_player /app/projects/zel-d/zel-d/./player.c:191:13
    #1 0x553b47 in main /app/projects/zel-d/zel-d/./zel.c:182:17
    #2 0x7f...1c9  (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9)
    #3 0x7f...28a in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a)
    #4 0x41f494 in _start (/app/projects/zel-d/zel-d/bin+0x41f494)
SUMMARY: UndefinedBehaviorSanitizer: undefined-behavior player.c:191:13
```

- **Stack hash:** `7b589be66ef15ec551ae5c616427b64614ec3ec2`
- **Dedup method:** stack-hash (depth 3)
- **Signal:** EXIT (exit code 1, UBSan `halt_on_error=1`)
- **Discovery times:** 164s, 273s, 544s, 677s, 846s (5 runs)
- **Found by:** `game-fuzz rand` with hierarchical scheduling, adaptive mutator
- **Date:** 2026-02-25

</details>
