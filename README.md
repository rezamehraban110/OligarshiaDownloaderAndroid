# Oligarshia Downloader — Android

نسخه‌ی اولیه‌ی اندرویدی دانلودر الیگارشیا، با پردازش محلی روی خود گوشی و بدون بک‌اند.

## امکانات نسخه 0.1

- رابط Native با Kotlin + Jetpack Compose
- فارسی / English و راست‌چین کامل
- تم روشن / تاریک
- ورود چند لینک و صف دانلود ترتیبی
- کیفیت‌های Best، 360p، 480p، 720p، 1080p، 2K، 4K و MP3
- نمایش مرحله، درصد، حجم، سرعت و زمان باقی‌مانده
- Pause / Resume / Cancel
- پردازش محلی با yt-dlp (داخل Python/Chaquopy) و FFmpegKit
- ذخیره پیش‌فرض در `Downloads/Oligarshia`
- انتخاب پوشه دلخواه با Android Storage Access Framework
- تاریخچه‌ی 100 دانلود آخر
- Foreground Service و نوتیفیکیشن دانلود
- ساختار جداشده‌ی Engine / UI برای اضافه‌کردن سرویس‌های بعدی

## نکته مهم درباره YouTube

YouTube مرتباً روش استخراج ویدئو را تغییر می‌دهد. این نسخه `yt-dlp 2026.08.19` را داخل APK قرار می‌دهد و برای اکثر مسیرهای عادی طراحی شده است. بعضی ویدئوها/فرمت‌های جدید ممکن است به JavaScript runtime خارجی برای چالش‌های EJS نیاز داشته باشند. معماری Engine عمداً جدا نوشته شده تا در نسخه بعدی بتوان QuickJS یا موتور جایگزین را بدون بازنویسی UI اضافه کرد.

## معماری

```text
Compose UI
   ↓
HomeViewModel
   ↓
DownloadService (Foreground, sequential queue)
   ↓
DownloadEngine interface
   ↓
YtDlpEngine → Chaquopy/Python → yt-dlp
   ↓
FFmpegProcessor → mux / MP3
   ↓
StorageExporter → MediaStore / SAF folder
```

## پیش‌نیاز ساخت

- Android Studio جدید
- JDK 17
- Android SDK 36
- Python 3.13 روی سیستم build (برای Chaquopy؛ در Windows معمولاً `py -3.13`)
- گوشی arm64-v8a، Android 7.0 (API 24) یا بالاتر

> فعلاً ABI روی `arm64-v8a` محدود شده تا خروجی APK برای گوشی‌های واقعی سبک‌تر و با FFmpegKit سازگارتر باشد.

## باز کردن پروژه

1. پوشه را در Android Studio باز کنید.
2. Gradle Sync را اجرا کنید.
3. اگر Android Studio از Python 3.13 را خودکار پیدا نکرد، در `app/build.gradle.kts` داخل `chaquopy.defaultConfig` مسیر `buildPython(...)` را تعیین کنید.
4. یک گوشی Android با USB Debugging وصل کنید.
5. `Run` یا `Build > Build APK(s)` را اجرا کنید.

خروجی Debug معمولاً اینجاست:

```text
app/build/outputs/apk/debug/app-debug.apk
```

## لایسنس‌های فنی

این پروژه عمداً از `ffmpeg-kit-full` غیر-GPL استفاده می‌کند. خود yt-dlp Unlicense است و Chaquopy تحت MIT عرضه می‌شود. قبل از انتشار تجاری، فایل NOTICE و بررسی نهایی مجوز همه وابستگی‌های transitive انجام شود.

## مرحله بعدی پیشنهادی

- QuickJS داخلی برای سازگاری بیشتر با YouTube EJS
- صفحه Preview اطلاعات و Thumbnail قبل از افزودن به صف
- Share Target اندروید (ارسال لینک از YouTube مستقیم به اپ)
- دانلود Playlist / Shorts
- موتورهای Instagram / Aparat / Pinterest
- Login + subscription entitlement برای نسخه وب/پولی
- Room database برای تاریخچه کامل
- Auto-update امن APK

---

Developer: Reza Mehraban  
A product of Oligarshia
