<div align="center">
  <img src="Preview.png" alt="PGClock Fusion Preview" width="900">
</div>

<h1 align="center">PGClock Fusion</h1>

<p align="center">
  صفحهٔ اشتراک سه‌تبی برای Pasarguard — ساعت زنده، نمودار مصرف و برندینگ سفارشی
</p>

<p align="center">
  <a href="README.md">English</a> ·
  <b>فارسی</b>
</p>

<p align="center">
  <a href="#نصب-خودکار">نصب خودکار</a> ·
  <a href="#نصب-دستی">نصب دستی</a> ·
  <a href="#سفارشی‌سازی-برند">سفارشی‌سازی برند</a> ·
  <a href="#تنظیمات-پنل">تنظیمات پنل</a> ·
  <a href="#تغییرات">تغییرات</a>
</p>

---

## ویژگی‌ها

- سه تب مجزا: **اطلاعات حساب** · **سرورها** · **اپلیکیشن‌ها**
- ساعت و تاریخ زندهٔ شمسی/میلادی
- حلقه‌های «روز باقی‌مانده» و «حجم باقی‌مانده» با رنگ وضعیت
- نمودار مصرف روزانه/هفتگی از API پنل
- کارت‌های جزئیات حساب (مصرف کل، آخرین اتصال، آخرین IP، ظرفیت دستگاه و…)
- پشتیبانی از حالت نامحدود (∞)، منقضی و محدود
- کپی، QR و دانلود WireGuard برای هر کانفیگ + پرچم SVG کشور (نمایش یکسان در همهٔ سیستم‌عامل‌ها حتی ویندوز)
- دو زبان **FA / EN** با RTL/LTR خودکار و دو تم **تیره / روشن**
- نام برند، زیرعنوان و لوگوی سفارشی (سازگار با اسکریپت نصب)
- حالت سبک (perf-lite) خودکار روی دستگاه‌های ضعیف
- یک فایل HTML — بدون Node.js و build

---

## نصب خودکار

روی سرور **Ubuntu** با Pasarguard نصب‌شده:

```bash
curl -fsSL https://raw.githubusercontent.com/Pasham0/PGClockFusion/main/install.sh -o /tmp/pgclock-install.sh && sudo bash /tmp/pgclock-install.sh
```

یا:

```bash
wget -qO /tmp/pgclock-install.sh https://raw.githubusercontent.com/Pasham0/PGClockFusion/main/install.sh && sudo bash /tmp/pgclock-install.sh
```

نصاب می‌پرسد که آیا می‌خواهید برند را سفارشی کنید؛ با Enter رد کنید تا برند پیش‌فرض بماند.

### اسکریپت چه کار می‌کند؟

1. (اختیاری) دریافت نام برند، زیرعنوان و لوگو و patch خودکار روی `index.html`
2. ذخیرهٔ قالب در:

```text
/var/lib/pasarguard/templates/subscription/index.html
```

3. به‌روزرسانی `/opt/pasarguard/.env`:

```env
CUSTOM_TEMPLATES_DIRECTORY="/var/lib/pasarguard/templates/"
SUBSCRIPTION_PAGE_TEMPLATE="subscription/index.html"
```

4. اجرای `pasarguard restart`

> **پیش‌نیازها:** `wget`، `curl`، `python3`

---

## نصب دستی

### ۱. دانلود قالب

```bash
sudo mkdir -p /var/lib/pasarguard/templates/subscription/
sudo wget -N -O /var/lib/pasarguard/templates/subscription/index.html \
  https://raw.githubusercontent.com/Pasham0/PGClockFusion/main/index.html
```

### ۲. تنظیم Pasarguard

```bash
sudo nano /opt/pasarguard/.env
```

اضافه یا به‌روز کنید:

```env
CUSTOM_TEMPLATES_DIRECTORY="/var/lib/pasarguard/templates/"
SUBSCRIPTION_PAGE_TEMPLATE="subscription/index.html"
```

### ۳. راه‌اندازی مجدد

```bash
sudo pasarguard restart
```

---

## سفارشی‌سازی برند

اسکریپت نصب به‌صورت خودکار این کار را می‌کند؛ برای ویرایش دستی، ابتدای `index.html` این آبجکت را پیدا کنید:

```javascript
var DEFAULT_BRAND = {
  name: "PGClock Fusion",
  subtitle: { fa: "پنل اشتراک", en: "Subscription panel" },
  logoUrl: ""
};
```

- `name` — نام برند (در هدر نمایش داده می‌شود)
- `subtitle.fa` / `subtitle.en` — زیرعنوان برای هر زبان
- `logoUrl` — آدرس `https://` لوگو؛ خالی باشد آیکن پیش‌فرض نمایش داده می‌شود

**قرارداد قالب (این کلیدها را ثابت نگه دارید):** اسکریپت نصب دنبال `DEFAULT_BRAND` با فیلدهای `name`، `subtitle` و `logoUrl` می‌گردد.

---

## تنظیمات پنل

1. پنل Pasarguard → **Settings → Subscription**
2. ویرایش **announcement** و **announcement link**
3. افزودن/ویرایش اپ‌ها در بخش apps

---

## تغییرات

### v1.3.0

- **کارت اطلاعات حساب تا انتهای ستون کنارش کشیده می‌شود** — قبلاً هرجا محتوایش تمام می‌شد متوقف می‌شد و لبهٔ پایینی ناهموار می‌ماند
- حلقهٔ «روز/حجم باقی‌مانده» در فضای خالی باقی‌مانده وسط‌چین می‌شود؛ عنوان بالا و فوتر پایین ثابت می‌مانند
- فقط دسکتاپ (≥۸۶۰ پیکسل) — موبایل بدون تغییر

### v1.2.0

- **چیدمان دسکتاپ بازطراحی شد**: نوار پایین به یک **تب‌بار سگمنتی در بالای صفحه** تبدیل می‌شود
- عرض حداکثر صفحه در دسکتاپ به ۱۲۸۰ پیکسل رسید
- ستون اطلاعات حساب و ستون نمودار/جزئیات جای‌شان عوض شد (۵fr / ۷fr)
- گریدهای «جزئیات حساب» با `auto-fit` به عرض ستون واکنش می‌دهند، نه به عرض پنجره — دیگر در ۸۶۰ تا ۱۱۸۰ پیکسل مقادیر بریده نمی‌شوند
- گرید سرورها و اپلیکیشن‌ها در ≥۱۱۸۰ پیکسل سه‌ستونه شد
- برچسب مقدار روی میله‌های نمودار در دسکتاپ افقی شد (قبلاً عمودی بود)
- پنجره‌های اپلیکیشن و QR در دسکتاپ وسط صفحه باز می‌شوند، نه از پایین

### v1.1.0

- **پرچم‌های SVG دایره‌ای** به‌جای ایموجی — با [circle-flags](https://github.com/HatScripts/circle-flags)؛ روی ویندوز هم پرچم رنگی و یکسان نمایش داده می‌شود
- حذف فونت polyfill پرچم (Twemoji) — حدود ۶۰۰ کیلوبایت دانلود کمتر
- ایموجی پرچمی که ادمین داخل نام سرور گذاشته باشد به‌صورت خودکار به پرچم SVG تبدیل می‌شود
- شکستن خطوط مقادیر بلند در کارت‌های جزئیات حساب (به‌جای بریده‌شدن)
- گرید جزئیات حساب در دسکتاپ به ۲ ستون برگشت تا مقادیر جا شوند

### v1.0.1

- گریدهای واکنش‌گرا برای دسکتاپ

### v1.0.0

- انتشار اولیه

---

## لایسنس

[MIT](LICENSE)
