# AI Commerce Chatbot

The **AI Commerce Chatbot** adds an AI customer support and shopping assistant to Adobe Commerce storefronts (SaaS and
PaaS) using Adobe App Builder.

Shoppers can ask about products, get recommendations, manage their cart, track orders and read store policies in a
chat widget. Merchants set it up and configure it in **Apps → App Management** in the Commerce Admin, with no store
URL or credentials to enter.

---

## 🚀 Features

### 🔹 Shopping Assistant

- Answers product questions and recommends products
- Lists in-stock and out-of-stock products
- Adds products to the cart and removes them: the Edge Delivery Services cart drop-in, the Luma session cart, or a
  cart the bot manages
- Shows prices in the store's display currency

### 🔹 Customer Support

- Tracks orders with the order number and email
- Answers store policy questions from CMS pages
- When a shopper asks for a human, points them to the store's contact details

### 🔹 Notify Me (Back in Stock)

- Shoppers ask to be emailed when an out-of-stock product is back
- Catalog sync and emails run automatically every 15 minutes
- Requests and the synced catalog are shown on the Admin page

### 🔹 Chat Widget

- One script tag for any storefront (Edge Delivery Services, Luma, headless)
- Dark, rounded design with the bot's name and avatar, a cart badge, quick-question chips and a "New chat" button
- Name, welcome message, quick questions, avatar and colors can be set per store

### 🔹 Multiple AI Providers

- Anthropic (Claude), OpenAI or Google Gemini
- Choose the provider, model and API key in **Configure**

### 🔹 Data in the Merchant's Region

- Data is stored in an App Builder Database in the region the merchant chooses: Americas (US), Europe (EU), Asia
  Pacific (Japan) or Australia
- Each store's data is kept separate; unassociating the app deletes the store's data

### 🔹 Secure by Default

- Answers only questions about the store; refuses unrelated requests and prompt-injection attempts
- Public endpoints are limited to the allowed storefront origins
- Password settings (AI API key, SMTP password, Commerce Integration credentials) are stored encrypted
- Store access uses Adobe IMS

---

## 📦 Installation

Please follow the detailed installation instructions in:

👉 [**INSTALL.md**](INSTALL.md)

## ✅ Prerequisites

- Node.js v24
- Adobe App Builder
- Adobe I/O CLI v10 or later
- Adobe Commerce 2.4.4 or later (SaaS or PaaS) with App Management
- An API key for Anthropic, OpenAI or Google Gemini
- The **App Builder Data Services** API added to each workspace in the Adobe Developer Console (database access)

---

## ⚠️ Important -- Data Region

The store's data is stored in the region chosen **once**, before the app is installed in App Management:

- Merchants installing the app from Adobe Exchange or the App Builder Catalog choose **Data region** in the install
  form.
- For your own deploys, set `AIO_DB_REGION` in `.env` (`amer`, `emea`, `apac` or `aus`) before deploying.
- If neither is set, the merchant chooses it in **App Management → Configure → Data region**.

The database can't be moved afterwards. See [Database](#database).

## ⚠️ Important -- Encryption Key

`AIO_COMMERCE_CONFIG_ENCRYPTION_KEY` in `.env` encrypts the password settings saved in **Configure**. It is generated
by `npm install`.

**Keep it the same across deploys.** If it changes, the saved passwords can't be read and must be entered again.

## ⚠️ Important -- Allowed Storefronts

Set **Allowed storefronts** in **Configure** to the storefront origins, comma-separated, for example:

```text
https://store.example.com,https://www.store.example.com
```

Leaving it empty allows any website. Use that only for development.

## ⚠️ Important -- Merchant Settings

Merchants enter these settings in **App Management → AI Commerce Chatbot → Configure**:

