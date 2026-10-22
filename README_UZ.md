# Daemon — Render uchun Telegram AI bot

## Xavfsizlik
Avval chatda oshkor bo'lgan Telegram tokenini BotFather `/revoke` bilan bekor qiling. Yangi tokenni hech kimga yubormang. `.env` faylini GitHub'ga yuklamang.

## 1. GitHub
Ushbu papkadagi `webhook_app.py`, `requirements.txt`, `render.yaml`, `.gitignore`, `.env.example`, `setup_webhook.py` fayllarini yangi **private** GitHub repository'ga yuklang. `.env` yuklanmasin.

## 2. Render
Render Dashboard -> New -> Blueprint -> GitHub repository'ni ulang -> Deploy Blueprint. `render.yaml` Free web service yaratadi. Render so'ragan TELEGRAM_BOT_TOKEN, GEMINI_API_KEY, ADMIN_USER_ID ni kiriting. WEBHOOK_SECRET avtomatik yaratiladi. Deploy tugagach `https://...onrender.com/` ochilganda `status: ok` bo'lishi kerak.

Agar Blueprint opsiyasi ko'rinmasa: New -> Web Service -> GitHub repo -> Runtime Python -> Build `pip install -r requirements.txt` -> Start `gunicorn --bind 0.0.0.0:$PORT --workers 1 --threads 4 --timeout 120 webhook_app:app` -> Instance Free. Environment bo'limida `.env.example` dagi qiymatlarni kiriting. WEBHOOK_SECRET uchun uzun tasodifiy maxfiy matn ishlating.

## 3. Telegram webhook'ni Render'ga o'tkazish
BotFather'dan yangilangan token va Render'dagi WEBHOOK_SECRET kerak. Token/secretni faqat shaxsiy kompyuteringizdagi `.env` faylga kiriting. `RENDER_URL=https://sizning-servisingiz.onrender.com` qatorini qo'shing. `pip install -r requirements.txt` va `python setup_webhook.py` ni kompyuterda bajaring. `Webhook set: True` chiqishi kerak. Shu vaqtdan boshlab Telegram yangi serverga xabar yuboradi; eski PythonAnywhere web app'ni keyin o'chirishingiz mumkin.

## 4. Guruh
BotFather -> /setprivacy -> Disable. Guruhga botni qo'shing; `Daemon, salom`, `@DaemonDragonAI_bot salom`, `/help@DaemonDragonAI_bot` deb sinang. Rhaenyra belgilash: bot admini guruhdagi odamning xabariga Reply -> `/rhaenyra`.

## Cheklovlar
- Render Free 15 daqiqa trafik bo'lmasa uxlab qolishi mumkin; dastlabki javob sekinlashadi.
- Render Free disk doimiy emas: SQLite xabar tarixi, Rhaenyra va kunlik hisob restart/deployda yo'qolishi mumkin. Muhim xotira uchun keyin tashqi PostgreSQL kabi bazaga o'ting.
- Bot webhook'ga tez 200 qaytaradi, AI fon oqimida ishlaydi; server uxlab qolsa yoki qayta ishga tushsa ayrim javoblar yo'qolishi mumkin. Bu 24/7 kafolatlangan tizim emas.
- Bot oxirgi 30 xabarni AI'ga kontekst sifatida yuboradi va SQLite'ga so'nggi 300 xabarni saqlaydi. Guruh a'zolariga oldindan ayting.
- Gemini API va hostingning bepul limitlari o'zgarishi mumkin.
