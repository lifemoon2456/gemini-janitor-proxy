

<div dir="rtl">

# 🤖 JanitorAI to Google Gemini Proxy (Render Ready)

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-black.svg)](https://flask.palletsprojects.com/)
[![Render Ready](https://img.shields.io/badge/Render-Ready-46E3B7.svg)](https://render.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

هذا المشروع عبارة عن خادم وسيط (Proxy Server) مبني بلغة Python باستخدام إطار عمل Flask. وظيفته هي تحويل طلبات واجهة برمجة التطبيقات (API) الخاصة بـ **JanitorAI** (والتي تستخدم صيغة OpenAI) إلى صيغة تتوافق مع **Google AI Studio (Gemini)**.

يسمح لك هذا المشروع باستخدام نماذج جوجل القوية (مثل Gemini 1.5 Pro و Flash) داخل JanitorAI مجاناً، مع تفعيل ميزة "التفكير" (Thinking) وتجاوز قيود المحتوى (NSFW) عبر تقنيات الـ Prompting المتقدمة.

---

## 📑 جدول المحتويات
- [✨ المميزات](#-المميزات)
- [🚀 طريقة النشر على Render](#-طريقة-النشر-على-render)
  - [المتطلبات الأساسية](#المتطلبات-الأساسية)
  - [الخطوة 1: رفع الملفات إلى GitHub](#الخطوة-1-رفع-الملفات-إلى-github)
  - [الخطوة 2: إنشاء خدمة جديدة على Render](#الخطوة-2-إنشاء-خدمة-جديدة-على-render)
  - [الخطوة 3: إضافة متغيرات البيئة](#الخطوة-3-إضافة-متغيرات-البيئة-environment-variables)
  - [الخطوة 4: إنشاء الخدمة](#الخطوة-4-إنشاء-الخدمة)
- [🔗 ربط Proxy مع JanitorAI](#-ربط-proxy-مع-janitorai)
- [⚠️ ملاحظات هامة حول الباقة المجانية في Render](#️-ملاحظات-هامة-حول-الباقة-المجانية-في-render)
- [🛠️ استكشاف الأخطاء وإصلاحها](#️-استكشاف-الأخطاء-وإصلاحها)
- [📜 الترخيص](#-الترخيص)

---

## ✨ المميزات

- **توافق كامل مع JanitorAI:** يعمل كـ OpenAI Reverse Proxy بسلاسة.
- **دعم البث (Streaming):** يستجيب للرسائل في الوقت الفعلي (SSE) لتجربة استخدام واقعية.
- **وضع التفكير (Thinking Mode):** يفصل عملية تفكير النموذج عن الرد النهائي باستخدام وسوم `<think>` و `<response>`.
- **تجاوز قيود المحتوى (NSFW Bypass):** يحتوي على System Prompts مخصصة لتجاوز رفض النموذج لتوليد المحتوى للبالغين.
- **إعدادات مرنة:** يتم التحكم بكل الإعدادات عبر متغيرات البيئة (Environment Variables) دون الحاجة لتعديل الكود.
- **جاهز للنشر على Render:** يعمل بسلاسة باستخدام Gunicorn مع إعدادات محسّنة.

---

## 🚀 طريقة النشر على Render

### المتطلبات الأساسية
- حساب على [GitHub](https://github.com/).
- حساب على [Render](https://render.com/).
- مفتاح API خاص بـ Google AI Studio (يمكنك الحصول عليه مجاناً من [Google AI Studio](https://aistudio.google.com/)).

### الخطوة 1: رفع الملفات إلى GitHub
قم بإنشاء مستودع جديد (Repository) على GitHub وارفع فيه ملفات المشروع الأساسية:
- `app.py`
- `requirements.txt`
- `render.yaml` (اختياري للنشر التلقائي عبر Blueprint)
- `README.md`

### الخطوة 2: إنشاء خدمة جديدة على Render
1. اذهب إلى [لوحة تحكم Render](https://dashboard.render.com/).
2. اضغط على زر **New +** واختر **Web Service**.
3. اربط حسابك بـ GitHub واختر المستودع الذي يحتوي على ملفات المشروع.
4. املأ الإعدادات الأساسية كالتالي:
   - **Name:** اسم مشروعك (مثال: `gemini-janitor-proxy`).
   - **Runtime:** Python 3.
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT --timeout 300 --workers 1`
   - **Instance Type:** Free (مجاني).

### الخطوة 3: إضافة متغيرات البيئة (Environment Variables)
في نفس صفحة الإعدادات على Render، انزل لأسفل إلى قسم **Environment**، وأضف المتغيرات التالية (يمكنك تعديلها حسب رغبتك):

| Key | Value | الوصف |
| :--- | :--- | :--- |
| `MODEL` | `gemini-1.5-flash` | اسم نموذج جوجل (مثال: `gemini-1.5-pro`). |
| `TEMPERATURE` | `1.05` | درجة الإبداع (من 0 إلى 2). |
| `ENABLE_NSFW` | `true` | تفعيل تجاوز قيود المحتوى للبالغين. |
| `ENABLE_THINKING` | `true` | تفعيل وضع التفكير وفصل الأفكار عن الرد. |
| `ENABLE_GOOGLE_SEARCH` | `false` | تفعيل أداة البحث في جوجل للنموذج. |
| `TOP_P` | `0.95` | إعداد Top P. |
| `TOP_K` | `40` | إعداد Top K. |
| `MAX_TOKENS` | `10000` | أقصى طول للرد. |

> [!NOTE]
> **لا تضع مفتاح Google API الخاص بك هنا في Render.** سنستخدمه لاحقاً في JanitorAI للحفاظ على خصوصيته.

### الخطوة 4: إنشاء الخدمة
اضغط على زر **Create Web Service** في أسفل الصفحة. ستبدأ Render ببناء المشروع (قد يستغرق دقيقتين تقريباً). عند الانتهاء، ستحصل على رابط الخدمة الخاص بك، وسيكون بهذا الشكل:
```url
https://gemini-janitor-proxy-xxxx.onrender.com
```

---

## 🔗 ربط Proxy مع JanitorAI

بعد أن يصبح السيرفر يعمل على Render، اتبع الخطوات التالية في JanitorAI:

1. افتح JanitorAI واذهب إلى إعدادات الـ API (**API Settings**).
2. اختر **OpenAI Reverse Proxy**.
3. في خانة **Reverse Proxy URL**، ضع رابط Render الخاص بك متبوعاً بـ `/v1/chat/completions`.
   ```url
   https://gemini-janitor-proxy-xxxx.onrender.com/v1/chat/completions
   ```
4. في خانة **API Key (Reverse Proxy)**، ضع مفتاح Google AI Studio الخاص بك (الذي حصلت عليه من جوجل).
5. احفظ الإعدادات واضغط على **"Validate API"**. يجب أن تظهر علامة خضراء تفيد بنجاح الاتصال! 🎉

---

## ⚠️ ملاحظات هامة حول الباقة المجانية في Render

> [!WARNING]
> - **السبات (Sleep Mode):** في الباقة المجانية، إذا لم يستقبل السيرفر أي طلب لمدة 15 دقيقة، سيتم إيقافه مؤقتاً (Sleep).
> - **التشغيل التلقائي:** عندما ترسل أول رسالة من JanitorAI والسيرفر نائم، سيستغرق حوالي 30 إلى 50 ثانية ليستيقظ. قد تظهر لك رسالة خطأ في JanitorAI، فقط انتظر قليلاً وأعد إرسال الرسالة وستعمل بسرعة.
> - **اختبار الحالة:** للتأكد من أن السيرفر يعمل، يمكنك فتح الرابط الأساسي في المتصفح: `https://your-render-url.onrender.com/` وستظهر لك حالة السيرفر.

---

## 🛠️ استكشاف الأخطاء وإصلاحها

<details>
<summary><b>Click to expand (انقر للعرض)</b></summary>

- **ظهور رسالة "No content received from Google AI":**
  هذا يعني أن جوجل قامت بحظر الرد بسبب المحتوى. تأكد من أن `ENABLE_NSFW=true` في متغيرات البيئة، وأن النموذج المختار يدعم هذا الوضع.

- **مشكلة في الـ Streaming (تقطع في الردود):**
  تأكد من أنك تستخدم أحدث إصدار من ملف `app.py` وأن `Start Command` على Render تحتوي على `--timeout 300`.

- **خطأ 401 Unauthorized:**
  تأكد من أنك وضعت مفتاح Google API بشكل صحيح في إعدادات JanitorAI وليس في متغيرات بيئة Render.

</details>

---

## 📜 الترخيص

هذا المشروع مفتوح المصدر ومتاح للاستخدام والتعديل لأي غرض تحت رخصة [MIT License](LICENSE).

</div>

4. **التنبيهات (GitHub Alerts):** تم استخدام ميزة التنبيهات الجديدة في جيت هاب `> [!NOTE]` و `> [!WARNING]` لإبراز الملاحظات المهمة.
5. **أقسام قابلة للطي (Collapsible Section):** تم وضع قسم "استكشاف الأخطاء" داخل `<details>` ليكون الملف منظماً.
6. **تنسيق الأكواد والجداول:** تم تحسين تنسيق الأكواد (Code Blocks) والجداول لتكون مريحة للعين وسهلة القراءة والنسخ.
