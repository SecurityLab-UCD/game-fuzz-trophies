# Game Fuzz Trophies

Bugs found by [game-fuzz](https://github.com/SecurityLab-UCD/game-fuzz), a coverage-guided fuzzing framework for video games.

## Status Legend

| Symbol | Meaning                              |
| ------ | ------------------------------------ |
| ✅     | Confirmed, not yet reported upstream |
| 📬     | Reported upstream                    |
| 🔧     | Fixed upstream                       |

## Summary

| Game                                                                  | Language | Bugs |
| --------------------------------------------------------------------- | -------- | ---- |
| [pacman](https://github.com/ebuc99/pacman)                            | C++      | 1    |
| [sokoban](https://github.com/swatteau/sokoban-rs)                     | Rust     | 22   |
| [SuperMarioBros](https://github.com/MitchellSternke/SuperMarioBros-C) | C        | 87   |
| [zel](https://github.com/superjer/tinyc.games/tree/main/zel-game)     | C        | 2    |

## pacman

| #   | Bug ID     | Type  | Description                          | Location                               | Repro | Median TTF | Status |
| --- | ---------- | ----- | ------------------------------------ | -------------------------------------- | ----- | ---------- | ------ |
| 1   | `4765e56d` | UBSan | left shift of negative value (`-23`) | `figur.cpp:27` in `Figur::move_left()` | 5/5   | 13.7s      | ✅     |

<details>
<summary>Bug 1 — full stack trace</summary>

```
figur.cpp:27:45: runtime error: left shift of negative value -23
    #0 0x4ed5f9 in Figur::move_left(int, int) (/usr/local/bin/pacman+0x4ed5f9)
    #1 0x540b74 in Pacman::move_left(int, int) (/usr/local/bin/pacman+0x540b74)
    #2 0x5c583b in FunnyAnimation::animate() (/usr/local/bin/pacman+0x5c583b)
    #3 0x5a5998 in MenuMain::show() (/usr/local/bin/pacman+0x5a5998)
    #4 0x52a51c in main (/usr/local/bin/pacman+0x52a51c)
    #5 0x7c1bad1f91c9  (/lib/x86_64-linux-gnu/libc.so.6+0x2a1c9)
    #6 0x7c1bad1f928a in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2a28a)
    #7 0x424ec4 in _start (/usr/local/bin/pacman+0x424ec4)
SUMMARY: UndefinedBehaviorSanitizer: undefined-behavior figur.cpp:27:45 in
```

- **Stack hash:** `4765e56dfe89692185e53b50475074f6a18ed293`
- **Dedup method:** stack-hash (depth 3)
- **Signal:** EXIT (exit code 1, UBSan `halt_on_error=1`)
- **Discovery times:** 9.3s, 9.6s, 13.7s, 16.5s, 19.8s (5 runs)
- **Found by:** `game-fuzz rand` with hierarchical scheduling, adaptive mutator
- **Date:** 2026-03-03

</details>

## sokoban

22 unique window crashes found across 5 runs. Root cause analysis pending.

| #   | Bug ID     | Type         | Repro | Median TTF | Status |
| --- | ---------- | ------------ | ----- | ---------- | ------ |
| 1   | `5894ec75` | Window crash | 2/5   | 1.97h      | ✅     |
| 2   | `544f7b37` | Window crash | 1/5   | 1.55h      | ✅     |
| 3   | `32ab483b` | Window crash | 1/5   | 2.88h      | ✅     |
| 4   | `35644d0b` | Window crash | 1/5   | 2.90h      | ✅     |
| 5   | `c4bc3b71` | Window crash | 1/5   | 2.96h      | ✅     |
| 6   | `2ed19311` | Window crash | 1/5   | 3.08h      | ✅     |
| 7   | `ea1a5a9e` | Window crash | 1/5   | 3.08h      | ✅     |
| 8   | `bd571b5c` | Window crash | 1/5   | 3.12h      | ✅     |
| 9   | `9750210a` | Window crash | 1/5   | 3.78h      | ✅     |
| 10  | `14406af3` | Window crash | 1/5   | 3.78h      | ✅     |
| 11  | `3f86e942` | Window crash | 1/5   | 3.79h      | ✅     |
| 12  | `fc0016d7` | Window crash | 1/5   | 3.91h      | ✅     |
| 13  | `5e040baf` | Window crash | 1/5   | 4.55h      | ✅     |
| 14  | `6d5a32c7` | Window crash | 1/5   | 6.46h      | ✅     |
| 15  | `1c684e69` | Window crash | 1/5   | 6.58h      | ✅     |
| 16  | `1857462a` | Window crash | 1/5   | 6.80h      | ✅     |
| 17  | `5566b48a` | Window crash | 1/5   | 7.24h      | ✅     |
| 18  | `5aeb4a88` | Window crash | 1/5   | 7.85h      | ✅     |
| 19  | `06596623` | Window crash | 1/5   | 10.58h     | ✅     |
| 20  | `4621c012` | Window crash | 1/5   | 12.76h     | ✅     |
| 21  | `db88b537` | Window crash | 1/5   | 16.44h     | ✅     |
| 22  | `b3a82ae0` | Window crash | 1/5   | 22.98h     | ✅     |

- **Signal:** WINDOW_CRASH (SDL window unexpectedly closed)
- **Dedup method:** branch-path hash
- **Found by:** `game-fuzz rand` with hierarchical scheduling, adaptive mutator
- **Date:** 2026-03-03

## SuperMarioBros

87 unique window crashes found across 5 runs. Root cause analysis pending.

<details>
<summary>All 87 window crash bugs</summary>

| #   | Bug ID     | Type         | Repro | Median TTF | Status |
| --- | ---------- | ------------ | ----- | ---------- | ------ |
| 1   | `4f960487` | Window crash | 1/5   | 6.4m       | ✅     |
| 2   | `920c9084` | Window crash | 1/5   | 6.6m       | ✅     |
| 3   | `36b0b170` | Window crash | 1/5   | 9.2m       | ✅     |
| 4   | `99513038` | Window crash | 1/5   | 11.2m      | ✅     |
| 5   | `fd7fffa3` | Window crash | 1/5   | 13.4m      | ✅     |
| 6   | `bc18906b` | Window crash | 1/5   | 13.5m      | ✅     |
| 7   | `f3993df1` | Window crash | 2/5   | 25.5m      | ✅     |
| 8   | `bdce1768` | Window crash | 1/5   | 17.4m      | ✅     |
| 9   | `45e2b817` | Window crash | 1/5   | 22.5m      | ✅     |
| 10  | `14f5efed` | Window crash | 1/5   | 23.5m      | ✅     |
| 11  | `f0cee5a4` | Window crash | 1/5   | 31.2m      | ✅     |
| 12  | `5169fc4b` | Window crash | 1/5   | 32.0m      | ✅     |
| 13  | `5697c4ec` | Window crash | 1/5   | 32.4m      | ✅     |
| 14  | `9c0153de` | Window crash | 1/5   | 32.7m      | ✅     |
| 15  | `f005e55d` | Window crash | 1/5   | 33.2m      | ✅     |
| 16  | `3f8af60e` | Window crash | 1/5   | 34.8m      | ✅     |
| 17  | `67c499c5` | Window crash | 1/5   | 36.0m      | ✅     |
| 18  | `77feb98c` | Window crash | 1/5   | 36.3m      | ✅     |
| 19  | `ffd39564` | Window crash | 1/5   | 37.1m      | ✅     |
| 20  | `707f4312` | Window crash | 1/5   | 38.4m      | ✅     |
| 21  | `e27312bb` | Window crash | 1/5   | 43.9m      | ✅     |
| 22  | `80b090a1` | Window crash | 1/5   | 45.5m      | ✅     |
| 23  | `d0965a59` | Window crash | 1/5   | 46.3m      | ✅     |
| 24  | `a9e268ec` | Window crash | 1/5   | 49.7m      | ✅     |
| 25  | `daf0def5` | Window crash | 1/5   | 50.5m      | ✅     |
| 26  | `a857dd15` | Window crash | 1/5   | 50.8m      | ✅     |
| 27  | `8dc62ad0` | Window crash | 1/5   | 55.5m      | ✅     |
| 28  | `19efbbe0` | Window crash | 1/5   | 57.0m      | ✅     |
| 29  | `c4c71f1e` | Window crash | 1/5   | 57.6m      | ✅     |
| 30  | `5e6b063c` | Window crash | 1/5   | 59.3m      | ✅     |
| 31  | `a7e276da` | Window crash | 1/5   | 1.03h      | ✅     |
| 32  | `a0fb5ba8` | Window crash | 1/5   | 1.04h      | ✅     |
| 33  | `aab45172` | Window crash | 1/5   | 1.04h      | ✅     |
| 34  | `0eec4254` | Window crash | 1/5   | 1.10h      | ✅     |
| 35  | `4283c118` | Window crash | 1/5   | 1.14h      | ✅     |
| 36  | `a3bcd9b6` | Window crash | 1/5   | 1.16h      | ✅     |
| 37  | `a3e7c3ff` | Window crash | 1/5   | 1.33h      | ✅     |
| 38  | `38d0f117` | Window crash | 1/5   | 1.34h      | ✅     |
| 39  | `267e06a0` | Window crash | 1/5   | 1.35h      | ✅     |
| 40  | `4598dee6` | Window crash | 1/5   | 1.36h      | ✅     |
| 41  | `b80a55fc` | Window crash | 1/5   | 1.45h      | ✅     |
| 42  | `894677f5` | Window crash | 1/5   | 1.80h      | ✅     |
| 43  | `4af82bda` | Window crash | 1/5   | 2.28h      | ✅     |
| 44  | `e71f1c56` | Window crash | 1/5   | 2.32h      | ✅     |
| 45  | `4d8546b4` | Window crash | 1/5   | 2.36h      | ✅     |
| 46  | `876fb91d` | Window crash | 1/5   | 2.63h      | ✅     |
| 47  | `a85eaf96` | Window crash | 1/5   | 2.64h      | ✅     |
| 48  | `6ffdd552` | Window crash | 1/5   | 2.83h      | ✅     |
| 49  | `206ea6ef` | Window crash | 1/5   | 3.42h      | ✅     |
| 50  | `43674faa` | Window crash | 1/5   | 3.82h      | ✅     |
| 51  | `fa9e1521` | Window crash | 1/5   | 3.89h      | ✅     |
| 52  | `2d3422c4` | Window crash | 1/5   | 3.89h      | ✅     |
| 53  | `0d546218` | Window crash | 1/5   | 5.16h      | ✅     |
| 54  | `4ff49ca9` | Window crash | 1/5   | 5.25h      | ✅     |
| 55  | `19f877ce` | Window crash | 1/5   | 5.35h      | ✅     |
| 56  | `432a28cf` | Window crash | 1/5   | 5.58h      | ✅     |
| 57  | `823c013b` | Window crash | 1/5   | 5.77h      | ✅     |
| 58  | `459de103` | Window crash | 1/5   | 6.40h      | ✅     |
| 59  | `c525657a` | Window crash | 1/5   | 6.45h      | ✅     |
| 60  | `fbbacecf` | Window crash | 1/5   | 6.50h      | ✅     |
| 61  | `0f5bb9e0` | Window crash | 1/5   | 7.14h      | ✅     |
| 62  | `cc543672` | Window crash | 1/5   | 7.62h      | ✅     |
| 63  | `d1a5b312` | Window crash | 1/5   | 7.66h      | ✅     |
| 64  | `d687e1d1` | Window crash | 1/5   | 7.97h      | ✅     |
| 65  | `ab7174f6` | Window crash | 1/5   | 7.97h      | ✅     |
| 66  | `42a1f8dc` | Window crash | 1/5   | 8.22h      | ✅     |
| 67  | `c2085615` | Window crash | 1/5   | 8.27h      | ✅     |
| 68  | `c7d2b9b2` | Window crash | 1/5   | 8.28h      | ✅     |
| 69  | `8b682071` | Window crash | 1/5   | 8.38h      | ✅     |
| 70  | `dbd5b60c` | Window crash | 1/5   | 8.70h      | ✅     |
| 71  | `36747a6b` | Window crash | 1/5   | 8.72h      | ✅     |
| 72  | `57ab2b83` | Window crash | 1/5   | 8.75h      | ✅     |
| 73  | `2d1fbd23` | Window crash | 1/5   | 8.84h      | ✅     |
| 74  | `acfdc5e2` | Window crash | 1/5   | 8.87h      | ✅     |
| 75  | `b9b8b44b` | Window crash | 1/5   | 8.93h      | ✅     |
| 76  | `ce13049c` | Window crash | 1/5   | 11.29h     | ✅     |
| 77  | `8c792ab7` | Window crash | 1/5   | 13.30h     | ✅     |
| 78  | `ea17781b` | Window crash | 1/5   | 13.35h     | ✅     |
| 79  | `d15acd5e` | Window crash | 1/5   | 13.50h     | ✅     |
| 80  | `0adfb783` | Window crash | 1/5   | 13.54h     | ✅     |
| 81  | `c3886190` | Window crash | 1/5   | 13.62h     | ✅     |
| 82  | `3859517c` | Window crash | 1/5   | 13.74h     | ✅     |
| 83  | `8dd0ae28` | Window crash | 1/5   | 14.37h     | ✅     |
| 84  | `688aa2ae` | Window crash | 1/5   | 15.07h     | ✅     |
| 85  | `62482a5e` | Window crash | 1/5   | 17.87h     | ✅     |
| 86  | `6b51ea7f` | Window crash | 1/5   | 18.12h     | ✅     |
| 87  | `788a3ee0` | Window crash | 1/5   | 18.30h     | ✅     |

</details>

- **Signal:** WINDOW_CRASH (SDL window unexpectedly closed)
- **Dedup method:** branch-path hash
- **Found by:** `game-fuzz rand` with hierarchical scheduling, adaptive mutator
- **Date:** 2026-03-03

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
