# Installation Guide

AI Commerce Chatbot

This guide takes you from buying **AI Commerce Chatbot** on Adobe Exchange to a working chat on your storefront. The
app runs in your own Adobe organization and is set up entirely from Adobe Exchange and the Commerce Admin. You don't
need to download code, use a command line or edit configuration files.

**Contents**

- [SaaS or PaaS: which steps are for you](#saas-or-paas-which-steps-are-for-you)

1. [Before you start](#1-before-you-start)
2. [Buy the app on Adobe Exchange](#2-buy-the-app-on-adobe-exchange)
3. [Approve and deploy it](#3-approve-and-deploy-it)
4. [Create your AI API key](#4-create-your-ai-api-key)
5. [Get your SMTP email settings](#5-get-your-smtp-email-settings-for-notify-me)
6. [Connect the store (PaaS only)](#6-connect-the-store-paas-only)
7. [Associate the app in the Commerce Admin](#7-associate-the-app-in-the-commerce-admin)
8. [Configure the app](#8-configure-the-app)
9. [Install the app](#9-install-the-app)
10. [Check the AI Chatbot page](#10-check-the-ai-chatbot-page)
11. [Add the chat widget to your storefront](#11-add-the-chat-widget-to-your-storefront)
12. [Customize the widget](#12-customize-the-widget)
13. [Test everything](#13-test-everything)
14. [Everyday use](#14-everyday-use)
15. [Change settings, move region or remove the app](#15-change-settings-move-region-or-remove-the-app)
16. [Troubleshooting](#16-troubleshooting)
17. [Support](#17-support)

---

## SaaS or PaaS: which steps are for you

**How to tell which you have:**

- **SaaS** (Adobe Commerce as a Cloud Service): you open the Commerce Admin from
  [Adobe Experience Cloud](https://experience.adobe.com/) (Commerce → your instance), and the REST API address looks
  like `https://na1.api.commerce.adobe.com/<tenant-id>/`. Your storefront runs on Edge Delivery Services.
- **PaaS** (Adobe Commerce on cloud infrastructure or on-premises): you open the Admin at your own address, for
  example `https://mystore.com/admin` or `https://<environment>.magentosite.cloud/admin`. Your storefront is Luma,
  Edge Delivery Services or headless.

| Section                                                | SaaS                         | PaaS                                                       |
| ------------------------------------------------------ | ---------------------------- | ---------------------------------------------------------- |
| 1. Before you start                                    | ✅                           | ✅ plus App Management / Admin UI SDK on 2.4.5 to 2.4.7    |
| 2. Buy the app on Adobe Exchange                       | ✅                           | ✅                                                         |
| 3. Approve and deploy it                               | ✅                           | ✅                                                         |
| 4. Create your AI API key                              | ✅                           | ✅                                                         |
| 5. Get your SMTP email settings                        | ✅ optional                  | ✅ optional                                                |
| 6. Connect the store                                   | ❌ skip (automatic with IMS) | ✅ Adobe IMS or a Commerce Integration                     |
| 7. Associate the app                                   | ✅                           | ✅ (in the backend Admin)                                  |
| 8. Configure the app                                   | ✅                           | ✅ plus the 4 Commerce Integration settings if you use one |
| 9. Install the app                                     | ✅                           | ✅                                                         |
| 10. Check the AI Chatbot page                          | ✅                           | ✅                                                         |
| 11. Add the chat widget                                | ✅ Edge Delivery Services    | ✅ Edge Delivery Services, Luma or headless                |
| 12. Customize the widget                               | ✅                           | ✅                                                         |
| 13 to 17. Test, everyday use, changes, troubleshooting | ✅                           | ✅                                                         |

---

## 1. Before you start

**Applies to:** ✅ SaaS · ✅ PaaS (a few requirements differ, marked below)

### What you need

| What                      | Details                                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Adobe Commerce            | 2.4.4 or later. **SaaS** (Adobe Commerce as a Cloud Service) or **PaaS** (Adobe Commerce on cloud infrastructure or on-premises)                                      |
| App Management            | **Apps → App Management** in the Commerce Admin. Included on SaaS and on PaaS 2.4.8+. On PaaS 2.4.5 to 2.4.7, your Commerce developer installs the Admin UI SDK first |
| Adobe App Builder         | Your Adobe organization must be entitled to App Builder (ask your Adobe account manager if you aren't sure)                                                           |
| An AI provider account    | Anthropic (Claude), OpenAI or Google Gemini, with billing set up (section 4)                                                                                          |
| An SMTP email account     | Only for **Notify me** back-in-stock emails (section 5)                                                                                                               |
| PaaS store authentication | Adobe IMS for Adobe Commerce, or a Commerce Integration (section 6). Not needed on SaaS                                                                               |

### Who does what

| Step                             | Who                                                                         |
| -------------------------------- | --------------------------------------------------------------------------- |
| Buy the app on Adobe Exchange    | Anyone in your Adobe organization who can make purchases                    |
| Approve and deploy it            | A **System Administrator** of your Adobe organization (Adobe Admin Console) |
| Associate, configure and install | A Commerce Admin user with access to **Apps → App Management**              |
| Add the widget to the storefront | Whoever manages your storefront theme or Edge Delivery Services site        |

Have these open in your browser: [Adobe Exchange](https://exchange.adobe.com/), your Commerce Admin and the account
page of your AI provider.

---

## 2. Buy the app on Adobe Exchange

**Applies to:** ✅ SaaS · ✅ PaaS

1. Go to [Adobe Exchange](https://exchange.adobe.com/) and sign in with the Adobe ID of your **company organization**
   (the one your Commerce store belongs to). If you belong to several organizations, switch to the right one using
   the profile menu at the top right.
2. Search for **AI Commerce Chatbot** (filter the app type to **App Builder** if needed) and open the listing.
3. Read the description, the pricing and the required products (**App Builder** and **Adobe Commerce**).
4. Click the purchase button at the top right of the listing (it shows the price) and complete the checkout: choose
   the plan, enter the billing details and confirm the payment.
5. In the next dialogs, confirm that your organization has the required Adobe products (**Yes, continue**), then
   read and accept the license agreement (**Accept and continue**).
6. When the purchase is complete, the listing button changes to **Manage** and the status shows **Acquired**. You
   receive a receipt by email. Keep it: our support team asks for it.

The purchase covers the app itself. AI usage is billed separately by your AI provider (section 4), and emails by your
email provider (section 5).

---

## 3. Approve and deploy it

**Applies to:** ✅ SaaS · ✅ PaaS

A System Administrator of your Adobe organization does this step.

### 3.1 Approve the purchase

1. In Adobe Exchange, open **Manage → App Builder Applications**.
2. Click **AI Commerce Chatbot**, then **Review**.
3. Add a note if you like and **approve** it. The status changes to **Approved**.

### 3.2 Create an environment and deploy

Deploy once for each Commerce environment you want the chatbot on. We recommend a **Stage** environment first for
your staging store, and **Production** once you've tested it.

1. On the app's page in **Manage**, click **Add environment**, give it a name (for example `Stage` or `Production`)
   and confirm.
2. Click the environment's card and open the **Configuration** tab. Leave the defaults unless your Adobe administrator
   uses specific product profiles.
3. Click **Deploy**. It takes a few minutes. When the status shows **Deployed**, the app is running in your
   organization.

Deploying creates a project and a workspace (with the environment's name) in the
[Adobe Developer Console](https://developer.adobe.com/developer-console/) of your organization. You'll pick them in
section 7. The App Builder Data Services API and the credentials the app needs are added for you.

---

## 4. Create your AI API key

**Applies to:** ✅ SaaS · ✅ PaaS

The chatbot uses an AI provider of your choice, with **your own** API key. You pay the provider directly for what the
chat uses. Pick one provider and follow its steps.

> **Keep the key secret.** It is shown only once. Paste it straight into **Configure** (section 8), where it is stored
> encrypted, and don't email it or put it on the storefront.

### Anthropic (Claude), the default provider

1. Go to the [Claude Console](https://console.anthropic.com/) and sign up or sign in.
2. Set up billing: open **Settings → Billing**, add a payment method and buy credits. Without credits the key doesn't
   work. A Claude Pro or Max chat subscription does **not** include API usage.
3. Open **Settings → API Keys** and click **Create Key**.
4. Name it (for example `AI Commerce Chatbot - Production`) and click **Create**.
5. Copy the key (it starts with `sk-ant-`) and keep it for section 8.
6. Recommended: set a monthly spend limit under **Settings → Limits**.

Default model: `claude-sonnet-5-5`.

### OpenAI

1. Go to the [OpenAI Platform](https://platform.openai.com/) and sign up or sign in.
2. Set up billing: open **Settings → Billing**, add a payment method and add credit.
3. Open [**API keys**](https://platform.openai.com/api-keys) and click **Create new secret key**.
4. Name it (for example `AI Commerce Chatbot`), choose the project, keep the permissions at **All** and click
   **Create secret key**.
5. Copy the key (it starts with `sk-`) and keep it for section 8.
6. Recommended: set a monthly budget under **Settings → Limits**.

Default model: `gpt-4.1-mini`.

### Google Gemini

1. Go to [Google AI Studio](https://aistudio.google.com/) and sign in with a Google account.
2. Click **Get API key**, then **Create API key**, and select (or create) a Google Cloud project.
3. Copy the key (it starts with `AIza`) and keep it for section 8.
4. For a live store, turn on billing for that Google Cloud project (**Set up billing** next to the key). The free tier
   has low limits and allows Google to use the data to improve its products.

Default model: `gemini-3.8-flash`.

### Choosing a model

Leave **AI model** empty in section 8 to use the default model shown above. To use another model of the same
provider, enter its exact model ID as listed on the provider's models page.

---

## 5. Get your SMTP email settings (for Notify me)

**Applies to:** ✅ SaaS · ✅ PaaS (optional, only for Notify me)

**Notify me** lets shoppers ask to be emailed when an out-of-stock product is back. The app sends a confirmation email
(double opt-in) and then the back-in-stock email, through your own email account. Skip this section if you don't
want Notify me: the chat works without it.

You need: **SMTP host**, **port**, **secure connection** on or off, **username**, **password** and the **sender**
address. Common providers:

| Provider                 | SMTP host                           | Port | Secure connection | Username                      | Password                                                                                         |
| ------------------------ | ----------------------------------- | ---- | ----------------- | ----------------------------- | ------------------------------------------------------------------------------------------------ |
| Google Workspace / Gmail | `smtp.gmail.com`                    | 587  | Off               | Your full email address       | An **app password**: turn on 2-Step Verification, then Google Account → Security → App passwords |
| Microsoft 365 / Outlook  | `smtp.office365.com`                | 587  | Off               | Your full email address       | The mailbox password. Your Microsoft 365 admin must allow **Authenticated SMTP** for the mailbox |
| SendGrid                 | `smtp.sendgrid.net`                 | 587  | Off               | `apikey` (literally)          | A SendGrid API key with **Mail Send** permission                                                 |
| Amazon SES               | `email-smtp.<region>.amazonaws.com` | 587  | Off               | The SMTP username from SES    | The SMTP password from SES (**SMTP settings → Create SMTP credentials**)                         |
| Mailgun                  | `smtp.mailgun.org`                  | 587  | Off               | The SMTP login of your domain | The SMTP password of your domain                                                                 |

Port 465 also works with **Secure connection** turned on.

For the **sender**, use an address on your store's domain that your provider is allowed to send from, for example
`My Store <no-reply@mystore.com>`. With SendGrid, SES or Mailgun, verify the domain or sender with the provider first,
or the emails are rejected or land in spam.

---

## 6. Connect the store (PaaS only)

**Applies to:** ❌ SaaS (skip) · ✅ PaaS only

**SaaS stores: skip this section.** The app connects to your store with Adobe IMS automatically.

On **PaaS**, the app reads products, orders and CMS pages through the Commerce REST API. Use one of these:

### 6.1: Adobe IMS for Adobe Commerce

To configure IMS auth follow the document given below
[Adobe IMS for Adobe Commerce](https://experienceleague.adobe.com/en/docs/commerce-admin/start/admin/ims/adobe-ims-config).

---

## 7. Associate the app in the Commerce Admin

**Applies to:** ✅ SaaS · ✅ PaaS

This connects the deployed app to your store.

1. In the Commerce Admin, go to **Apps → App Management**. (SaaS: the Commerce Admin of your instance. PaaS: the
   backend Admin.)
2. Click **Associate App**.
3. Select the **Project** (**AI Commerce Chatbot**) and the **Workspace** (the environment name from section 3.2, for
   example `Production`). Choose the environment that matches this store: Stage for a staging store, Production for
   the live store.
4. Click **Associate**. The app card shows **Associated**, and the store's websites, stores and store views are
   imported.

---

## 8. Configure the app

**Applies to:** ✅ SaaS · ✅ PaaS (the **Commerce Integration** settings are PaaS only)

On the **AI Commerce Chatbot** card, click **Configure**, fill in the settings below and click **Save**. Values apply
within about a minute. Passwords and keys are stored encrypted.

To use different values for a website or store view, click **Change Scope**, select it, change the values and save.
Lower scopes override higher ones.

### Data region (SaaS and PaaS, choose this first)

Where the store's data (Notify me requests, synced catalog, sync history) is stored. **It can't be changed later**
without deleting the data (section 15).

| Option                          | Data stored in | Choose it for                                                           |
| ------------------------------- | -------------- | ----------------------------------------------------------------------- |
| Americas, US data center        | United States  | United States, Canada, Mexico, Central and South America, the Caribbean |
| Europe, Middle East and Africa  | European Union | European Union, EEA, United Kingdom, Switzerland, Middle East, Africa   |
| Asia Pacific, Japan data center | Japan          | Japan, India, China, Hong Kong, Singapore, South Korea, Taiwan, SE Asia |
| Australia data center           | Australia      | Australia, New Zealand, Pacific islands                                 |

Choose the region of your store and customers, and check your data protection rules (for example GDPR: Europe).

### AI assistant (SaaS and PaaS)

| Setting         | What to enter                                                                  |
| --------------- | ------------------------------------------------------------------------------ |
| **AI provider** | Anthropic (Claude), OpenAI or Google Gemini: the provider of your key          |
| **AI model**    | Leave empty for the provider's default model (section 4), or an exact model ID |
| **AI API key**  | The key from section 4                                                         |

### Chat (SaaS and PaaS)

| Setting                                  | What to enter                                                                                                                                                              |
| ---------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Chatbot name**                         | The name shown in the chat header, for example `Shopping Assistant`                                                                                                        |
| **Store name**                           | Your store's name, used by the assistant in its answers                                                                                                                    |
| **Allowed storefronts**                  | Your storefront addresses, comma-separated, for example `https://mystore.com,https://www.mystore.com`. **Required**: while empty, the chat works only in the Admin preview |
| **Chat messages per shopper per hour**   | Default `60`. Limits one shopper. `0` = no limit                                                                                                                           |
| **Chat messages per hour for the store** | Default `3000`. Limits all shoppers together and caps your AI costs. `0` = no limit                                                                                        |

For **Allowed storefronts**, enter only the origin: `https://`, the domain and the port if there is one. No path and
no trailing slash. Add every domain shoppers use (with and without `www`, and staging domains on the Stage
environment).

### Storefront links (SaaS and PaaS: Edge Delivery Services and headless only)

Leave both empty on a **Luma** storefront: they are detected from the store.

| Setting                       | What to enter                                                                                                         |
| ----------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| **Product page URL template** | The product page address, with `{url_key}` and/or `{sku}`, for example `https://mystore.com/products/{url_key}/{sku}` |
| **Media base URL**            | Where product images are served from, for example `https://mystore.com/media`                                         |

### Notify me emails (SaaS and PaaS)

The values from section 5: **SMTP host**, **SMTP port**, **SMTP secure connection** (on for port 465, off for 587),
**SMTP username**, **SMTP password** and **Email sender**. Leave the host empty to turn Notify me off.

### Commerce Integration (PaaS only)

Only if you chose Option B in section 6: **Commerce Integration consumer key**, **consumer secret**, **access token**
and **access token secret**. Fill in all four, or leave all four empty to use Adobe IMS.

---

## 9. Install the app

**Applies to:** ✅ SaaS · ✅ PaaS

1. Back on the app card in **Apps → App Management**, click **Install** and confirm.
2. The installation runs the **Register store** step: it creates the app's database in your Data region and records
   your store. This takes a few minutes (up to 5).
3. When it finishes, the card shows **Installed**.

If the card shows **Partially Installed**, open the details to see the failed step, fix the cause (usually the Data
region isn't chosen yet) and click **Install** again.

---

## 10. Check the AI Chatbot page

**Applies to:** ✅ SaaS · ✅ PaaS

Open **Apps → AI Chatbot** in the Commerce Admin. The page has four tabs:

- **Overview**
  - **Store connection:** the store type (SaaS or PaaS), the store URL and how the app connects (IMS or Integration).
  - **Database:** **Connected**, with your **Data region** and the region recommended for your store.
  - **AI assistant:** the provider and model.
- **Settings:** every setting with the value in use and where it comes from.
- **Storefront:**
  - **Try the chatbot:** a live preview, to test the chat before adding it to the storefront.
  - **Embed code:** the script tag for section 11, the allowed storefronts and the chat endpoint.
- **Notify me:** shoppers' back-in-stock requests and the synced catalog (section 14).

If **Overview** shows a warning, see section 16.

---

## 11 Edge Delivery Services storefront (SaaS )

In your storefront's code repository (usually GitHub):

1. Open `scripts/delayed.js` and add at the end (replace the address with yours):

   ```js
   import { loadScript } from "./aem.js";

   loadScript(
     "https://<your-namespace>.adobeioruntime.net/api/v1/web/chatbot/widget",
     { defer: "" },
   );
   ```

   If `loadScript` is already imported in the file, don't import it again. Alternatively, paste the `<script>` tag
   into `head.html`.

2. Commit and push to your main branch. The change is live on the site within a few minutes.

On SaaS, also pass the cart drop-in so the chatbot adds products to the shopper's own cart (see section 12).

### Luma storefront (PaaS only)

1. In the Commerce Admin, go to **Content → Design → Configuration**.
2. Click **Edit** on the row of your store view (or the website/global row to cover all of them).
3. Expand **HTML Head** and paste the `<script>` tag at the end of **Scripts and Style Sheets**.
4. Click **Save Configuration**.
5. Go to **System → Cache Management** and refresh the invalid cache types (or **Flush Magento Cache**).

### Headless or custom storefront (PaaS)

Paste the `<script>` tag into the HTML `<head>` (or before `</body>`) of the page layout used by every page, and
deploy your storefront as usual.

### Check it

Open your storefront in a private window. A chat button appears at the bottom right. Open it and ask a product
question. It can take up to a minute after saving settings for changes to show.

---

## 12. Customize the widget (SaaS)

**Applies to:** ✅ SaaS · ❌ PaaS

**PaaS:** skip this section. The `<script>` tag from section 11 is all you need. The chatbot name comes from
**Chatbot name** (section 8), the rest of the widget uses its built-in look, and the bot adds products to the
shopper's own Luma cart without any extra setup.

**SaaS (Edge Delivery Services):** set `window.CHATBOT_CONFIG` in `scripts/delayed.js`, before the widget loads.
`cartApi` connects the bot to the shopper's own cart and updates the mini cart. Without it, the bot keeps its own
separate cart. The other settings change the welcome message, the quick questions, the icon and the input text; leave
out any you don't need and the default is used.

Replace the code you added to `scripts/delayed.js` in section 11 with this:

```js
import * as cartApi from "@dropins/storefront-cart/api.js";
import { loadScript } from "./aem.js";

window.CHATBOT_CONFIG = {
  cartApi,
  welcome:
    "Hi! I can help you find a product, add it to your cart or track an order.",
  suggestions: ["New arrivals", "View cart", "Gift ideas", "Track my order"],
  avatarIcon: "bag",
  placeholder: "Ask about products or your order",
};

loadScript(
  "https://<your-namespace>.adobeioruntime.net/api/v1/web/chatbot/widget",
  { defer: "" },
);
```

Commit and push to your main branch, as in section 11.

---

## 15. Change settings, move region or remove the app

**Applies to:** ✅ SaaS · ✅ PaaS

- **Change a setting:** **Apps → App Management → Configure**, change it and click **Save**. It applies within about
  a minute.
- **Switch AI provider:** change **AI provider** and **AI API key** together (and empty **AI model**), then save.
- **Add a storefront domain:** add it to **Allowed storefronts**, comma-separated, then save.
- **Move to another Data region:** the data can't be moved. In production workspace database can not be removed.
- **Remove the app from a store:** **Apps → App Management**, click **Unassociate** on the app and confirm. This
  deletes the app's database with **all** its data and every value saved in **Configure**, and can't be undone. Also
  remove the widget script from the storefront (section 11).
- **Stop using the app in your organization:** after unassociating every store, a System Administrator can undeploy
  the environments in **Adobe Exchange → Manage**, and contact us about the subscription.

---

## 16. Troubleshooting

**Applies to:** ✅ SaaS · ✅ PaaS (rows that differ say SaaS or PaaS)

| Problem                                                                           | What to do                                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| The chat button doesn't appear on the storefront                                  | Check that the script is on the page (section 11) and, on Luma, that the cache was refreshed. Try a private window.                                                                                                                      |
| The chat opens but every message fails                                            | Add the exact storefront origin to **Allowed storefronts** (`https://`, the domain, no path or trailing slash) and save. Check the AI key: **AI Chatbot → Overview → AI assistant**.                                                     |
| It works in **Try the chatbot** but not on the storefront                         | **Allowed storefronts** is empty or doesn't list this domain (check `www` and staging domains).                                                                                                                                          |
| **AI provider missing** or AI errors                                              | Enter the **AI provider** and **AI API key** in **Configure**. Check that the key belongs to the selected provider, isn't deleted and that the provider account has credit. If you entered an **AI model**, empty it to use the default. |
| **The chatbot cannot reach the store**                                            | Check the association in **App Management**. PaaS: set up Adobe IMS or fill in all four Commerce Integration values (section 6), and check that the integration is **Active**. SaaS: contact support.                                    |
| **Database: Not created yet**                                                     | Choose the **Data region** in **Configure**, save, and click **Install** again. Creating the database takes a few minutes.                                                                                                               |
| Database in the wrong region                                                      | It can't be moved: see section 15.                                                                                                                                                                                                       |
| The card shows **Partially Installed**                                            | Open it to see the failed step. Usually the **Data region** isn't chosen yet: choose it, save and click **Install** again.                                                                                                               |
| **Configure** shows "Configuration load error" or "Failed to fetch config values" | Contact support with your environment name. Don't unassociate the app: that deletes the saved settings and data.                                                                                                                         |
| Wrong product links or missing images                                             | On Edge Delivery Services or headless storefronts, set **Product page URL template** and **Media base URL** (section 8). On Luma, leave both empty.                                                                                      |
| New products or stock changes aren't in the chat yet                              | The catalog syncs every 15 minutes: wait for the next sync and check **Last sync** on **AI Chatbot → Notify me**. If it doesn't change for over an hour, contact support.                                                                |
| "Notify me is not available right now"                                            | Fill in **SMTP host** and **Email sender** in **Configure**.                                                                                                                                                                             |
| Notify me emails don't arrive                                                     | Check that the shopper confirmed the request (not **Awaiting confirmation**), the SMTP values (section 5), the spam folder, and that the sender's domain is verified with your email provider.                                           |
| "You've sent a lot of messages…"                                                  | An hourly limit was reached. Raise **Chat messages per shopper per hour** or **per hour for the store** in **Configure**.                                                                                                                |
| **AI Commerce Chatbot** isn't listed in App Management                            | Check in **Adobe Exchange → Manage** that the app is **Approved** and the environment is **Deployed**, and that you're associating with the same Adobe organization.                                                                     |

---

## 17. Support

**Applies to:** ✅ SaaS · ✅ PaaS

18th Digitech Pvt Ltd: **support@18dapps.com**

Please include: your Adobe organization name, the environment (for example `Production`), the store URL, what you
did, what you expected and a screenshot of **AI Chatbot → Overview**. Never send your AI API key, SMTP password or
Commerce Integration secrets.
