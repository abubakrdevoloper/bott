# TELESHOP — Premium Smartphone E-Commerce Platform

TELESHOP is a production-ready, multilingual (**Uzbek**, **English**, **Russian**) smartphone e-commerce platform built with semantic HTML5, modern CSS3 (Light & Dark themes), Vanilla JavaScript, and Vercel Serverless Functions with secure **Telegram Bot** order notifications.

## Key Features

- **280+ Real Smartphone Models** across 20 brands (`Apple`, `Samsung`, `Xiaomi`, `Redmi`, `POCO`, `Google`, `OnePlus`, `Huawei`, `Honor`, `OPPO`, `Vivo`, `Realme`, `Motorola`, `Nothing`, `Sony`, `ASUS`, `ZTE`, `Tecno`, `Infinix`, `Nokia`) with authentic model-matched product photography stored in `/public/images/phones/`.
- **Complete 3-Language Support (O‘zbekcha / English / Русский)** across every visible interface element, product description, filter, notification, checkout, and error message.
- **Dark Mode & Light Mode** persisted in `localStorage` with smooth CSS variable transitions.
- **Advanced Search & Multi-Filter Engine**: Live search suggestions, filtering by Brand, Price Range, RAM, Storage, Screen Size, Camera, Battery, Discount, and Stock Availability, plus 5 sorting modes.
- **Interactive Shopping Cart, Wishlist & 3-Phone Comparison**: Stock-aware quantity controls, storage variant pricing, subtotal/discount/total calculation in both **USD** and **UZS**, persisted in `localStorage`.
- **Secure Telegram Bot Order Integration (`POST /api/order`)**:
  - Server-side price & stock verification against `api/catalog-data.json` (never trusts browser-sent prices).
  - HTML-safe Telegram message formatting, payload size limits, input validation & sanitization, and sliding-window rate limiting.
  - Bot token and chat ID stay strictly inside Vercel Environment Variables.

---

## Project Structure

```text
/api
  catalog-data.json     # Authoritative server-side catalog for price/stock verification
  order.js              # Vercel Serverless Function (POST /api/order)
/public
  images/
    phones/             # Verified original smartphone images
    placeholder-phone.svg
/src
  css/
    style.css
  js/
    translations.js     # Complete UZ / EN / RU dictionary
    products.js         # 280+ smartphone catalog
    cart.js             # Cart state & drawer
    checkout.js         # Order validation & /api/order submission
    app.js              # Storefront, filters, modals, compare, wishlist, auth
index.html
package.json
vercel.json
.env.example
.gitignore
README.md
```

---

## Telegram Bot Setup Guide

Follow these steps to receive every confirmed TELESHOP order directly in your Telegram chat:

### STEP 1
Open Telegram and search for **@BotFather** (the official verified Telegram bot creator).

### STEP 2
Start a chat with @BotFather and send the command:
```text
/newbot
```

### STEP 3
Choose a display name for your bot (for example, `TELESHOP Orders`) and a unique username ending in `bot` (for example, `teleshop_orders_bot`).

### STEP 4
Copy the generated **Bot Token** provided by @BotFather (it looks like `1234567890:AAH...`).
> **Security Warning:** Never commit your real Bot Token to GitHub or place it in any frontend file (`index.html`, `app.js`, `products.js`, etc.).

### STEP 5
Open your newly created bot in Telegram using the link from @BotFather and send `/start` so Telegram allows the bot to message you.

### STEP 6
Find your administrator **Chat ID** using a trusted method:
- Option A: Open `https://api.telegram.org/bot<YOUR_BOT_TOKEN>/getUpdates` in your browser right after sending `/start` to your bot, and copy the `"chat":{"id": 123456789}` number.
- Option B: Message `@userinfobot` on Telegram to view your numeric user ID (or add your bot to a private admin group and use the negative group chat ID, e.g. `-1001234567890`).

### STEP 7
Open your **Vercel** dashboard and select your TELESHOP project.

### STEP 8
Go to:
**Project Settings → Environment Variables**

### STEP 9
Add the following two environment variables (for Production, Preview, and Development):
```text
TELEGRAM_BOT_TOKEN = your actual bot token
TELEGRAM_CHAT_ID   = your actual chat ID
```

### STEP 10
Save the environment variables and **redeploy** the project in Vercel (**Deployments → Redeploy**). Redeployment is required whenever environment variables are added or changed so the serverless function picks up the updated values.

### STEP 11
Open your deployed TELESHOP website, add one or more smartphones to the cart, complete the checkout form, and click **Confirm Order**. Verify that the formatted order notification arrives in your Telegram chat with the matching Order ID.

---

## GitHub + Vercel Deployment Guide

1. **Create a GitHub repository** on [github.com](https://github.com) (e.g., `teleshop-ecommerce`).
2. **Upload / Push the project** to GitHub (`.gitignore` automatically excludes `.env` and local secrets):
   ```bash
   git init
   git add .
   git commit -m "Initial TELESHOP e-commerce platform"
   git branch -M main
   git remote add origin https://github.com/<your-username>/teleshop-ecommerce.git
   git push -u origin main
   ```
3. **Open Vercel** at [vercel.com](https://vercel.com) and click **Add New → Project**.
4. **Import the GitHub repository** (`teleshop-ecommerce`). Keep Framework Preset as **Other** (no build command needed).
5. **Configure Environment Variables** (`TELEGRAM_BOT_TOKEN` and `TELEGRAM_CHAT_ID`) before clicking **Deploy** (or add them in Project Settings and redeploy).
6. **Deploy the website** and wait for Vercel to assign your `.vercel.app` URL.
7. **Test the live website**: Check theme switching, 3-language translation, search/filters, cart persistence, wishlist, comparison, and mobile layout.
8. **Test the Telegram order notification**: Submit a test order at checkout and confirm the Telegram bot delivers the full itemized order message.

---

## Local Development

Copy `.env.example` to `.env.local` and run with the Vercel CLI:
```bash
cp .env.example .env.local
# Fill in TELEGRAM_BOT_TOKEN and TELEGRAM_CHAT_ID inside .env.local
npx vercel dev
```
