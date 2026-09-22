# مستودع تحديثات TajirDZ

هذا هو مصدر التحديثات الوحيد. البرنامج المثبت عند العملاء يقرأ `version.json`
من هذا المستودع، وينزّل الحزمة المناسبة لجهازه من صفحة [Releases](https://github.com/ZAKI7543/TajirDZ-Updates/releases).

> لماذا الحزم في صفحة Releases وليست مجلدًا عاديًا؟ لأن GitHub يمنع أي ملف
> أكبر من 100MB داخل مجلدات المستودع، وحجم الحزمة ~600MB. صفحة Releases
> تقبل حتى 2GB للملف، وهي تعمل كمجلد حزم لكل إصدار — تُملأ بالسحب والإفلات
> من المتصفح.

## الحزم الخمسة لكل إصدار

| الملف | لمن |
|---|---|
| `TajirDZ_X.Y.Z_Windows_Modern_Setup.exe` | المضيف (جهاز الصندوق) — ويندوز 10/11 |
| `TajirDZ_X.Y.Z_Windows_Legacy_Setup.exe` | المضيف — ويندوز 7/8/8.1 |
| `TajirDZ_X.Y.Z_Windows_Modern_Worker_Setup.exe` | العمال — ويندوز 10/11 |
| `TajirDZ_X.Y.Z_Windows_Legacy_Worker_Setup.exe` | العمال — ويندوز 7/8/8.1 |
| `TajirDZ_X.Y.Z_Android.apk` | الهاتف (تنزيل يدوي حاليًا) |

البرنامج يختار تلقائيًا حسب نوع التطبيق (مضيف/عامل) ونظام الويندوز —
أنت لا تحدد لمن شيء، فقط ارفع الحزم الخمس.

## خطوات النشر — بالمتصفح فقط

### الخطوة 1: ارفع الحزم

1. افتح صفحة Releases في هذا المستودع.
2. اضغط **Draft a new release**.
3. في **Choose a tag** اكتب تاغ جديد بالشكل `v1.1.38` (بدون v لن يعرض الإصدار) ثم اضغط **Create a new tag on publish**.
4. اكتب العنوان ووصف التحديث.
5. **اسحب ملفات الحزم الخمسة** إلى منطقة الإرفاق وانتظر اكتمال رفع كل واحد.
6. اضغط **Publish release**.

### الخطوة 2: احصل على بصمات SHA256

انسخ هذا السطر كاملًا في نافذة PowerShell (غيّر رقم الإصدار إن لزم) —
سيطبع البصمة لكل ملف على سطح المكتب أو في مجلد البناء:

```powershell
Get-ChildItem "D:\my\workspace1\tajirdz-prod\src-tauri\target\release\bundle\nsis\TajirDZ_*_Setup.exe", "D:\my\workspace1\tajirdz-prod\android\app\build\outputs\apk\debug\app-debug.apk" | ForEach-Object { "$((Get-FileHash $_.FullName -Algorithm SHA256).Hash.ToLower())  $($_.Name)  $($_.Length)" }
```

المخرج: سطر لكل ملف بالشكل `البصمة  الاسم  الحجم-بالبايت`.

### الخطوة 3: حدّث version.json

1. افتح [version.json](https://github.com/ZAKI7543/TajirDZ-Updates/blob/main/version.json) في هذا المستودع.
2. اضغط أيقونة **القلم (Edit)**.
3. انسخ القالب أدناه وعدّل: رقم الإصدار في `host.version` و`worker.version` و`android.version`، ووصف التحديث، والبصمات من الخطوة 2.
4. اضغط **Commit changes**.

**انتهى.** خلال ساعة كحد أقصى (أو فور إعادة فتح البرنامج) يرى كل العملاء التحديث.

## قالب version.json

```json
{
  "releaseStatus": "active",
  "releaseDate": "2026-01-01",
  "host": {
    "version": "1.1.38",
    "changelog": "اكتب هنا ما الجديد",
    "mandatory": false,
    "showNotification": true,
    "requiresMigration": false,
    "restartRequired": true,
    "allowPauseResume": true,
    "compatibility": {
      "windows": {
        "modern": { "enabled": true, "downloadUrl": "https://github.com/ZAKI7543/TajirDZ-Updates/releases/download/v1.1.38/TajirDZ_1.1.38_Windows_Modern_Setup.exe", "sha256": "بصمة-الحزمة-هنا", "size": "640417913" },
        "legacy": { "enabled": true, "downloadUrl": "https://github.com/ZAKI7543/TajirDZ-Updates/releases/download/v1.1.38/TajirDZ_1.1.38_Windows_Legacy_Setup.exe", "sha256": "بصمة-الحزمة-هنا", "size": "637966889" }
      }
    }
  },
  "worker": {
    "version": "1.1.38",
    "changelog": "اكتب هنا ما الجديد",
    "mandatory": false,
    "showNotification": true,
    "restartRequired": true,
    "allowPauseResume": true,
    "compatibility": {
      "windows": {
        "modern": { "enabled": true, "downloadUrl": "https://github.com/ZAKI7543/TajirDZ-Updates/releases/download/v1.1.38/TajirDZ_1.1.38_Windows_Modern_Worker_Setup.exe", "sha256": "بصمة-الحزمة-هنا", "size": "0" },
        "legacy": { "enabled": true, "downloadUrl": "https://github.com/ZAKI7543/TajirDZ-Updates/releases/download/v1.1.38/TajirDZ_1.1.38_Windows_Legacy_Worker_Setup.exe", "sha256": "بصمة-الحزمة-هنا", "size": "0" }
      }
    }
  },
  "android": {
    "version": "1.1.38",
    "downloadUrl": "https://github.com/ZAKI7543/TajirDZ-Updates/releases/download/v1.1.38/TajirDZ_1.1.38_Android.apk",
    "sha256": "بصمة-الملف-هنا",
    "size": "0"
  }
}
```

ملاحظات على القالب:

- `size` = الحجم بالبايت كما طبعته الخطوة 2 (الرقم الثالث في كل سطر).
- `releaseStatus` بقيمة `"blocked"` تسحب إصدارًا معطوبًا فورًا من كل العملاء (بدل رفع إصلاح).
- `mandatory: true` يجعل التحديث إجباريًا — نافذة لا تُغلق حتى التثبيت.
- لا تحذف الإصدارات القديمة من Releases — هي شبكة أمان للرجوع.
- لا تضع أسرارًا أو مفاتيحًا في هذا المستودع — هو عام.

## كيف يحدث الاستبدال دون فقدان البيانات

حزمة التحديث هي المثبّت الكامل. عند تثبيتها صامتًا:

1. يقف برنامج التحقق أي حزمة على نظام ويندوز خاطئ قبل أن تلمس شيئًا.
2. تُوقف قاعدة البيانات إيقافًا نظيفًا ثم كل مكونات البرنامج.
3. تُستبدل ملفات البرنامج فقط (`TajirDZ` للمضيف، `TajirDZ Worker` للعمال).
4. **لا تُلمس إطلاقًا**: بيانات المتجر `TajirDZ-Data` (قاعدة البيانات/المخزون/الصور/النسخ الاحتياطية) وذاكرة العرض المؤقتة `com.tajirdz.*`.
5. تُعاد قاعدة البيانات والخادم للتشغيل تلقائيًا.
6. قبل كل تثبيت تُنشأ نسخة احتياطية كاملة من قاعدة البيانات — وفشلها يلغي التحديث.
