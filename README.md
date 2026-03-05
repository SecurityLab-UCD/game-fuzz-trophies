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
| zel  | [zel](https://github.com/superjer/tinyc.games/tree/main/zel-game) | C        | 2    |

## zel

| #   | Bug ID     | Type  | Description                                   | Location                            | Repro | Median TTF | Status |
| --- | ---------- | ----- | --------------------------------------------- | ----------------------------------- | ----- | ---------- | ------ |
| 1   | `7b589be6` | UBSan | array index out of bounds (`int[11][15]`)     | `player.c:191` in `update_player()` | 5/5   | 9.1m       | ✅     |
| 2   | `43d78e29` | LSan  | memory leak (1.4 MB leaked in 21 allocations) | `zel.c:204` in `setup()`            | 5/5   | 5.1h       | ✅     |

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

<details>
<summary>Bug 2 — full stack trace</summary>

```
=================================================================
==25558==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 576 byte(s) in 6 object(s) allocated from:
    #0 0x4a1d98 in __interceptor_calloc asan_malloc_linux.cpp:77:3
    #1 0x7f5a451d8442  (/lib/x86_64-linux-gnu/libSDL2-2.0.so.0+0xdc442)
    #2 0x7f5a451d07fa  (/lib/x86_64-linux-gnu/libSDL2-2.0.so.0+0xd47fa)
    #3 0x5556e7 in setup /app/projects/zel-d/zel-d/./zel.c:204:24
    #4 0x553a4e in main /app/projects/zel-d/zel-d/./zel.c:170:9
    #5 0x7f5a44ed11c9  (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9)
    #6 0x7f5a44ed128a in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a)
    #7 0x41f494 in _start (/app/projects/zel-d/zel-d/bin+0x41f494)
SUMMARY: AddressSanitizer: 1460184 byte(s) leaked in 21 allocation(s).
```

- **Stack hash:** `43d78e29a68d066a377c1f1c10d57e45d8c3d53d`
- **Dedup method:** stack-hash (depth 3)
- **Signal:** EXIT (exit code 1, LSan `halt_on_error=1`)
- **Discovery times:** 15204s, 15326s, 18507s, 28101s, 34372s (5 runs)
- **Found by:** `game-fuzz rand` with hierarchical scheduling, adaptive mutator
- **Date:** 2026-02-25

</details>