| Setting                                                           | Notes                                                       |
| ----------------------------------------------------------------- | ----------------------------------------------------------- |
| Data region                                                       | Only if it wasn't chosen at install; can't be changed later |
| AI provider, AI model, AI API key                                 | The key is stored encrypted                                 |
| Chatbot name, Store name                                          | The chatbot name is shown in the widget header              |
| Allowed storefronts                                               | Storefront origins allowed to use the chat                  |
| Product page URL template, Media base URL                         | Only for Edge Delivery Services / headless storefronts      |
| SMTP host, port, secure connection, username, password, sender    | For Notify me emails; the password is stored encrypted      |
| Commerce Integration consumer key / secret, access token / secret | PaaS stores without Adobe IMS only; stored encrypted        |

Saved values apply within about a minute, without a redeploy. They are never read from `.env` or from the request.
`.env` only holds developer and platform values: Runtime and IMS credentials, `AIO_COMMERCE_CONFIG_ENCRYPTION_KEY`,
`AIO_DB_REGION`, `LOG_LEVEL` and the legacy Notify me file paths.

The currency (PaaS and SaaS) and, on Luma (PaaS), the product page, image and cart URLs are detected from the store.

---

## 🛍 Storefront Setup

After deploying, add the script shown on the **AI Chatbot** Admin page to the storefront:

```html
<script
  defer
  src="https://<namespace>.adobeioruntime.net/api/v1/web/chatbot/widget"
></script>
```

- **Edge Delivery Services:** load it from `scripts/delayed.js` (see below) or add it to `head.html`
- **Luma:** Content → Design → Configuration → HTML Head → Scripts and Style Sheets

### Customize the Widget

Optional settings go in `window.CHATBOT_CONFIG`, set **before** the widget script loads:

| Key           | What it changes                                                                    | Default                                                     |
| ------------- | ---------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `title`       | Name in the header (otherwise the **Chatbot name** setting)                        | "Shopping Assistant"                                        |
| `subtitle`    | Line under the name                                                                | "Online"                                                    |
| `welcome`     | First message                                                                      | A generic greeting                                          |
| `suggestions` | Quick-question chips shown before the first message                                | "Best sellers", "Under $50", "Gift ideas", "Track my order" |
| `placeholder` | Text in the message box                                                            | "Ask about products or your order"                          |
| `avatarIcon`  | Header icon: `bag`, `wine`, `sparkle` or `chat`                                    | `bag`                                                       |
| `avatarUrl`   | An image for the avatar instead of an icon                                         | —                                                           |
| `theme`       | Colors: `accent`, `accentText`, `background`, `surface`, `border`, `text`, `muted` | Coral on dark brown                                         |
| `cartApi`     | The storefront's cart drop-in API (Edge Delivery Services)                         | —                                                           |
| `cartMode`    | `storefront` (Luma session cart) or `bot` (cart managed by the bot)                | Detected                                                    |
| `cartUrl`     | Cart page for the header cart icon, when the bot manages the cart                  | The cart URL returned by the bot                            |
| `open`        | Open the chat when the page loads                                                  | `false`                                                     |

All keys are also listed at the top of `src/commerce-extensibility-1/actions/chatbot/widget/chat-widget.js`.

Example for an Edge Delivery Services store (`scripts/delayed.js`), with the cart drop-in so the bot changes the
shopper's own cart:

```js
import * as cartApi from "@dropins/storefront-cart/api.js";
import { loadScript } from "./aem.js";

window.CHATBOT_CONFIG = {
  cartApi,
  welcome:
    "Hi, I am Zyra. I can help you find a bottle, add it to your cart, or track an order.",
  suggestions: ["Red wine", "Under $20", "Gift ideas", "Track my order"],
  avatarIcon: "wine",
  placeholder: "Ask about wines or your order",
};

loadScript("https://<namespace>.adobeioruntime.net/api/v1/web/chatbot/widget", {
  defer: "",
});
```

The widget script is cached for 60 seconds, so changes reach storefronts within about a minute.

---

## 🧩 Extensions & Actions

