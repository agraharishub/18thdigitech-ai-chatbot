# Installation Guide

AI Commerce Chatbot

This document gives step-by-step instructions to install, configure and deploy the AI Commerce Chatbot app for Adobe
Commerce using Adobe App Builder.

---

## 1. Prerequisites

### System Requirements

- Node.js v24 (see `.nvmrc` and `engines` in `package.json`)
- Adobe I/O CLI v10+ (installed and authenticated)
- Adobe Commerce 2.4.4 or later (SaaS or PaaS), with App Management available

### Access Requirements

- Adobe App Builder
- Adobe Developer Console
- Adobe Commerce Admin

### Credentials Required

- An API key for one AI provider: Anthropic (Claude), OpenAI or Google Gemini
- SMTP server credentials (only for **Notify me** back-in-stock emails)
- Commerce Integration OAuth 1.0a credentials (optional, PaaS only, for stores without Adobe IMS on their REST API)

## 2. Quick Start Installation

There are two ways to install the app:

- **Merchants: acquire it from Adobe Exchange** (section 2.1). Adobe Exchange deploys the app into your organization:
  no code, CLI or `.env` needed.
- **Developers: deploy it from the source code** (section 2.2), with the Adobe I/O CLI.

In both cases, the store is connected in **Apps → App Management** of the Commerce Admin, and the App Builder Database
follows the association: it is created when the app is installed, and deleted with all its data when the app is
unassociated.

### 2.1 Install from Adobe Exchange

