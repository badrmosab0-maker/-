# Test Quality Audit — Phase A-1

## Vitest (19)

| File | Test name | Assertion | Type |
|---|---|---|---|
| `tests/chess-rules.test.ts` | يقبل النقلات القانونية ويرفض غير القانونية | e2e4 قانونية وe2e5 غير قانونية | edge/property |
| `tests/chess-rules.test.ts` | يدعم التبييت | نقل الملك والرخ إلى g1/f1 | edge |
| `tests/chess-rules.test.ts` | يدعم en passant | إزالة بيدق d5 ووضعه d6 | edge |
| `tests/chess-rules.test.ts` | يدعم الترقية | تحويل a7 إلى وزير a8 | edge |
| `tests/chess-rules.test.ts` | يكشف كش مات وجمود وتعادل | checkmate/stalemate/draw flags | edge |
| `tests/chess-rules.test.ts` | يتحقق من FEN ويشرح الفشل | FEN صالح وغير صالح | property |
| `tests/chess-rules.test.ts` | يعيد بناء PGN ويحتفظ بكل وضع | خمس أوضاع ووضع النهاية | integration |
| `tests/chess-rules.test.ts` | يحوّل تقييم UCI من منظور الدور | cp وmate من منظور الأبيض | property |
| `tests/engine-protocol.test.ts` | يقرأ MultiPV وعمق وعقد وPV | parse UCI metadata/PV | protocol/property |
| `tests/engine-protocol.test.ts` | يحوّل mate ويحافظ على منظور الأبيض | mate sign وM3 text | edge |
| `tests/engine-protocol.test.ts` | يقرأ bestmove none كما هو | لا يختلق bestmove | failure |
| `tests/engine-protocol.test.ts` | يتحقق من قانونية المرشح قبل عرضه | legal/illegal UCI | safety |
| `tests/performance.test.ts` | يحسم hash path المحلي تحت 800ms | 200 updates تحت الميزانية | performance |
| `tests/performance.test.ts` | لا يعيد تحليل نفس hash | unchanged update changed=false | property |
| `tests/position-pipeline.test.ts` | يعطي hash متساويًا للوضع نفسه ومختلفًا عند تغير الدور | Zobrist determinism | property |
| `tests/position-pipeline.test.ts` | لا يبدأ إصدارًا جديدًا إذا لم يتغير الوضع | version remains zero | property |
| `tests/position-pipeline.test.ts` | يرفع الإصدار عند تغير موضع فعليًا ويحفظ المصدر | version/source update | integration |
| `tests/position-pipeline.test.ts` | يمثل تعارض مصدرين دون تخمين | conflict status/message | failure |
| `tests/cancellation.test.ts` | يرفض الوعد ولا يسمح بنتيجة متأخرة بعد الإلغاء | pending promise rejects and worker terminates | failure/concurrency |

## Playwright (14)

| File | Test name | Assertion | Type |
|---|---|---|---|
| `tests/e2e/phase-a.spec.ts` | click source square then destination square updates FEN | e2-e4 changes FEN/history/best move | integration |
| `tests/e2e/phase-a.spec.ts` | paste FEN, validate it, render the board, and start analysis | FEN loads, king renders, analysis starts | integration |
| `tests/e2e/phase-a.spec.ts` | paste four-move PGN, navigate to move two, and assert FEN | PGN rebuild and position 2/4 | integration |
| `tests/e2e/phase-a-gap.spec.ts` | mate in 1 is detected in the browser and no illegal recommendation is shown | mate is reached through legal move | edge |
| `tests/e2e/phase-a-gap.spec.ts` | mate score path remains visible for a longer mating position | mate/score card is rendered | edge |
| `tests/e2e/phase-a-gap.spec.ts` | stalemate is detected in the browser | stalemate label | edge |
| `tests/e2e/phase-a-gap.spec.ts` | all four castling variants are legal in the browser | K-side/Q-side for both colors | edge |
| `tests/e2e/phase-a-gap.spec.ts` | en passant is legal in the browser | captured pawn removed | edge |
| `tests/e2e/phase-a-gap.spec.ts` | promotion dialog supports queen rook bishop and knight | all four promotion outcomes | edge |
| `tests/e2e/phase-a-gap.spec.ts` | undo and redo preserve position integrity | FEN round trip | integration |
| `tests/e2e/phase-a-gap.spec.ts` | board flip changes the square order without changing the FEN | a8→h1 and same FEN | UI/property |
| `tests/e2e/phase-a-gap.spec.ts` | FAST and DEEP toggles change the observed search depth | depth comparison | performance/UI |
| `tests/e2e/phase-a-gap.spec.ts` | worker crash displays recovery state and restart resumes analysis | error then restart/refined | failure/recovery |
| `tests/e2e/phase-a-gap.spec.ts` | illegal move attempt is rejected without changing the position | warning and unchanged FEN | safety |

All listed tests were run on **Linux (Manus sandbox)**. User hardware status remains pending until the self-verification kit is run.
