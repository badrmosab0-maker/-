# Chess Analysis Assistant (Manual Entry)

مساعد تحليل شطرنج محلي للإدخال اليدوي، مبني بـ React/TypeScript. يستخدم `chess.js` لقواعد النقلات وStockfish 19.0.1 WASM داخل Web Worker للتحليل المحلي. لا يحتاج حسابًا أو خادمًا أو API مدفوعًا.

## نطاق Phase A

- النقر على مربع المصدر ثم النقر على مربع الوجهة.
- إدخال FEN والتحقق منه ورسم الوضع.
- لصق PGN وإعادة بناء الوضع والتنقل بين النقلات.
- Stockfish UCI وMultiPV=3 وcp/mate وPV.
- Zobrist hashing وإلغاء التحليل القديم عند تغير الوضع.

**السحب غير مدعوم.** هذا الإصدار مخصص للإدخال اليدوي فقط؛ الميزات النظامية المتقدمة خارج نطاق Phase A ولا تظهر في الواجهة.

## التشغيل

```bash
pnpm install
pnpm dev
```

يفتح التطبيق على `http://localhost:3000`.

## التحقق الكامل

```bash
pnpm verify
```

يشغل الأمر typecheck وESLint واختبارات Vitest واختبارات Playwright على Chromium ثم production build. يحتفظ Playwright بالنتائج في `artifacts/playwright-results.json`.

## النشر الثابت

```bash
pnpm build
```

يمكن نشر `dist/` على static host مع توجيه `/` إلى `index.html`. يجب أن يخدم المضيف ملف WASM المحلي بنوع MIME مناسب. ملف `manus-routes.json` خاص بتكامل المعاينة وليس شرطًا خارج Manus.

## الترخيص

حزمة `stockfish@19.0.1` وملفات المحرك الموزعة في `public/stockfish/` مرخصة GPLv3 وتحتوي على `Copying.txt`. راجع تراخيص `chess.js` وReact وLucide React عند إعادة التوزيع.

## الأدلة

- [`SUPPORT.md`](./SUPPORT.md): حالة كل ميزة مع اسم الاختبار والقيود.
- [`PERFORMANCE.md`](./PERFORMANCE.md): قياس Chromium Phase A مع JSON خام.
- [`TEST-AUDIT.md`](./TEST-AUDIT.md): تدقيق أسماء الاختبارات والـassertions وتصنيفها.
- [`KNOWN_LIMITATIONS.md`](./KNOWN_LIMITATIONS.md): قيود Phase A المفتوحة، وليس قائمة ميزات جديدة.
- [`plan.md`](./plan.md): الخطة المعمارية الأصلية وسجل قرارات النطاق.

## How to run self-verification on your machine

بعد `pnpm install` و`pnpm build`، شغّل `pnpm preview --host 0.0.0.0` ثم نفّذ `HEADLESS=false bash self-verify/collect.sh`، أو `self-verify/collect.ps1` في Windows PowerShell. لتشغيل القياس المرئي: `HEADLESS=false BASE_URL=http://127.0.0.1:4173 node self-verify/benchmark.browser.mjs`. مخرجات Sandbox تسمى `report-linux-sandbox.json` و`perf-linux-sandbox.json`؛ مخرجات Windows تسمى `report-windows-user.json`. راجع `self-verify/SELF_VERIFY.md` و`self-verify/screenshots-checklist.md`، واحفظ JSON واللقطات في `self-verify/output/`. لا تُضمّن Tokens أو Cookies أو مفاتيح API.

## Preview and branding disclosure

بعد تشغيل `pnpm preview --host 0.0.0.0` تكون المعاينة المحلية على `http://127.0.0.1:4173/`. في بيئة Manus قد تتوفر معاينة عامة مؤقتة على رابط المنصة؛ لا يُخزّن الرابط داخل التطبيق. لم أتحقق من إعداد Dashboard الخاص بخيار **Hide Manus branding** من داخل هذا المشروع؛ لذلك لا أؤكد أنه ON. أي watermark خاص بواجهة Manus يُعامل كأثر Sandbox/Preview فقط، ولا يدخل في `dist/` أو build الإنتاجي للتطبيق.
