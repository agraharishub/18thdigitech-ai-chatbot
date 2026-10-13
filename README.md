# AI Commerce Chatbot

The **AI Commerce Chatbot** adds an AI customer support and shopping assistant to Adobe Commerce storefronts (SaaS and
PaaS).

Shoppers can ask about products, get recommendations, manage their cart, track orders and read store policies in a
chat widget. Merchants buy it on Adobe Exchange and set it up in the Commerce Admin. You don't need to download code,
use a command line or give us store credentials.

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
- **Admin page:** store connection, database, AI status, a live chat preview, the embed code and the Notify me
  requests, in **Apps → AI Chatbot**
- **Data in your region:** Americas (US), Europe (EU), Asia Pacific (Japan) or Australia
- **Secure by default:** answers only store-related questions, works only on your allowed storefronts, applies rate
  limits and stores passwords and keys encrypted

---

## ✅ Requirements

- Adobe Commerce 2.4.4 or later (SaaS or PaaS) with **Apps → App Management**
- An Adobe organization entitled to **Adobe App Builder**
- An API key for Anthropic, OpenAI or Google Gemini (with billing set up with the provider)
- SMTP email credentials (only for Notify me emails)
- PaaS only: Adobe IMS for Adobe Commerce, or a Commerce Integration

---

## 📦 Installation at a Glance

| #   | Step                                                             | Where                                  | Guide                                                                | SaaS / PaaS |
| --- | ---------------------------------------------------------------- | -------------------------------------- | -------------------------------------------------------------------- | ----------- |
| 1   | Buy **AI Commerce Chatbot**                                      | Adobe Exchange                         | [Section 2](INSTALL.md#2-buy-the-app-on-adobe-exchange)              | Both        |
| 2   | Approve it, add an environment and **Deploy**                    | Adobe Exchange → Manage (System Admin) | [Section 3](INSTALL.md#3-approve-and-deploy-it)                      | Both        |
| 3   | Create your AI API key                                           | Anthropic, OpenAI or Google AI Studio  | [Section 4](INSTALL.md#4-create-your-ai-api-key)                     | Both        |
| 4   | Get your SMTP email settings (optional)                          | Your email provider                    | [Section 5](INSTALL.md#5-get-your-smtp-email-settings-for-notify-me) | Both        |
| 5   | Set up IMS or a Commerce Integration (SaaS: skip)                | Commerce Admin                         | [Section 6](INSTALL.md#6-connect-the-store-paas-only)                | PaaS only   |
| 6   | **Associate** the app                                            | Commerce Admin → Apps → App Management | [Section 7](INSTALL.md#7-associate-the-app-in-the-commerce-admin)    | Both        |
| 7   | **Configure**: Data region first, then AI, storefronts and email | Commerce Admin → Apps → App Management | [Section 8](INSTALL.md#8-configure-the-app)                          | Both        |
| 8   | **Install**                                                      | Commerce Admin → Apps → App Management | [Section 9](INSTALL.md#9-install-the-app)                            | Both        |
| 9   | Copy the embed code and add it to the storefront                 | Apps → AI Chatbot → Storefront         | [Section 11](INSTALL.md#11-add-the-chat-widget-to-your-storefront)   | Both        |
| 10  | Test it                                                          | Your storefront                        | [Section 13](INSTALL.md#13-test-everything)                          | Both        |

See [**INSTALL.md**](INSTALL.md) for every step in detail, widget customization, everyday use and troubleshooting.

### ⚠️ Good to Know

- **AI usage is billed by your AI provider**, separately from the app. Set a spend limit with the provider and use
  the hourly message limits in **Configure** to control costs.
- **Allowed storefronts** must list your storefront origins (for example `https://store.example.com`), or the chat
  works only in the Admin preview.
- The **Data region** can't be changed after installing. To move it, unassociate the app and install it again.
- **Unassociating** the app deletes all its data and the saved settings.
- The **catalog** (stock, quantity and price) is synced every 15 minutes, so new products and stock changes can take
  up to 15 minutes to reach the chat.

---

## 📄 License

See [LICENSE](LICENSE) and [COPYRIGHT](COPYRIGHT).

## 📩 Support

18th Digitech Pvt Ltd: **support@18dapps.com**
