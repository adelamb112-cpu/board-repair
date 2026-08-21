# 🔧 تعمیرکار برد کامل

ابزار وب برای عیب‌یابی بردهای الکترونیکی (تلفن، آیفون تصویری، دوربین مداربسته، لوازم خانگی) + تحلیل عکس با Grok + **آموزش کامل تست قطعات الکترونیکی با نمایش ظاهری قطعات و ترفندهای حرفه‌ای**.

**آدرس آنلاین:** https://adelamb112-cpu.github.io/board-repair/

### امکانات آموزش قطعات
- نمایش ظاهری مقاومت، خازن، دیود، ترانزیستور، ماسفت (معمولی + SMD)
- تست گرم و سرد
- محل دقیق پروب قرمز و مشکی
- ترفندهای تعمیرکاران (ESR، بلند کردن پایه، کدهای SMD و ...)

---

## 📱 ساخت اپلیکیشن اندروید

دو روش ساده وجود دارد:

### روش ۱: PWA (ساده‌ترین – بدون نیاز به کدنویسی)

سایت به صورت Progressive Web App آماده شده است.

**روی گوشی اندروید:**
1. سایت را با **Chrome** باز کنید
2. منوی سه‌نقطه → **Add to Home screen** یا **Install app**
3. اپ روی صفحه اصلی نصب می‌شود و مثل اپ واقعی کار می‌کند (تمام صفحه، بدون نوار آدرس)

> این روش رایگان، سریع و بدون نیاز به Android Studio است.

---

### روش ۲: ساخت APK واقعی با Capacitor (توصیه‌شده برای انتشار)

با این روش یک فایل `.apk` واقعی می‌سازید که می‌توانید آن را روی هر اندرویدی نصب کنید یا در گوگل‌پلی منتشر کنید.

#### پیش‌نیازها
- Node.js (نسخه ۱۸ یا بالاتر) → [دانلود](https://nodejs.org)
- Android Studio → [دانلود](https://developer.android.com/studio)
- یک کامپیوتر ویندوز / مک / لینوکس

#### مراحل ساخت APK

```bash
# ۱. کلون کردن پروژه
git clone https://github.com/adelamb112-cpu/board-repair.git
cd board-repair

# ۲. ساخت پوشه وب (چون پروژه فقط یک index.html دارد)
mkdir www
cp index.html manifest.json sw.js www/
# اگر آیکون دارید:
# cp icon-192.png icon-512.png www/

# ۳. مقداردهی اولیه npm
npm init -y

# ۴. نصب Capacitor
npm install @capacitor/core @capacitor/cli @capacitor/android

# ۵. مقداردهی اولیه Capacitor
npx cap init "تعمیرکار برد" com.adelamb.boardrepair --web-dir=www

# ۶. اضافه کردن پلتفرم اندروید
npx cap add android

# ۷. همگام‌سازی فایل‌های وب با پروژه اندروید
npx cap sync

# ۸. باز کردن پروژه در Android Studio
npx cap open android
```

#### ساخت فایل APK داخل Android Studio

1. صبر کنید تا Gradle کامل sync شود
2. از منوی بالا: **Build → Build Bundle(s) / APK(s) → Build APK(s)**
3. بعد از اتمام، روی **locate** کلیک کنید
4. فایل آماده است:
   ```
   android/app/build/outputs/apk/debug/app-debug.apk
   ```

این فایل را می‌توانید مستقیم روی گوشی نصب کنید.

#### ساخت نسخه Release (برای گوگل‌پلی)

1. **Build → Generate Signed Bundle / APK**
2. یک Keystore بسازید (رمز را فراموش نکنید)
3. نوع **Android App Bundle (.aab)** یا **APK** را انتخاب کنید
4. فایل نهایی در پوشه `release` ساخته می‌شود

---

### تنظیمات پیشنهادی در `capacitor.config.json`

بعد از `npx cap init` فایل `capacitor.config.json` را باز کنید و این محتوا را قرار دهید:

```json
{
  "appId": "com.adelamb.boardrepair",
  "appName": "تعمیرکار برد",
  "webDir": "www",
  "server": {
    "androidScheme": "https"
  },
  "android": {
    "allowMixedContent": true
  }
}
```

سپس دوباره اجرا کنید:
```bash
npx cap sync
```

---

## 🖼️ آیکون اپ

برای PWA و APK بهتر است دو آیکون داشته باشید:

- `icon-192.png` (۱۹۲×۱۹۲)
- `icon-512.png` (۵۱۲×۵۱۲)

می‌توانید با ابزارهای آنلاین مثل [PWA Asset Generator](https://www.pwabuilder.com/) یا [RealFaviconGenerator](https://realfavicongenerator.net/) بسازید و در ریشه پروژه قرار دهید.

---

## 📂 ساختار فایل‌ها

```
board-repair/
├── index.html          # اپ اصلی
├── manifest.json       # تنظیمات PWA
├── sw.js               # Service Worker (آفلاین)
├── README.md           # این فایل
└── (اختیاری) icon-192.png / icon-512.png
```

---

## 🔄 آپدیت کردن اپ بعد از تغییرات

هر بار که `index.html` را تغییر دادید:

```bash
cp index.html www/
npx cap sync
npx cap open android   # دوباره بیلد کنید
```

---

## 📌 نکات مهم

- برای استفاده از دوربین داخل اپ Capacitor، پلاگین `@capacitor/camera` را نصب کنید (اختیاری).
- API Key مربوط به Grok را کاربر خودش در تنظیمات وارد می‌کند (در localStorage ذخیره می‌شود).
- اگر می‌خواهید اپ بدون اینترنت هم کار کند، Service Worker فعلی کافی است (صفحات اصلی کش می‌شوند).

---

ساخته‌شده با ❤️ برای تعمیرکاران ایرانی