1. **Acquire the app.** Sign in to [Adobe Exchange](https://exchange.adobe.com/) with your Adobe ID, open **AI Commerce
   Chatbot** and acquire it for your organization.
2. **Select an environment and deploy.** In Adobe Exchange, open the **Manage** tab, select **AI Commerce Chatbot**,
   select the environment of your organization to install it in (for example Stage or Production) and deploy the app.
3. **Check the Developer Console.** Deploying creates the project and the environment (workspace) in the
   [Adobe Developer Console](https://developer.adobe.com/developer-console/) of your organization, and deploys the app
   in it. The App Builder Data Services API and its credentials are added for you.
4. **Open App Management in your Commerce Admin:**
   - **SaaS** (Adobe Commerce as a Cloud Service): the Commerce Admin of your instance → **Apps → App Management**.
   - **PaaS** (Adobe Commerce on cloud infrastructure or on-premises): the backend Admin → **Apps → App Management**.
     The Admin UI SDK must be installed (section 4).
5. **Associate the app.** Select **AI Commerce Chatbot**, click **Associate** and fill in the credentials asked for:
   the organization, project and environment (workspace) where the app was deployed in step 2.
6. **Configure it.** Click **Configure**, choose the **Data region** first, then enter the other settings (section 8,
   step 3) and save.
7. **Install it.** Click **Install**: the App Builder Database is created in the Data region (it takes a few
   minutes), with its collections and indexes, and the store is recorded.
8. **Add the chat widget** to the storefront (section 9).

**Unassociating** the app in App Management deletes the database with all its data and the values saved in
**Configure**. Associating it again starts from scratch: choose the Data region again, then install.

### 2.2 Deploy from the Source Code

1. Download the AI Commerce Chatbot app code.
2. Create an App Builder project in the Developer Console, with the **App Builder Data Services** API (section 3).
3. Install dependencies and select the project and workspace (section 5).
4. Create the `.env` file (section 6).
5. Deploy the app with `aio app deploy` (section 7).
6. Associate the app in **Apps → App Management**, **Configure** it (with the **Data region**), then **Install**: this
   creates the database (section 8).
7. Add the widget script to the storefront (section 9).

---

## 3. Configure Required Services

Create an App Builder project in the Developer Console with the required services:

1. Log in to the [Adobe Developer Console](https://developer.adobe.com/developer-console/) and select your
   organization from the dropdown in the top-right corner.
2. Click **Create project from template**.
3. Select **App Builder**. The **Set up templated project** page opens.
4. Enter a project title and app name. Check **Include Runtime with each workspace**.
5. Create the workspaces (Stage/Production).
6. Add an **OAuth Server-to-Server** credential to each workspace:
   - **SaaS:** add the **Adobe Commerce as a Cloud Service** API, which provides the `commerce.accs` scope.
   - **PaaS:** use the credential that your IMS-enabled Commerce instance trusts (see section 4).
7. Add the **App Builder Data Services** API to each workspace. It gives the credential the database scopes
   (`adobeio.abdata.read`, `adobeio.abdata.write` and `adobeio.abdata.manage`).

---

## 4. Store Connection: SaaS vs PaaS

The app does not need the store URL or credentials in `.env`. The store comes from the App Management association,
and access uses Adobe IMS.

### Base URL Differences

- PaaS: `https://<environment-name>.magentosite.cloud/`
- SaaS: `https://na1.api.commerce.adobe.com/<tenant-id>/`

### Authentication Precedence

- Commerce Integration (OAuth 1.0a) is used on PaaS only when all four Commerce Integration settings are filled in
  **Configure**.
- Adobe IMS is used in every other case. SaaS accepts only IMS.

### Authenticate with IMS

- For SaaS, IMS is included by default. The workspace credential needs the `commerce.accs` scope.
- For PaaS, the Commerce instance needs
  [**Adobe Identity Management Service (IMS) for Adobe Commerce**](https://experienceleague.adobe.com/en/docs/commerce-admin/start/admin/ims/adobe-ims-integration-overview)
  configured. If it isn't, fill in the Commerce Integration settings in **Configure** (section 8).

### App Management and Admin UI SDK

- For PaaS, install the
  [**Admin UI SDK**](https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/installation/) and make sure
  **Apps → App Management** is available in the Commerce Admin.
- For SaaS, both are included.

---

## 5. Initialize

```bash
npm install
aio login
aio console org select
aio console project select
aio console workspace select
aio app use --merge
```

`npm install` also runs the App Management generator (`postinstall`). It creates the generated App Management files
and `AIO_COMMERCE_CONFIG_ENCRYPTION_KEY`.

---

## 6. Environment Configuration

### 6.1 Create .env File

Run this in the project root:

```bash
cp env.dist .env
```

`.env` only holds developer and platform values. The merchant settings (AI provider and key, chatbot name, allowed
storefronts, SMTP, Commerce Integration credentials) are **not** set here: they are entered in **App Management →
Configure** (section 8), and never read from `.env`.

### 6.2 Data Region

Where the store's data is stored, chosen by the merchant in **App Management → Configure → Data region** (section
8). The database is created in this region when the app is **installed**, and deleted with all its data when the app
is **unassociated**.

| Value  | Data stored in | Recommended for                                                         |
| ------ | -------------- | ----------------------------------------------------------------------- |
| `amer` | United States  | United States, Canada, Mexico, Central and South America, the Caribbean |
| `emea` | European Union | European Union, EEA, United Kingdom, Switzerland, Middle East, Africa   |
| `apac` | Japan          | Japan, India, China, Hong Kong, Singapore, South Korea, Taiwan, SE Asia |
| `aus`  | Australia      | Australia, New Zealand, Pacific islands                                 |

Also check your data protection requirements (for example GDPR: `emea`).

### 6.3 Adobe IMS Credentials

These are copied from the Developer Console workspace into `.env` by `aio app use` and the build hook. You don't
need to enter them.

```env
AIO_COMMERCE_AUTH_IMS_CLIENT_ID=
AIO_COMMERCE_AUTH_IMS_CLIENT_SECRETS=
AIO_COMMERCE_AUTH_IMS_TECHNICAL_ACCOUNT_ID=
AIO_COMMERCE_AUTH_IMS_TECHNICAL_ACCOUNT_EMAIL=
AIO_COMMERCE_AUTH_IMS_ORG_ID=
AIO_COMMERCE_AUTH_IMS_SCOPES=
IMS_OAUTH_S2S_CLIENT_ID=
IMS_OAUTH_S2S_CLIENT_SECRET=
IMS_OAUTH_S2S_ORG_ID=
IMS_OAUTH_S2S_SCOPES=
```

`AIO_COMMERCE_AUTH_IMS_SCOPES` and `IMS_OAUTH_S2S_SCOPES` must include `adobeio.abdata.manage`. If they don't, add
the App Builder Data Services API to the workspace (section 3) and run `aio app use --merge` again.

### 6.4 Encryption Key

```env
AIO_COMMERCE_CONFIG_ENCRYPTION_KEY=
```

This key encrypts the password settings saved in **Configure** (AI API key, SMTP password, Commerce Integration
credentials). It is 64 hex characters and is generated by `npm install`, or by the build when `.env` has none.
**Keep it the same across deploys, and back it up**: a deploy with another key (for example after `.env` was
recreated, or `aio app use` without `--merge`) makes **Configure** fail with "Configuration load error". Use
`aio app use --merge` so `.env` keeps it.

### 6.5 Other Values

```env
LOG_LEVEL=info
NOTIFY_FILE_PATH=product/notify.json   # legacy, see below
PRODUCTS_FILE_PATH=products.json       # legacy, see below
```

The two file paths are only used once, to copy Notify me requests saved by older versions of the app (in App Builder
file storage) into the database. Leave the defaults on a new installation.

---

## 7. Deploy

```bash
aio app deploy
```

This deploys all extensions: App Management actions (including the installation with the custom step), the Configure
form backend, the **AI Chatbot** Admin page and the chatbot actions (`chat`, `widget`, `notify`, `notify-confirm`,
`notify-sync`, `status`, `notify-data`, `notify-delete`, `db-setup`).

After the deploy, the `post-app-deploy` hook:

- runs `scripts/db-setup.js`: prints `No database yet …` on a first deploy (the database is created when the app is
  installed, section 8), or `Database ready … (region …): 4 collections, 8 indexes` afterwards;
- runs the `db-setup` action, which copies the legacy Notify me files into the database once.

The `notify-sync` action is scheduled to run every 15 minutes after deployment.

---

## 8. Connect, Install and Configure in Commerce Admin

1. In the Commerce Admin, open **Apps → App Management** (SaaS: the Commerce Admin; PaaS: the backend Admin).
2. Find **AI Commerce Chatbot**, click **Associate** and fill in the organization, project and workspace the app is
   deployed in.
3. Click **Configure** and enter the merchant settings:
   - **Data region** (section 6.2): choose it before installing. To change it later, unassociate the app (this deletes
     the database and its data), then associate it again.
   - **AI provider**, **AI model** (empty = the provider's default) and **AI API key**
   - **Chatbot name** (shown in the chat widget) and **Store name**
   - **Allowed storefronts:** the storefront origins, comma-separated (required: while empty, only the Admin chat preview works)
   - **Chat messages per shopper per hour** and **for the store per hour:** rate limits (default 60 and 3000, 0 = no
     limit)
   - **Product page URL template** and **Media base URL:** only for Edge Delivery Services / headless storefronts;
     leave empty on Luma to detect them from the store
   - **SMTP host**, **port**, **secure connection**, **username**, **password** and **email sender** (required for
     Notify me: the confirmation and back-in-stock emails)
   - **Commerce Integration** consumer key / secret and access token / secret: PaaS stores without Adobe IMS only
4. Save. The values apply within about a minute.
5. Click **Install**. The installation creates the App Builder Database in the Data region, waits until it is ready
   (up to 5 minutes), creates its collections and indexes and records the store.
6. Open the **AI Chatbot** Admin page:
   - **Database** shows **Connected** and the **Data region**, with the region recommended for the store.
   - Copy the embed code from **Storefront chat widget**.

**Unassociate.** Unassociating the app runs the uninstallation: it deletes the App Builder Database with all its data
(`aio app db status` then shows it as not provisioned) and the values saved in **Configure**, in every scope. To
install it again, associate it, configure it (with the Data region) and install it.

---

## 9. Add the Widget to the Storefront

Add the script shown on the **AI Chatbot** Admin page:

```html
<script
  defer
  src="https://<namespace>.adobeioruntime.net/api/v1/web/chatbot/widget"
></script>
```

- **Edge Delivery Services:** load it from `scripts/delayed.js` (example below) or add it to `head.html`.
- **Luma:** Content → Design → Configuration → HTML Head → Scripts and Style Sheets.

Add the storefront origin to **Allowed storefronts**, or the `chat` and `notify` endpoints refuse the requests.

### 9.1 Customize the Widget

Optional settings go in `window.CHATBOT_CONFIG`, set **before** the widget script loads. For example, on an Edge
Delivery Services storefront (`scripts/delayed.js`), with the cart drop-in so the bot changes the shopper's own cart:

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

On Luma, put the same `window.CHATBOT_CONFIG = { … }` (without `cartApi`) in a `<script>` before the widget script.

| Key           | What it changes                                                                    |
| ------------- | ---------------------------------------------------------------------------------- |
| `title`       | Name in the header (default: the **Chatbot name** setting)                         |
| `subtitle`    | Line under the name (default "Online")                                             |
| `welcome`     | First message                                                                      |
| `suggestions` | Quick-question chips shown before the first message                                |
| `placeholder` | Text in the message box                                                            |
| `avatarIcon`  | Header icon: `bag` (default), `wine`, `sparkle` or `chat`                          |
| `avatarUrl`   | An image for the avatar instead of an icon                                         |
| `theme`       | Colors: `accent`, `accentText`, `background`, `surface`, `border`, `text`, `muted` |
| `cartApi`     | The cart drop-in API (Edge Delivery Services)                                      |
| `cartMode`    | `storefront` (Luma session cart) or `bot` (cart managed by the bot)                |
| `cartUrl`     | Cart page for the header cart icon, when the bot manages the cart                  |
| `open`        | Open the chat when the page loads                                                  |

All keys are listed at the top of `src/commerce-extensibility-1/actions/chatbot/widget/chat-widget.js`. The widget
script is cached for 60 seconds.

---

## 10. Testing

### Unit Tests

```bash
npm test                # all tests (vitest)
aio app test            # the same tests, run per extension
npm run test:coverage   # with coverage
npm run typecheck       # Admin UI types
```

### Functional Checklist

- Admin page → **Database** shows **Connected** and the chosen Data region
- Open the chat → the header shows the chatbot name, the cart badge and the quick-question chips
- Ask a product question → the bot answers with products from the catalog
- Ask for recommendations → products with links and prices are shown
- Add a product to the cart from the chat → the storefront cart and the cart badge update
- Track an order with the order number and email → the order status is shown
- Ask a store policy question → the answer comes from CMS pages
- Ask an unrelated question → the bot refuses
- Ask for a human → the bot points to the store's contact details
- Click **Notify me** on an out-of-stock product → a confirmation email arrives and the request shows as **Awaiting
  confirmation** on the Admin page; after confirming, the back-in-stock email is sent once the product is back in stock
- Notify me tab → **Delete a shopper's data** with that email → their requests are deleted
- Send more than the hourly limit of chat messages → the widget shows "You've sent a lot of messages…"
- Unassociate the app → the database is deleted (`aio app db status` shows it as not provisioned) and **Configure**
  is empty when the app is associated again

### Database Checks

```bash
aio app db status                    # PROVISIONED, in the chosen region
aio app db collection list           # merchants, notify_subscriptions, product_stock, sync_runs
aio app db doc find merchants '{}'
```

---

## 11. Distribution (Adobe Exchange / App Builder Catalog)

```bash
aio app pack   # creates dist/app.zip
```

Upload `dist/app.zip` in Adobe Developer Distribution: **private** (App Builder Catalog of your organization) or
**public** (Adobe Exchange, reviewed by Adobe). Merchants then acquire and deploy it as in section 2.1. Before
publishing, acquire it privately in a test organization, deploy it to an environment, associate it, choose a Data
region, install it in App Management and check `aio app db status` for that workspace; then unassociate it and check
that the database is deleted.

---

## 12. Debugging & Monitoring

```bash
aio rt:activation:list
aio rt:activation:log <activationId>
aio rt activation list app-management/installation   # the installation step (database and merchant)
```

Run the actions locally with:

```bash
aio app dev -e application
```

The `notify-sync` schedule only runs in deployed apps, not with `aio app dev`, and `aio app dev` can't reach a
production workspace's database.

More details:
https://developer.adobe.com/commerce/extensibility/app-development/best-practices/logging-troubleshooting/

---

## 13. Troubleshooting

- **Chat doesn't open or returns an error:** check that the storefront origin is in **Allowed storefronts** and that
  the AI API key is valid. The **AI Chatbot** Admin page shows the status.
- **Store not connected:** check the App Management association. On SaaS, make sure the workspace credential has the
  `commerce.accs` scope. On PaaS, configure IMS or fill in the Commerce Integration settings.
- **Database "Not created yet":** choose the Data region in **Configure**, then open
  the **AI Chatbot** page or wait up to 15 minutes. Creating it takes a few minutes.
- **`Missing required scope: adobeio.abdata.manage`:** add the App Builder Data Services API to the workspace, run
  `aio app use --merge` and deploy again.
- **Database in the wrong region:** it can't be moved. Unassociate the app (this deletes the database and **all** its
  data), associate it again, choose the new Data region in **Configure** and install.
- **Configure shows "Configuration load error" / "Failed to fetch config values":** the passwords saved in
  **Configure** were encrypted with another `AIO_COMMERCE_CONFIG_ENCRYPTION_KEY` (the `app-management/config`
  activation logs `Failed to decrypt value`). Restore the old key in `.env` and deploy again. Without it, unassociate
  the app (this clears the saved values and deletes the database), associate it again and enter the settings again.
- **Wrong product links or images:** set the product page URL template and media base URL in **Configure** (needed
  for non-Luma storefronts).
- **Notify me answers "Notify me is not available right now":** set the SMTP host and email sender in **Configure**.
- **Chat answers "You've sent a lot of messages…":** the hourly rate limit was reached; raise it in **Configure**.
- **Notify me emails not sent:** check that the shopper confirmed the request (not **Awaiting confirmation**), the SMTP
  settings and find the `notify-sync` activation with
  `aio rt:activation:list`, then read its log.
- **`aio app test` fails with `jest: not found`:** an extension has no `test` hook; every `ext.config.yaml` and
  `app.config.yaml` must define one (this project uses vitest).

---

## 14. References

- https://developer.adobe.com/app-builder/docs/
- https://developer.adobe.com/app-builder/docs/guides/app_builder_guides/storage/database
- https://developer.adobe.com/app-builder/docs/guides/app_builder_guides/distribution
- https://developer.adobe.com/commerce/extensibility/
- https://developer.adobe.com/commerce/extensibility/admin-ui-sdk/
- https://experienceleague.adobe.com/en/docs/commerce-admin/start/admin/ims/adobe-ims-config
