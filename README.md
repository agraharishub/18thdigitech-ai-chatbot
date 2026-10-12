# AI Commerce Chatbot

The **AI Commerce Chatbot** adds an AI customer support and shopping assistant to Adobe Commerce storefronts (SaaS and
PaaS).

Shoppers can ask about products, get recommendations, manage their cart, track orders and read store policies in a
chat widget. Merchants install it from Adobe Exchange and configure it in **Apps → App Management** in the Commerce
Admin: no code, CLI or store credentials needed.

---

## 🚀 Features

- **Shopping assistant:** answers product questions, recommends products, adds and removes cart items, and shows
  prices in the store's currency
- **Customer support:** tracks orders, answers store policy questions from CMS pages and points shoppers to the
  store's contact details
- **Notify me (back in stock):** shoppers confirm their email (double opt-in) and get an email when the product is
  back in stock
- **Chat widget:** one script tag for Edge Delivery Services, Luma and headless storefronts; name, welcome message,
  quick questions, avatar and colors can be customized
- **Your choice of AI:** Anthropic (Claude), OpenAI or Google Gemini, with your own API key
- **Data in your region:** Americas (US), Europe (EU), Asia Pacific (Japan) or Australia
- **Secure by default:** answers only store-related questions, works only on your allowed storefronts, applies rate
  limits and stores passwords encrypted

---

## ✅ Requirements

- Adobe Commerce 2.4.4 or later (SaaS or PaaS) with **Apps → App Management**
- PaaS only: the Admin UI SDK, and Adobe IMS for Adobe Commerce (or Commerce Integration credentials)
- An API key for Anthropic, OpenAI or Google Gemini
- SMTP server credentials (only for Notify me emails)

---

## 📦 Installation

1. Acquire **AI Commerce Chatbot** on [Adobe Exchange](https://exchange.adobe.com/) and deploy it to your
   environment.
2. In the Commerce Admin, open **Apps → App Management** and **Associate** the app.
3. Click **Configure**, choose the **Data region** first, then enter the AI provider, API key, allowed storefronts
   and other settings.
4. Click **Install**.
5. Copy the widget script from the **AI Chatbot** Admin page and add it to your storefront.

See [**INSTALL.md**](INSTALL.md) for the full steps, settings, widget customization and troubleshooting.

### ⚠️ Good to Know

- **Allowed storefronts** must list your storefront origins (for example `https://store.example.com`), or the chat
  works only in the Admin preview.
- The **Data region** can't be changed after installing. To move it, unassociate the app and install it again.
- **Unassociating** the app deletes all its data and the saved settings.

---

## 📄 License

See [LICENSE](LICENSE) and [COPYRIGHT](COPYRIGHT).

## 📩 Support

18th Digitech Pvt Ltd: **support@18dapps.com**
