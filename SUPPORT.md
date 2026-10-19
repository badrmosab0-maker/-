# SUPPORT.md — Chess Analysis Assistant (Manual Entry)

> **نطاق Phase A:** إدخال يدوي عبر اللوحة وFEN وPGN فقط. لا رؤية، لا مراقبة شاشة، ولا تطبيق Tauri في هذه المرحلة.

تفاصيل أسماء الاختبارات والـassertions وتصنيفها موجودة في [`TEST-AUDIT.md`](./TEST-AUDIT.md)؛ هذا الجدول هو مصدر حالة الدعم، وملف التدقيق يصف الأدلة دون إضافة ميزات.

| Feature | Status | Evidence (test name + file) | Limitations |
|---|---|---|---|
| Click source square → destination square | **Verified** | `click source square then destination square updates FEN` — `tests/e2e/phase-a.spec.ts` | السحب غير مدعوم؛ استخدم نقرتين  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Legal moves, castling, en passant, promotion | **Verified** | `قواعد الشطرنج الأساسية` — `tests/chess-rules.test.ts` | الاختبارات القانونية الأساسية في Vitest؛ لا ادعاء بسلوك UI لكل نوع قبل اختبار متصفح مخصص  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| FEN paste, validation, board render | **Verified** | `paste FEN, validate it, render the board, and start analysis` — `tests/e2e/phase-a.spec.ts` | لا يستنتج تاريخ repetition من FEN  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| PGN paste, reconstruction, navigation | **Verified** | `paste four-move PGN, navigate to move two, and assert FEN` — `tests/e2e/phase-a.spec.ts` | لا محرر مستقل للتفرعات المعقدة  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Stockfish WASM in persistent Worker | **Verified** | Browser assertions in `tests/e2e/phase-a.spec.ts`; UCI parsing in `tests/engine-protocol.test.ts` | نسخة lite-single؛ لا تعني تعدد خيوط WASM فعليًا  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| UCI lifecycle, MultiPV=3, cancellation | **Verified** | `Stockfish UCI protocol` — `tests/engine-protocol.test.ts`; browser re-analysis assertions — `tests/e2e/phase-a.spec.ts` | نتائج المحرك مرتبطة بالـ FEN/hash الحالي  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Zobrist determinism | **Verified** | `position pipeline` — `tests/position-pipeline.test.ts` | hash ليس بديلًا عن التحقق القانوني  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| cp/mate parsing and mate-in-N display data | **Verified** | `يحوّل mate ويحافظ على منظور الأبيض` — `tests/engine-protocol.test.ts`; `قواعد الشطرنج الأساسية` — `tests/chess-rules.test.ts` | عرض mate الكامل في UI يعتمد على وصول سطر UCI صالح  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Undo/redo and turn display | **UNTESTED** | لا يوجد اختبار Playwright مخصص بعد | لا ننقلها إلى Verified قبل اختبار المتصفح  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Orientation flip | **UNTESTED** | لا يوجد اختبار Playwright مخصص بعد | متاح في الواجهة لكن غير مثبت ببوابة Phase A  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| FAST/DEEP mode controls | **Verified** | `FAST and DEEP toggles change the observed search depth` — `tests/e2e/phase-a-gap.spec.ts`; headed/headless benchmark in `PERFORMANCE.md` | FAST engine cap is 1800ms; measured UI completion can exceed 2s on this Sandbox; DEEP uses 3500ms  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Image recognition / OCR / piece classification | **Unsupported** | لا يوجد module أو model في Phase A | مؤجل بالكامل إلى Phase B؛ لا model asset ولا ادعاء رؤية  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Screen capture / live monitoring | **Unsupported** | غير موجود في واجهة Phase A | مؤجل إلى Phase C مع Tauri؛ لا مراقبة خلفية أو التقاط شاشة  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Tauri Desktop | **Unsupported** | لم يبدأ Phase C | يتطلب بوابة Phase B أولًا  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Quick Entry mobile mode | **Unsupported** | لم يُنفذ | خارج Phase A  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Audio/TTS SAN announcement | **Unsupported** | لم يُنفذ | خارج Phase A  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Ghost/Arrow overlay | **Unsupported** | لم يُنفذ | لا overlay خارجي؛ تظليل المرشح ليس ميزة مستقلة مثبتة  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Time-Aware Search | **Unsupported** | لم يُنفذ | لا مصدر ساعة أو زمن متبقٍ  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Polyglot opening book | **Unsupported** | لم يُنفذ | التحليل ليس opening-book acceleration  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Pre-emptive top-2 opponent analysis | **Unsupported** | لم يُنفذ | MultiPV=3 ليس تحليلًا استباقيًا منفصلًا  Verified on: Linux (Manus sandbox); User-verified on: <pending> |
| Web/PWA installation | **UNTESTED** | لا اختبار install متصفح | قد يعمل كـ static app فقط؛ ليس deliverable Phase A  Verified on: Linux (Manus sandbox); User-verified on: <pending> |

## Failure behavior

- Worker failure أو timeout: تظهر حالة عدم توفر ولا تبقى توصية قديمة كأنها تخص الموضع الحالي.
- `bestmove (none)` مع نقلات قانونية: يرفضه العميل ولا يختلق نقلة.
- نتيجة متأخرة بعد تغيير FEN/hash: تُهمل.
- FEN/PGN غير صالح: لا يغير الموضع الحالي ويظهر خطأ التحقق.

## Platform verification

- تم التحقق الآلي على Linux داخل Sandbox فقط.
- Windows وmacOS وعتاد المستخدم الحقيقي: **Pending** إلى أن يشغّل المستخدم kit ويرفع المخرجات المسماة بوضوح.
- حالة خيار Dashboard **Hide Manus branding** غير قابلة للتحقق من ملفات المشروع؛ لا نعلن أنه ON. أي watermark في Preview يُعد أثر Sandbox فقط، وليس جزءًا من build الإنتاجي.
