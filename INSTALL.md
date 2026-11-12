# AI Commerce Chatbot for Adobe Commerce - Installation Guide

<!-- For what the app does and how it works, see [README.md](README.md). -->

## 1. Overview

AI Commerce Chatbot is an Adobe App Builder app that runs on Adobe I/O Runtime. It adds an AI shopping assistant and
customer support chat to your Adobe Commerce storefront:

<!-- The same app works for SaaS and PaaS. You don't download code or edit any files: every setting is entered in
**Apps → App Management → Configure**. -->

<!-- ## 2. SaaS vs PaaS

|                            |                              | PaaS                                                            |
| -------------------------- | ------------------------------- | --------------------------------------------------------------- |
| Commerce code changes      | None                            | None. Admin UI SDK only on 2.4.5 to 2.4.7                       |
| Authentication to Commerce | Adobe IMS, set up automatically | Adobe IMS, or a Commerce Integration (OAuth 1.0a), section 4.3  |
| Storefront                 | Edge Delivery Services (EDS)    | Luma, EDS or headless                                           |
| Product links and images   | Set in Configure                | Luma: detected automatically. EDS or headless: set in Configure | -->

## 2. Prerequisites

### 2.1 System Requirements

- Node v24
- Adobe Commerce as a Cloud Service (SaaS), or Adobe Commerce 2.4.5 or later (PaaS), with App Management
- Admin UI SDK 3.3.1 or later (PaaS 2.4.5 to 2.4.7 only)
- IMS enabled on Paas ([**Adobe Identity Management Service (IMS) for Adobe Commerce**](https://experienceleague.adobe.com/en/docs/commerce-admin/start/admin/ims/adobe-ims-integration-overview)) ,At Saas it is enabled by default
- Store reachable over public HTTPS (PaaS only)

### 2.2 Access Requirements

- Adobe Exchange (System Administrator, to deploy the app)
- Adobe Commerce Admin (App Management permissions)

### 2.3 Additional Requirements

- AI API key: Anthropic, OpenAI or Google Gemini (section 3.1)
- SMTP account, optional, for Notify me emails (section 3.2)

## 3. Prepare Credentials

### 3.1 AI API Key (SaaS and PaaS)

| Provider           | Where                                                                  | Key starts with | Default model       |
| ------------------ | ---------------------------------------------------------------------- | --------------- | ------------------- |
| Anthropic (Claude) | [Claude Console](https://console.anthropic.com/) → Settings → API Keys | `sk-ant-`       | `claude-sonnet-5-5` |
| OpenAI             | [OpenAI Platform](https://platform.openai.com/api-keys) → API keys     | `sk-`           | `gpt-4.1-mini`      |
| Google Gemini      | [Google AI Studio](https://aistudio.google.com/) → Get API key         | `AIza`          | `gemini-3.8-flash`  |

Add billing or credits with the provider first, or the key doesn't work. A Claude Pro or Max subscription does not
include API usage. Set a monthly spend limit with the provider: AI usage is billed by the provider, not by the app.

### 3.2 SMTP Email (Optional)

Only needed for Notify me (back-in-stock emails). You need the host, port, username, password and a sender address.

| Provider                 | Host                                | Port | Username                  |
| ------------------------ | ----------------------------------- | ---- | ------------------------- |
| Google Workspace / Gmail | `smtp.gmail.com`                    | 587  | Your email (app password) |
| Microsoft 365            | `smtp.office365.com`                | 587  | Your email                |
| SendGrid                 | `smtp.sendgrid.net`                 | 587  | `apikey`                  |
| Amazon SES               | `email-smtp.<region>.amazonaws.com` | 587  | SES SMTP username         |
| Mailgun                  | `smtp.mailgun.org`                  | 587  | Domain SMTP login         |

## 5 . Quick Start Installation

1. Acquire the AI Commerce Chatbot App from Adobe Exchange
2. Configure the environment and deploy the app from the Adobe Exchange Manage tab
3. Associate,Configure and Install the app in Commerce Admin → Apps → App Management
4. Your app is ready to use!

## 6. Acquire App From Adobe Exchange

1. Acquire the app from adobe exchnage
2. The app can be found in the Adobe Exchange under Manage Tab
3. Configure the environment and deploy the app
4. The app will be deployed to your organisation’s selected workspace.

<!--
### 4.3 Commerce Integration (PaaS only, without Adobe IMS)

Skip this if your store uses Adobe IMS for Adobe Commerce.

1. Commerce Admin → **System → Extensions → Integrations → Add New Integration**
2. Name: `AI Commerce Chatbot`. Enter your admin password
3. Open the **API** tab and choose **Resource Access: Custom**. Tick at least:
   - Catalog → Inventory → Products
   - Sales → Operations → Orders and Shipments
   - Content → Elements → Pages
   - Stores → Settings → All Stores
   - Stores → Attributes → Product
4. Click **Save**, then **Activate → Allow**
5. Copy the **Consumer Key**, **Consumer Secret**, **Access Token** and **Access Token Secret** -->

## 7. Install the App in Commerce

### 7.1 Associate the App

1. Log in to Commerce Admin
2. Go to **Apps → App Management**
3. Click **Associate App**
4. Select the Project and Workspace of AI Commerce Chatbot (for example `Production`)
5. Click **Associate**. The AI Commerce Chatbot card shows **Associated**

### 5.2 Configure the App

1. On the app card, click **Configure**
2. Choose the **Data region first**. It can't be changed after installing
3. Fill in the fields and click **Save**:

| Field                              | SaaS                                                               | PaaS                                                                                                                                                                                                     |
| ---------------------------------- | ------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Data region                        | Region of your store and customers                                 | Region of your store and customers                                                                                                                                                                       |
| AI provider                        | Select your AI provider                                            | Select your AI provider                                                                                                                                                                                  |
| AI model                           | Enter your AI model                                                | Enter your AI model                                                                                                                                                                                      |
| AI API key                         | Enter your AI API key                                              | Enter your AI API key                                                                                                                                                                                    |
| Chatbot name                       | Enter the name displayed in the chat header                        | Enter the name displayed in the chat header                                                                                                                                                              |
| Store name                         | Enter your store name                                              | Enter your store name                                                                                                                                                                                    |
| Allowed storefronts                | Enter comma-separated storefront URLs (e.g. `https://mystore.com`) | Enter comma-separated storefront URLs                                                                                                                                                                    |
| Chat messages per shopper per hour | `60` (default); `0` = unlimited                                    | `60` (default); `0` = unlimited                                                                                                                                                                          |
| Chat messages per store per hour   | `3000` (default); `0` = unlimited                                  | `3000` (default); `0` = unlimited                                                                                                                                                                        |
| Product page URL template          | `https://mystore.com/products/{url_key}/{sku}`                     | Luma: leave empty. EDS/Headless: configure as SaaS                                                                                                                                                       |
| Media base URL                     | `https://mystore.com/media`                                        | Luma: leave empty. EDS/Headless: configure as SaaS                                                                                                                                                       |
| SMTP settings (optional)           | Configure using section 4.2                                        | Configure using section 4.2                                                                                                                                                                              |
| Commerce Integration credentials   | Not required; Adobe IMS is used by default                         | Generate from **System → Extensions → Integrations → Add New Integration**, grant API access and activate the integration. Enter all four credentials. Alternatively, leave them empty to use Adobe IMS. |

**Data regions:** Americas (US), Europe, Middle East and Africa (EU), Asia Pacific (Japan), Australia.

**Allowed storefronts:** comma-separated origins only, no path and no trailing slash, for example
`https://mystore.com,https://www.mystore.com`

**SMTP secure connection:** toggle on for port 465, off for 587. Leave SMTP host empty to turn Notify me off.

Keys and passwords are stored encrypted. Changes apply within about a minute.

### 5.3 Install the App

1. On the app card, click **Install** and confirm. App Management:
   - It creates the app database in the chosen Data region
   - It registers your store
   - adds the Admin menu in adobe commerce admin **Apps → AI Chatbot**
2. Wait until the card shows **Installed**. If it shows **Partially Installed** or **Failed**, open the details, fix
   the cause (usually the Data region isn't chosen) and click **Install** again

## 6. Check the AI Chatbot Page

1. Go to **Apps → AI Chatbot → Overview**. Store connection, Database and AI assistant should show no warnings
2. Open the **Storefront** tab in app and use **Try the chatbot** to ask about a product in preview mode
3. Copy the `<script>` tag from **Embed code**. You need it in section 7

## 7. Connect Your Storefront

| Storefront                   | Section |
| ---------------------------- | ------- |
| Edge Delivery Services(Saas) | 7.1     |
| (PaaS)                       | 7.2     |

### 7.1 Edge Delivery Services

Make this change in your storefront repository.

1. Add the widget to the end of `scripts/delayed.js`. `cartApi` connects the bot to the shopper's own cart and mini
   cart. The other options are optional:

```js
import * as cartApi from "@dropins/storefront-cart/api.js";
import { loadScript } from "./aem.js";

window.CHATBOT_CONFIG = {
  cartApi,
  welcome:
    "Hi! I can help you find a product, add it to your cart or track an order.",
  suggestions: ["New arrivals", "Gift ideas", "Track my order"],
  avatarIcon: "bag",
  theme: { accent: "#1f6feb" },
};

loadScript("<widget URL from the embed code>", { defer: "" });
```

If `loadScript` is already imported, don't import it again.

2. Commit and push to your publish branch (usually `main`). Preview on `.aem.page`, then publish to `.aem.live`.

### 7.2 (PaaS)

1. Commerce Admin → **Content → Design → Configuration → Edit** (your store view, or the global row)
2. Expand **HTML Head** and paste the `<script>` tag at the end of **Scripts and Style Sheets**
3. For suggestion you can paste this **before** the `<script>` tag:

```html
<script>
  window.CHATBOT_CONFIG = {
    welcome: "Hi! How can I help you today?",
    suggestions: ["New arrivals", "Track my order"],
    theme: { accent: "#1f6feb" },
  };
</script>
```

4. Click **Save Configuration**
5. **System → Cache Management**: flush the cache

The bot adds products to the shopper's own Luma cart with no extra setup.

## 8. Troubleshooting

| Problem                                      | Fix                                                                                         |
| -------------------------------------------- | ------------------------------------------------------------------------------------------- |
| App not listed in App Management             | In Adobe Exchange → Manage, check the environment is **Deployed** in the same organization  |
| PaaS: no **Apps** menu in Admin              | On 2.4.5 to 2.4.7, install Admin UI SDK 3.3.1 or later (section 1), then flush the cache    |
| **Partially Installed** or database missing  | Choose the Data region in Configure, save and click **Install** again                       |
| AI assistant **Not configured** or AI errors | Fill in AI provider, AI model and AI API key, and check the provider account has credit     |
| Chat button doesn't appear                   | Check the app is **Installed** (click **Install** again after an update) and the script tag |
| Works in (Try the chatbot), not on the store | Add the exact storefront origin to Allowed storefronts (check `www`, no trailing slash)     |
| Wrong product links or no images             | EDS or headless: set Product page URL template and Media base URL                           |
| PaaS: the chatbot can't reach the store      | fill in all four Integration values and check the integration is Active                     |

## 8. Uninstall

Remove the script from your storefront, then **Apps → App Management → Unassociate**. This deletes the app's data and
every saved setting.
