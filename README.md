🤖 JanitorAI to Google Gemini Proxy (Render Ready)
هذا المشروع عبارة عن خادم وسيط (Proxy Server) مبني بلغة Python (Flask). وظيفته هي تحويل طلبات واجهة برمجة التطبيقات (API) الخاصة بـ JanitorAI (والتي تستخدم صيغة OpenAI) إلى صيغة تتوافق مع Google AI Studio (Gemini).

يسمح لك هذا المشروع باستخدام نماذج جوجل القوية (مثل Gemini 2.5 Pro و Flash) داخل JanitorAI مجاناً، مع تفعيل ميزة "التفكير" (Thinking) وتجاوز قيود المحتوى (NSFW) عبر تقنيات الـ Prompting المتقدمة.

✨ المميزات
توافق كامل مع JanitorAI: يعمل كـ OpenAI Reverse Proxy.
دعم البث (Streaming): يستجيب للرسائل في الوقت الفعلي (SSE).
وضع التفكير (Thinking Mode): يفصل عملية تفكير النموذج عن الرد النهائي باستخدام وسوم <think> و <response>.
تجاوز قيود المحتوى (NSFW Bypass): يحتوي على System Prompts مخصصة لتجاوز رفض النموذج لتوليد المحتوى للبالغين.
إعدادات مرنة: يتم التحكم بكل الإعدادات عبر متغيرات البيئة (Environment Variables) دون الحاجة لتعديل الكود.
جاهز للنشر على Render: يعمل بسلاسة باستخدام Gunicorn.
🚀 طريقة النشر على Render (خطوة بخطوة)
المتطلبات الأساسية:
حساب على GitHub.
حساب على Render.
مفتاح API خاص بـ Google AI Studio (يمكنك الحصول عليه مجاناً من Google AI Studio).
الخطوة 1: رفع الملفات إلى GitHub
قم بإنشاء مستودع جديد على GitHub وارفع فيه ملفات المشروع الثلاثة:

app.py
requirements.txt
render.yaml (اختياري للنشر التلقائي)
README.md
الخطوة 2: إنشاء خدمة جديدة على Render
اذهب إلى لوحة تحكم Render.
اضغط على زر New + واختر Web Service.
اربط حسابك بـ GitHub واختر المستودع الذي يحتوي على ملفات المشروع.
املأ الإعدادات الأساسية كالتالي:
Name: اسم مشروعك (مثال: gemini-janitor-proxy).
Runtime: Python 3.
Build Command: pip install -r requirements.txt
Start Command: gunicorn app:app --bind 0.0.0.0:$PORT --timeout 300 --workers 1
Instance Type: Free (مجاني).
الخطوة 3: إضافة متغيرات البيئة (Environment Variables)
في نفس صفحة الإعدادات على Render، انزل لأسفل إلى قسم Environment، وأضف المتغيرات التالية (يمكنك تعديلها حسب رغبتك):

Key	Value	الوصف
MODEL	gemini-2.5-flash	اسم نموذج جوجل (مثال: gemini-2.5-pro).
TEMPERATURE	1.05	درجة الإبداع (من 0 إلى 2).
ENABLE_NSFW	true	تفعيل تجاوز قيود المحتوى للبالغين.
ENABLE_THINKING	true	تفعيل وضع التفكير وفصل الأفكار عن الرد.
ENABLE_GOOGLE_SEARCH	false	تفعيل أداة البحث في جوجل للنموذج.
TOP_P	0.95	إعداد Top P.
TOP_K	40	إعداد Top K.
MAX_TOKENS	10000	أقصى طول للرد.
ملاحظة: لا تضع مفتاح Google API الخاص بك هنا في Render. سنستخدمه لاحقاً في JanitorAI.

الخطوة 4: إنشاء الخدمة
اضغط على زر Create Web Service في أسفل الصفحة. ستبدأ Render ببناء المشروع (قد يستغرق دقيقتين تقريباً). عند الانتهاء، ستحصل على رابط الخدمة الخاص بك، وسيكون بهذا الشكل:https://gemini-janitor-proxy-xxxx.onrender.com

🔗 ربط Proxy مع JanitorAI
بعد أن يصبح السيرفر يعمل على Render، اتبع الخطوات التالية في JanitorAI:

افتح JanitorAI واذهب إلى إعدادات الـ API (API Settings).
اختر OpenAI Reverse Proxy.
في خانة Reverse Proxy URL، ضع رابط Render الخاص بك متبوعاً بـ /v1/chat/completions.
مثال: https://gemini-janitor-proxy-xxxx.onrender.com/v1/chat/completions
في خانة API Key (Reverse Proxy)، ضع مفتاح Google AI Studio الخاص بك (الذي حصلت عليه من جوجل).
احفظ الإعدادات واضغط على "Validate API". يجب أن تظهر علامة خضراء تفيد بنجاح الاتصال!
⚠️ ملاحظات هامة حول الباقة المجانية في Render
السبات (Sleep Mode): في الباقة المجانية، إذا لم يستقبل السيرفر أي طلب لمدة 15 دقيقة، سيتم إيقافه مؤقتاً (Sleep).
التشغيل التلقائي: عندما ترسل أول رسالة من JanitorAI والسيرفر نائم، سيستغرق حوالي 30 إلى 50 ثانية ليستيقظ. قد تظهر لك رسالة خطأ في JanitorAI، فقط انتظر قليلاً وأعد إرسال الرسالة وستعمل بسرعة.
اختبار الحالة: للتأكد من أن السيرفر يعمل، يمكنك فتح الرابط الأساسي في المتصفح:https://your-render-url.onrender.com/ وستظهر لك حالة السيرفر.
🛠️ استكشاف الأخطاء وإصلاحها
ظهور رسالة "No content received from Google AI":هذا يعني أن جوجل قامت بحظر الرد بسبب المحتوى. تأكد من أن ENABLE_NSFW=true في متغيرات البيئة، وأن النموذج المختار يدعم هذا الوضع.
مشكلة في الـ Streaming (تقطع في الردود):تأكد من أنك تستخدم أحدث إصدار من ملف app.py وأن Start Command على Render تحتوي على --timeout 300.
خطأ 401 Unauthorized:تأكد من أنك وضعت مفتاح Google API بشكل صحيح في JanitorAI وليس في Render.
📜 الترخيص
هذا المشروع مفتوح المصدر ومتاح للاستخدام والتعديل لأي غرض.

