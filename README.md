

# 🤖 JanitorAI to Google Gemini Proxy (Render Ready)

[![Python](https://img.shields.io/badge/Python-3.x-blue.svg)](https://www.python.org/)
[![Flask](https://img.shields.io/badge/Flask-2.x-black.svg)](https://flask.palletsprojects.com/)
[![Render Ready](https://img.shields.io/badge/Render-Ready-46E3B7.svg)](https://render.com/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

**[🇸🇦 العربية](#العربية) | [🇬🇧 English](#english) | [🇪🇸 Español](#español)**

<hr>

<h2 id="العربية">العربية</h2>
<div dir="rtl">

هذا المشروع عبارة عن خادم وسيط (Proxy Server) مبني بلغة Python باستخدام إطار عمل Flask. وظيفته هي تحويل طلبات واجهة برمجة التطبيقات (API) الخاصة بـ **JanitorAI** إلى صيغة تتوافق مع **Google AI Studio (Gemini)**.

يسمح لك هذا المشروع باستخدام نماذج جوجل القوية داخل JanitorAI مجاناً، مع تفعيل ميزة "التفكير" (Thinking) وتجاوب قيود المحتوى (NSFW) عبر تقنيات متقدمة. يتضمن المشروع الآن **واجهة تحكم ويب** تدعم تعدد اللغات وتتيح لك مراجعة السجلات.

---

### ✨ المميزات
- **واجهة تحكم ويب (Control Panel):** واجهة رسومية لتغيير الإعدادات بسهولة دون لمس الكود.
- **تعدد اللغات (i18n):** تدعم العربية، الإنجليزية، والإسبانية (تكتشف لغة المتصفح تلقائياً).
- **نظام سجلات (Black Box Logger):** يسجل الردود الخام من جوجل والردود النهائية لـ JanitorAI لسهولة استكشاف الأخطاء.
- **الوضع الآمن والكلاسيكي:** خيار التبديل بين `systemInstruction` الآمن، أو الحقن الكلاسيكي القديم.
- **توافق كامل مع JanitorAI:** يعمل كـ OpenAI Reverse Proxy بسلاسة ودعم البث (Streaming).

---

### 🚀 طريقة النشر على Render

#### المتطلبات الأساسية
- حساب على [GitHub](https://github.com/) و [Render](https://render.com/).
- مفتاح API من [Google AI Studio](https://aistudio.google.com/).

#### الخطوة 1: رفع الملفات إلى GitHub
قم بإنشاء مستودع وارفع الملفات بهذا الهيكل:
```text
├── app.py
├── requirements.txt
├── translations.json
└── templates/
    └── index.html
```

#### الخطوة 2: إنشاء خدمة على Render
1. اذهب إلى [لوحة تحكم Render](https://dashboard.render.com/) واختر **New +** -> **Web Service**.
2. اربط مستودع GitHub الخاص بك.
3. املأ الإعدادات:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT --timeout 300 --workers 1`
   - **Instance Type:** Free

#### الخطوة 3: إضافة متغيرات البيئة (Environment Variables)
أضف هذه المتغيرات (يمكنك أيضاً تغييرها لاحقاً من واجهة التحكم):

| Key | Value | الوصف |
| :--- | :--- | :--- |
| `MODEL` | `gemini-2.5-flash` | اسم النموذج. |
| `TEMPERATURE` | `1.05` | درجة الإبداع. |
| `ENABLE_NSFW` | `true` | تفعيل تجاوز قيود المحتوى. |
| `ENABLE_THINKING` | `true` | تفعيل وضع التفكير. |
| `USE_CLASSIC_MODE` | `false` | تفعيل الوضع الكلاسيكي (قد يسبب خطأ 400). |

> [!NOTE]
> لا تضع مفتاح Google API هنا. سنستخدمه لاحقاً في JanitorAI.

#### الخطوة 4: إنشاء الخدمة
اضغط **Create Web Service**. ستحصل على رابط مثل: `https://your-project.onrender.com`.

---

### 🔗 ربط Proxy مع JanitorAI
1. افتح إعدادات الـ API في JanitorAI واختر **OpenAI Reverse Proxy**.
2. ضع رابط Render متبوعاً بـ `/v1/chat/completions`.
3. ضع مفتاح Google AI Studio الخاص بك في خانة API Key.
4. احفظ واضغط **Validate API**.

---

### 🛠️ استكشاف الأخطاء وإصلاحها
<details>
<summary><b>انقر للعرض</b></summary>

- **ظهور رسالة فارغة في JanitorAI:**
  اذهب إلى واجهة التحكم الخاصة بك على Render، وحمّل ملف السجلات (Download Logs). ستجد الرد الخام من جوجل لتعرفة إن كان قد تم حظره أم أنه مشكلة من السكربت.
- **خطأ 400 Bad Request:**
  يحدث عادة عند تفعيل "الوضع الكلاسيكي" مع النماذج الجديدة. قم بإيقافه من واجهة التحكم.
- **السبات (Sleep Mode):** الباقة المجانية في Render تنام بعد 15 دقيقة. أول رسالة تستغرق 30 ثانية لإيقاظها.

</details>

</div>

<hr>

<h2 id="english">English</h2>
<div dir="ltr">

This project is a Proxy Server built with Python (Flask). It converts **JanitorAI** API requests (OpenAI format) to be compatible with **Google AI Studio (Gemini)**.

It allows you to use powerful Google models in JanitorAI for free, with "Thinking" mode and NSFW bypass capabilities. The project now includes a **Web Control Panel** that supports multiple languages and allows you to review logs.

---

### ✨ Features
- **Web Control Panel:** A graphical UI to easily change settings without touching the code.
- **Multi-language (i18n):** Supports Arabic, English, and Spanish (auto-detects browser language).
- **Black Box Logger:** Logs raw Google responses and final JanitorAI text for easy debugging.
- **Safe & Classic Modes:** Toggle between safe `systemInstruction` or the old classic injection method.
- **Full JanitorAI Compatibility:** Works seamlessly as an OpenAI Reverse Proxy with Streaming support.

---

### 🚀 Deployment on Render

#### Prerequisites
- Accounts on [GitHub](https://github.com/) and [Render](https://render.com/).
- An API key from [Google AI Studio](https://aistudio.google.com/).

#### Step 1: Upload files to GitHub
Create a repository and upload files in this structure:
```text
├── app.py
├── requirements.txt
├── translations.json
└── templates/
    └── index.html
```

#### Step 2: Create Render Service
1. Go to [Render Dashboard](https://dashboard.render.com/) and click **New +** -> **Web Service**.
2. Connect your GitHub repository.
3. Fill in settings:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT --timeout 300 --workers 1`
   - **Instance Type:** Free

#### Step 3: Environment Variables
Add these variables (can also be changed later via the Control Panel):

| Key | Value | Description |
| :--- | :--- | :--- |
| `MODEL` | `gemini-2.5-flash` | Google model name. |
| `TEMPERATURE` | `1.05` | Creativity level. |
| `ENABLE_NSFW` | `true` | Enable NSFW content bypass. |
| `ENABLE_THINKING` | `true` | Enable thinking mode. |
| `USE_CLASSIC_MODE` | `false` | Enable classic mode (may cause 400 error). |

> [!NOTE]
> Do NOT put your Google API key here. We will use it in JanitorAI.

#### Step 4: Create Service
Click **Create Web Service**. You will get a URL like: `https://your-project.onrender.com`.

---

### 🔗 Connect to JanitorAI
1. Open API settings in JanitorAI and select **OpenAI Reverse Proxy**.
2. Enter your Render URL followed by `/v1/chat/completions`.
3. Enter your Google AI Studio API key in the API Key field.
4. Save and click **Validate API**.

---

### 🛠️ Troubleshooting
<details>
<summary><b>Click to expand</b></summary>

- **Empty message in JanitorAI:**
  Go to your Control Panel on Render and download the Logs file. You will see the raw Google response to determine if it was blocked or if it's a script issue.
- **400 Bad Request:**
  Usually happens when "Classic Mode" is enabled with new models. Turn it off from the Control Panel.
- **Sleep Mode:** Render's free tier sleeps after 15 mins of inactivity. The first message takes ~30 seconds to wake it up.

</details>

</div>

<hr>

<h2 id="español">Español</h2>
<div dir="ltr">

Este proyecto es un Servidor Proxy construido con Python (Flask). Convierte las solicitudes del API de **JanitorAI** (formato OpenAI) para que sean compatibles con **Google AI Studio (Gemini)**.

Te permite usar los potentes modelos de Google en JanitorAI gratis, con el modo de "Pensamiento" (Thinking) y capacidades para eludir el filtro NSFW. El proyecto ahora incluye un **Panel de Control Web** que admite varios idiomas y te permite revisar los registros.

---

### ✨ Características
- **Panel de Control Web:** Una interfaz gráfica para cambiar la configuración fácilmente sin tocar el código.
- **Multi-idioma (i18n):** Admite árabe, inglés y español (detecta automáticamente el idioma del navegador).
- **Registro de Caja Negra:** Guarda las respuestas sin procesar de Google y el texto final de JanitorAI para una fácil depuración.
- **Modos Seguro y Clásico:** Alterna entre el `systemInstruction` seguro o el método de inyección clásico.
- **Compatibilidad total con JanitorAI:** Funciona perfectamente como un OpenAI Reverse Proxy con soporte de Streaming.

---

### 🚀 Despliegue en Render

#### Requisitos previos
- Cuentas en [GitHub](https://github.com/) y [Render](https://render.com/).
- Una clave API de [Google AI Studio](https://aistudio.google.com/).

#### Paso 1: Subir archivos a GitHub
Crea un repositorio y sube los archivos en esta estructura:
```text
├── app.py
├── requirements.txt
├── translations.json
└── templates/
    └── index.html
```

#### Paso 2: Crear servicio en Render
1. Ve al [Panel de control de Render](https://dashboard.render.com/) y haz clic en **New +** -> **Web Service**.
2. Conecta tu repositorio de GitHub.
3. Completa la configuración:
   - **Runtime:** Python 3
   - **Build Command:** `pip install -r requirements.txt`
   - **Start Command:** `gunicorn app:app --bind 0.0.0.0:$PORT --timeout 300 --workers 1`
   - **Instance Type:** Free

#### Paso 3: Variables de Entorno
Añade estas variables (también se pueden cambiar luego desde el Panel de Control):

| Key | Value | Descripción |
| :--- | :--- | :--- |
| `MODEL` | `gemini-2.5-flash` | Nombre del modelo de Google. |
| `TEMPERATURE` | `1.05` | Nivel de creatividad. |
| `ENABLE_NSFW` | `true` | Habilitar omisión de contenido NSFW. |
| `ENABLE_THINKING` | `true` | Habilitar modo de pensamiento. |
| `USE_CLASSIC_MODE` | `false` | Habilitar modo clásico (puede causar error 400). |

> [!NOTE]
> NO pongas tu clave API de Google aquí. La usaremos más tarde en JanitorAI.

#### Paso 4: Crear Servicio
Haz clic en **Create Web Service**. Obtendrás una URL como: `https://your-project.onrender.com`.

---

### 🔗 Conectar a JanitorAI
1. Abre la configuración del API en JanitorAI y selecciona **OpenAI Reverse Proxy**.
2. Ingresa tu URL de Render seguida de `/v1/chat/completions`.
3. Ingresa tu clave API de Google AI Studio en el campo API Key.
4. Guarda y haz clic en **Validate API**.

---

### 🛠️ Solución de problemas
<details>
<summary><b>Haz clic para expandir</b></summary>

- **Mensaje vacío en JanitorAI:**
  Ve a tu Panel de Control en Render y descarga el archivo de Registros (Logs). Verás la respuesta sin procesar de Google para determinar si fue bloqueada o si es un problema del script.
- **400 Bad Request:**
  Suele ocurrir cuando el "Modo Clásico" está activado con modelos nuevos. Desactívalo desde el Panel de Control.
- **Modo de reposo (Sleep):** El nivel gratuito de Render se duerme después de 15 minutos de inactividad. El primer mensaje tarda unos 30 segundos en despertarlo.

</details>

</div>
```

