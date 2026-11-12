# AI Commerce Chatbot

AI Commerce Chatbot adds an AI assistant to your Adobe Commerce store that answers shopper questions instantly, at any hour. It helps shoppers find products, see prices and stock, and add items to their cart from the chat. It can also track orders and answer questions about shipping and returns. When a product is out of stock, shoppers can ask to get an email when it's back. You can use Claude, OpenAI or Google Gemini, and change the chatbot's name, look and welcome message to match your store. Setup takes one line of code, and the assistant only answers store-related questions and never saves chat conversations

To install and set it up, see [**INSTALL.md**](INSTALL.md).

## What it does

- A **chat button** shows at the bottom right of every storefront page.
- A shopper asks a question in their own words, for example "Do you have running shoes under $100?" or "Where is my
  order 000123?".
- The chatbot looks up the answer in **your Commerce store**: products, prices, stock, orders and CMS pages. The AI
  provider you choose (Anthropic, OpenAI or Google Gemini) writes the reply.
- The shopper can **add products to the cart** from the chat, or ask to be **notified by email** when an out-of-stock
  product is back.
- You manage everything in the Commerce Admin: settings in **Apps → App Management → Configure**, and status, preview
  and Notify me requests in **Apps → AI Chatbot**.

---

## Features

- **Shopping assistant:** answers product questions, recommends products, shows prices in your store's currency, and
  adds or removes cart items
- **Customer support:** tracks orders, answers shipping, returns and other policy questions from your CMS pages, and
  directs shoppers to your contact details when they need a person
- **Notify me (back in stock):** shoppers confirm their email address, then get an email when the product is back in
  stock
- **Chat widget:** one script tag for Edge Delivery Services and Luma storefronts. You can change the name, welcome
  message, quick questions, icon and colors
- **Your choice of AI:** Anthropic (Claude), OpenAI or Google Gemini, with your own API key
- **Admin page:** **Apps → AI Chatbot** shows the status, a live chat preview, the embed code and the Notify me
  requests
- **Data in your region:** United States, European Union, Japan or Australia
- **Secure by default:** answers only questions about your store, runs only on the storefronts you allow, limits
  messages per hour, and stores keys and passwords encrypted

---

## Good to know

- **AI usage is billed by your AI provider**, separately from the app. Set a spend limit with the provider and use
  the hourly message limits in **Configure** to control costs.
- **Allowed storefronts** must list every storefront address (with and without `www`), or the chat works only in the
  Admin preview.
- The **Data region** can't be changed after installing. Choose the region of your store and customers.
- **Database:** nothing to set up. **Install** creates it in your Data region. It stores only your store's
  association record and Notify me requests with product stock; chat conversations are not stored.
- **Unassociating** the app deletes its saved settings and data.
- New products, stock and prices reach the chat within **15 minutes**.

---

## Support

18th Digitech Pvt Ltd: **support@18dapps.com**

Include your Adobe organization name, the environment name, your store URL and a screenshot of **Apps → AI Chatbot →
Overview**. Never send us your AI API key, SMTP password or Commerce Integration keys.
