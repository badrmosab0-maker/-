# PERFORMANCE.md — Phase A-1 Evidence and C1 Cleanup

## C1 decision

اخترت **C1(c)**: حد FAST الصارم في عميل Stockfish هو `movetime 1800ms` لكل المواضع. السبب: حل واحد موحد وقابل للتدقيق بدل منطق خاص بنوع الموضع، مع إبقاء كل النتائج الخام ظاهرة. هذا لا يجعل كل pipeline UI أقل من 1800ms؛ القياس يشمل بدء Worker، الإلغاء، تحديث React، وانتظار `REFINED`.

## Final headless benchmark after C1(c)

تم القياس على production preview `http://127.0.0.1:4173/` داخل Chromium Headless 131، 100 عينة و30 وضعًا مميزًا، وكل العينات صالحة.

| Position type | Samples | First PV p50 / p95 / p99 | First bestmove p50 / p95 / p99 |
|---|---:|---:|---:|
| Opening | 40 | 380 / 642 / 742ms | 2589 / 2920 / 3045ms |
| Tactical | 30 | 374 / 482 / 493ms | 2614 / 2818 / 2820ms |
| Endgame | 30 | 392 / 536 / 591ms | 2630 / 3687 / 3803ms |
| **Overall** | **100** | **383 / 591 / 680ms** | **2616 / 2983 / 3687ms** |

**C1 disclosure:** under the 1800ms engine movetime cap, the measured end-to-`REFINED` UI pipeline remains above 2s in this environment. The result is reported, not hidden. First PV remains below 800ms at p95 overall.

Cold start: first board 378ms, first `readyok` 710ms, first analysis 2714ms.

Raw report: [`artifacts/phase-a1-headless-preview.json`](./artifacts/phase-a1-headless-preview.json).

## C8 headed vs headless

The headed run used `xvfb-run -a` with `HEADLESS=false` on the same production preview, same 100 positions, same Chromium build.

| Metric | Headless p50 / p95 / p99 | Headed p50 / p95 / p99 | Headed delta vs headless |
|---|---:|---:|---:|
| First PV painted | 383 / 591 / 680ms | 388 / 727 / 899ms | +1.3% / +23.0% / +32.2% |
| First bestmove / REFINED | 2616 / 2983 / 3687ms | 2610 / 2955 / 3686ms | -0.2% / -0.9% / 0.0% |

| Position type | Headless PV p50/p95/p99 | Headed PV p50/p95/p99 | Headless bestmove p50/p95/p99 | Headed bestmove p50/p95/p99 |
|---|---|---|---|---|
| Opening (40) | 380 / 642 / 742ms | 389 / 777 / 899ms | 2589 / 2920 / 3045ms | 2608 / 2875 / 2887ms |
| Tactical (30) | 374 / 482 / 493ms | 382 / 480 / 492ms | 2614 / 2818 / 2820ms | 2604 / 2808 / 2817ms |
| Endgame (30) | 392 / 536 / 591ms | 407 / 734 / 1717ms | 2630 / 3687 / 3803ms | 2610 / 3686 / 3837ms |

### Anomalies

- Headed mode increased first-PV p95/p99, especially one endgame outlier; this is expected display/compositing overhead under Xvfb.
- Bestmove distributions were nearly identical headed vs headless; the bottleneck is Stockfish/worker/UI completion rather than painting.
- All 200 benchmark samples (100 headless + 100 headed) were valid; no sample was dropped.

## Environment disclosure

- OS: Linux x86_64 (Manus Sandbox)
- Browser: Chromium 131.0.6778.33
- Preview: Vite production preview on port 4173
- Stockfish: `stockfish@19.0.1`, WASM inside Web Worker
- Threads: 5 (`navigator.hardwareConcurrency - 1`), Hash: 128MB, MultiPV: 3
- NNUE is embedded in the shipped WASM; no separate NNUE file exists for an independent hash.

## User-device status

Windows/macOS and physical user hardware remain **Pending** until the user runs the kit and uploads outputs. Sandbox results must not be labeled Windows/macOS.