| Extension                  | Folder                                         | Purpose                                                                                   |
| -------------------------- | ---------------------------------------------- | ----------------------------------------------------------------------------------------- |
| `commerce/extensibility/1` | `src/commerce-extensibility-1`                 | App Management actions (generated), including the installation with the custom step       |
| `commerce/configuration/1` | `src/commerce-configuration-1`                 | Configure form backend (generated from `businessConfig`)                                  |
| `commerce/backend-ui/2`    | `src/commerce-backend-ui-2`                    | **AI Chatbot** Admin page: status, database, embed code, chat preview, Notify me requests |
| `application`              | `src/commerce-extensibility-1/actions/chatbot` | Chatbot actions (package `chatbot`)                                                       |

| Action                                    | Access                             | Purpose                                                                    |
| ----------------------------------------- | ---------------------------------- | -------------------------------------------------------------------------- |
| `chat`                                    | public, origin allow-list          | Chat endpoint called by the widget                                         |
| `widget`                                  | public                             | Serves the storefront widget script                                        |
| `notify`                                  | public, origin allow-list          | Saves a Notify me request                                                  |
| `notify-sync`                             | scheduled every 15 minutes         | Syncs the catalog, sends back-in-stock emails, finishes the database setup |
| `status`                                  | Admin (IMS)                        | Status for the Admin page, including the database and region               |
| `notify-data`                             | Admin (IMS)                        | Notify me requests and the synced catalog for the Admin page               |
| `db-setup`                                | `post-app-deploy` hook             | One-time data migrations                                                   |
| `app-management/installation` (generated) | App Management install / uninstall | Runs `scripts/installation/merchant-lifecycle.js` (database and merchant)  |

Shared code lives in `src/lib`: Commerce client, LLM providers, settings, store detection, mailer, database (`db.js`),
merchants, Data regions and Notify me storage.

### Database

