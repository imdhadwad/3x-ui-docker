# 🚀 3x-ui Panel (Sanaei) on Railway

<p align="center">
  <a href="https://t.me/meov2ray">
    <img src="https://img.shields.io/badge/Telegram-MEOV2RAY-26A5E4?style=for-the-badge&logo=telegram&logoColor=white" alt="Telegram Channel" />
  </a>
  &nbsp;
  <a href="https://youtube.com/@meov2ray">
    <img src="https://img.shields.io/badge/YouTube-MEOV2RAY-FF0000?style=for-the-badge&logo=youtube&logoColor=white" alt="YouTube Channel" />
  </a>
</p>

---

[فارسی](#-راهنمای-فارسی) | [English](#-english-guide)

---

## 🇮🇷 راهنمای فارسی

این پروژه امکان نصب و اجرای **تمامی نسخه‌های پنل 3x-ui (سنایی)** را روی سرویس ابری **Railway** فراهم می‌کند. با استفاده از این Dockerfile بهینه‌شده، پنل به‌صورت مستقیم از سورس گیت‌هاب دانلود و اجرا می‌شود.

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/deploy)

### 📢 کانال‌های ما
* 📢 **کانال تلگرام:** [meov2ray@](https://t.me/meov2ray)
* 🎥 **کانال یوتیوب:** [meov2ray@](https://youtube.com/@meov2ray)

### 🚀 مراحل راه‌اندازی سریع
1. این ریپازیتوری را **Fork** کنید.
2. وارد [Railway.app](https://railway.app) شوید و پروژه جدید ایجاد کنید (**Deploy from GitHub repo**).
3. ریپازیتوری Fork شده را متصل کنید.

### ⚙️ تنظیمات ضروری در Railway
1. **دیتابیس دائم (Persistent Volume):**  
   در تب **Volumes** یک Volume جدید با مسیر Mount برابر با `/etc/x-ui` ایجاد کنید تا اطلاعات با ری‌استارت پاک نشوند.
2. **تنظیم پورت (Target Port):**  
   در تب **Settings -> Networking** مقدار **Target Port** را روی `2053` تنظیم کنید و یک دامنه (Domain) تولید کنید.
3. **انتخاب نسخه دلخواه سنایی:**  
   در تب **Variables** متغیر زیر را تعریف کنید (به عنوان مثال برای نسخه v2.5.8 یا v3.8.5):
   * `XUI_VERSION` = `v2.5.8`

### 🔑 اطلاعات ورود پیش‌فرض
* **نام کاربری:** `admin`
* **رمز عبور:** `admin`
*(حتماً پس از اولین ورود، نام کاربری و رمز عبور را تغییر دهید)*

---

## 🇬🇧 English Guide

This repository allows you to deploy **any version of the 3x-ui Panel (Sanaei)** on **Railway** seamlessly. It builds directly from official GitHub releases to avoid Docker Hub access issues.

[![Deploy on Railway](https://railway.app/button.svg)](https://railway.app/template/deploy)

### 📢 Official Channels
* 📢 **Telegram Channel:** [@meov2ray](https://t.me/meov2ray)
* 🎥 **YouTube Channel:** [@meov2ray](https://youtube.com/@meov2ray)

### 🚀 Quick Deployment
1. **Fork** this repository.
2. Log in to [Railway.app](https://railway.app) and create a **New Project -> Deploy from GitHub repo**.
3. Select your forked repository.

### ⚙️ Required Railway Configurations
1. **Persistent Volume:**  
   Go to the **Volumes** tab and add a volume with Mount Path `/etc/x-ui` to preserve panel data across restarts.
2. **Target Port:**  
   Under **Settings -> Networking**, set **Target Port** to `2053` and generate a domain.
3. **Select 3x-ui Version:**  
   In the **Variables** tab, add the environment variable for your preferred version (e.g., `v2.5.8` or `v3.8.5`):
   * `XUI_VERSION` = `v2.5.8`

### 🔑 Default Credentials
* **Username:** `admin`
* **Password:** `admin`
*(Please change default credentials immediately after your first login)*

---

## 📄 Dockerfile Reference

```dockerfile
FROM alpine:3.19

ARG XUI_VERSION=v3.9.0
ARG ARCH=amd64

RUN apk add --no-cache \
    curl \
    bash \
    ca-certificates \
    tzdata \
    sqlite \
    && ln -sf /usr/share/zoneinfo/Asia/Tehran /etc/localtime

RUN curl -L "[https://github.com/mhsanaei/3x-ui/releases/download/$](https://github.com/mhsanaei/3x-ui/releases/download/$){XUI_VERSION}/x-ui-linux-${ARCH}.tar.gz" -o /tmp/x-ui.tar.gz \
    && tar -xzf /tmp/x-ui.tar.gz -C /usr/local/ \
    && rm /tmp/x-ui.tar.gz \
    && chmod +x /usr/local/x-ui/x-ui \
    && chmod +x /usr/local/x-ui/x-ui.sh \
    && chmod +x /usr/local/x-ui/bin/xray-linux-${ARCH}

RUN mkdir -p /etc/x-ui /var/log/x-ui

WORKDIR /usr/local/x-ui

EXPOSE 2053

CMD ["./x-ui"]
