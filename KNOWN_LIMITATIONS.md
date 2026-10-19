# Known Limitations — Chess Analysis Assistant (Manual Entry)

هذه الوثيقة ليست roadmap ولا طلبًا لبدء Phase B؛ إنها قائمة بما بقي خارج Phase A أو يحتاج تحققًا إضافيًا.

- السحب غير مدعوم؛ الإدخال بالضغط على مربع المصدر ثم الوجهة.
- Image recognition/OCR وScreen Capture وlive monitoring وTauri خارج Phase A.
- التحقق على Windows/macOS وعتاد المستخدم الحقيقي Pending حتى تشغيل kit ورفع المخرجات.
- benchmark headed مستقل مطلوب وموثق في `PERFORMANCE.md` قبل اعتماد Phase B؛ لا نخلط نتائجه بنتائج Sandbox headless.
- FAST يستخدم حدًا صارمًا 1800ms؛ DEEP يستخدم 3500ms ولا يوجد ادعاء بميزانية DEEP مستقلة.
- PWA install وTTS وtime-aware search وopening book وexternal overlay غير مدعومة.