The app stores its data in the
[App Builder Database](https://developer.adobe.com/app-builder/docs/guides/app_builder_guides/storage/database)
(MongoDB-compatible, `@adobe/aio-lib-db`). Each workspace has **one database, in one region**.

| Collection             | One document per  | Content                                                                         |
| ---------------------- | ----------------- | ------------------------------------------------------------------------------- |
| `merchants`            | Commerce store    | Store URL, SaaS/PaaS, `associated` or `unassociated`, dates, history            |
| `notify_subscriptions` | product and email | Notify me request: `status` `pending`, `sent` or `failed`, send attempts, error |
| `product_stock`        | product           | Catalog synced by `notify-sync`: stock status, quantity, price                  |
| `sync_runs`            | `notify-sync` run | Products synced, emails sent and failed, purged requests, error                 |

**Data region.** The region to create the database in comes from, in order:

1. the **Data region** chosen in the install form (Adobe Exchange / App Builder Catalog, `configSchema` in
   `app.config.yaml`), passed to the actions as `AIO_DB_REGION`; for your own deploys, `AIO_DB_REGION` in `.env`;
2. the **Data region** field in App Management → Configure.

| Data region                         | Data stored in | Recommended for                                                         |
| ----------------------------------- | -------------- | ----------------------------------------------------------------------- |
| Americas (US)                       | United States  | United States, Canada, Mexico, Central and South America, the Caribbean |
| Europe, Middle East and Africa (EU) | European Union | European Union, EEA, United Kingdom, Switzerland, Middle East, Africa   |
| Asia Pacific (Japan)                | Japan          | Japan, India, China, Hong Kong, Singapore, South Korea, Taiwan, SE Asia |
| Australia                           | Australia      | Australia, New Zealand, Pacific islands                                 |

The Admin page recommends a region for the store, from its time zone (else its locale's country).

**Provisioning.** When the merchant installs the app in App Management, the custom installation step
(`scripts/installation/merchant-lifecycle.js`, declared in `installation.customInstallationSteps` in
`app.commerce.config.ts`):

1. creates the database in the chosen region,
2. waits until it is ready (up to 5 minutes),
3. creates its collections and indexes,
4. records the store in `merchants`.

If the database isn't ready in time, or no region was chosen yet, the installation still completes. The app then
finishes the setup the next time it needs the database: the scheduled `notify-sync`, or opening the Admin page.

**Changing the region.** The database can't be moved. Changing the Data region afterwards has no effect, and the Admin
page shows a warning. To move it, unassociate the app, delete the database (`aio app db delete`, which deletes **all**
its data), set the new region, deploy and install again.

**Associate / unassociate.**

- When a merchant associates and installs the app, the store is added to `merchants` (or set back to `associated`).
- When a merchant unassociates the app, the store's `notify_subscriptions`, `product_stock` and `sync_runs` documents
  are deleted. The `merchants` document is kept, set to `unassociated`, with the deleted counts and the history. The
  database itself is kept, and is never created when unassociating.

**Store isolation.** Every document has a `store_id` (derived from the store's Commerce URL) and a `store_env` (`saas`
or `paas`), set on the server only. Every query is limited to the associated store and every index starts with
`store_id`, so one store's data is never shown or emailed for another.

**Credentials.** The actions reach the database with the workspace's `AIO_COMMERCE_AUTH_IMS_*` credentials, whose
scopes include `adobeio.abdata.manage` (needed to create the database, collections and indexes). Add the **App Builder
Data Services** API to each workspace so the credential has these scopes.

**After each deploy.** The `post-app-deploy` hook runs `scripts/db-setup.js`, which creates any missing collections
and indexes when the database exists (it uses `IMS_OAUTH_S2S_*` from `.env`), then the `db-setup` action, which runs
the one-time data migrations. Both are safe to run again. Run the script by hand with `node scripts/db-setup.js`.

**Retention.** `notify-sync` deletes Notify me requests emailed more than 180 days ago.

**Migrating from the JSON files.** Before the database, Notify me requests were stored in App Builder file storage
(`product/notify.json` and `products.json`, see `NOTIFY_FILE_PATH` and `PRODUCTS_FILE_PATH`). The first `db-setup` run
after the database exists copies them into it, once. The files are left as they are.

**Look at the data** with the `aio app db` commands, for the workspace the CLI is set to (`aio where`):

```bash
aio app db status                                  # provisioning state and region
aio app db collection list                         # the 4 collections
aio app db doc find merchants '{}'
aio app db doc find notify_subscriptions '{"status":"pending"}'
aio app db doc find sync_runs '{}' -o '{"started_at":-1}' -l 5
```

### Configure Notify Me Schedule

The sync interval is set by the `notify-sync-every-15m` trigger in `app.config.yaml`. Change `minutes` to suit your
needs and redeploy.

### Distribution (Adobe Exchange / App Builder Catalog)

`app.config.yaml` declares a `configSchema` with the **Data region** field, so merchants choose it when they install
the app. Package the app with `aio app pack` and upload `dist/app.zip` in Adobe Developer Distribution (private: App
Builder Catalog; public: Adobe Exchange). Test it with a private install into a test org before publishing.

---

## 🧪 Testing Checklist

- Ask a product question → the bot answers from the catalog
- Ask for recommendations → products with links and prices are shown
- Tap a quick-question chip → it is sent as a message
- Add a product to the cart from the chat → the storefront cart and the widget's cart badge update
- Track an order → the order status is shown
- Ask a store policy question → the answer comes from CMS pages
- Ask for a human → the bot points to the store's contact details
- Click **Notify me** on an out-of-stock product → the email is sent once it is back in stock
- Admin page → **Database** shows **Connected** and the Data region

```bash
npm test          # all unit tests (vitest)
aio app test      # the same tests, run per extension
```

---

## 🛠 Debugging & Monitoring

### Chatbot Status

Commerce Admin → **AI Chatbot** shows the store connection, the database and its region, the AI provider and where
each setting comes from.

### Action Logs

```bash
aio rt:activation:list
aio rt:activation:log <activationId>
```

The installation step logs to the `app-management/installation` action:

```bash
aio rt activation list app-management/installation
```

### Local Development

```bash
aio app dev -e application
```

The Notify me schedule runs only in deployed apps. `aio app dev` can't reach a production workspace's database.

---

## 📄 License

See [LICENSE](LICENSE) and [COPYRIGHT](COPYRIGHT).

## 📩 Support

For advanced features or enterprise capabilities, contact:
18th Digitech Pvt Ltd Team: **support@18dapps.com**
