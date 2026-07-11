# @blackcatofc/baileys

Black Cat Studio විසින් maintain කරන Baileys fork එකක් — Sakura XD සහ Mezuka MD bots සඳහා use කරන අතරේ, extra interactive-button support එකක් සමඟ.

> Based on the official [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys) library.

---

## 📦 Install

```bash
npm install github:NimeshMihiranga-Neno/baileys
```

nathnam `package.json` eke:

```json
"dependencies": {
  "@blackcatofc/baileys": "github:NimeshMihiranga-Neno/baileys"
}
```

---

## ✨ Features

- ✅ Multi-device WhatsApp socket connection (QR / Pairing code)
- ✅ Full interactive button system (`nativeFlow`) — see below
- ✅ Newsletter/channel helper functions (follow, unfollow, mute, metadata)
- ✅ Media handling (image, video, audio, sticker, document) with caching
- ✅ Sticker pack creation
- ✅ Poll creation & vote aggregation
- ✅ Group invite messages with auto thumbnail
- ✅ Carousel / product / catalog message support
- ✅ Album (multi-image/video) messages
- ✅ View-once, ephemeral, spoiler, edit message wrapping
- ✅ No forced newsletter auto-follow, no hidden media annotation injection

---

## 🔘 Button Types (`nativeFlow`)

Meම library eken **21ක් vitara** button shortcuts support karanawa. `sock.sendMessage()` call ekedi `nativeFlow.buttons` array eka athule use karanna.

### Basic

| Shortcut key | Button type | Example |
|---|---|---|
| `id` | Quick reply | `{ text: "Menu", id: ".menu" }` |
| `url` | Open link | `{ text: "Visit", url: "https://..." }` |
| `call` | Call number | `{ text: "Call", call: "+94xxxxxxxxx" }` |
| `copy` | Copy code | `{ text: "Copy", copy: "PROMO50" }` |
| `sections` | List / single select | `{ text: "Select", sections: [...] }` |

### Extended

| Shortcut key | Button type |
|---|---|
| `reminder` | `cta_reminder` |
| `cancelReminder` | `cta_cancel_reminder` |
| `address` | `address_message` |
| `location: true` | `send_location` |
| `catalog` | `catalog_message` |
| `products` | `mpm` (multi-product message) |
| `otp` | `otp_button` |
| `oneTapOtp` | `authentication_button` |
| `flow` | `flow_action` (WhatsApp Flows) |
| `voiceCall` | `voice_call` |
| `videoCall` | `video_call_button` |
| `phoneNumber` | `call_button` (legacy) |
| `urlBtn` | `url_button` (legacy) |
| `reply` | `reply_button` (legacy) |
| `card` | `card_message` |
| `orderDetails` | `order_details` |

> ⚠️ Extended button types (`otp_button`, `flow_action`, `mpm`, `card_message` wage) WhatsApp Business accounts walata witharai reliably wada karanawa. Regular personal bot accounts walata `quick_reply`, `cta_url`, `cta_call`, `cta_copy`, `single_select` witharai guarantee ekක් thiyenne.

### Usage Example

```js
await sock.sendMessage(jid, {
  text: "Choose an option:",
  nativeFlow: {
    buttons: [
      { text: "🏠 Menu", id: ".menu" },
      { text: "🌐 Website", url: "https://blackcatofc.com" },
      { text: "📞 Call us", call: "+94xxxxxxxxx" },
      { text: "📋 Copy code", copy: "SAKURA10" }
    ]
  }
}, { quoted: msg });
```

Image/video ekකත් sameta button danna:

```js
await sock.sendMessage(jid, {
  image: { url: "https://example.com/banner.jpg" },
  caption: "Check this out!",
  nativeFlow: {
    buttons: [
      { text: "Order", id: ".order" },
      { text: "Track order", url: "https://track.example.com" }
    ]
  }
}, { quoted: msg });
```

List/section button:

```js
await sock.sendMessage(jid, {
  text: "Pick a category",
  nativeFlow: {
    buttons: [{
      text: "📋 Categories",
      sections: [
        {
          title: "Anime",
          rows: [
            { title: "One Piece", rowId: ".anime onepiece" },
            { title: "Naruto", rowId: ".anime naruto" }
          ]
        }
      ]
    }]
  }
});
```

---

## 📨 Receiving Button Replies

Button click eka message handler ekedi mehema catch karanna puluwan:

```js
sock.ev.on('messages.upsert', async ({ messages }) => {
  const msg = messages[0];
  if (!msg?.message) return;

  const buttonId =
    msg.message?.interactiveResponseMessage?.nativeFlowResponseMessage?.paramsJson;

  if (buttonId) {
    const parsed = JSON.parse(buttonId);
    console.log('Button clicked:', parsed.id);
    // route to your command handler here
  }
});
```

---

## 📡 Newsletter / Channel Functions

```js
await sock.newsletterFollow(jid);
await sock.newsletterUnfollow(jid);
await sock.newsletterMute(jid);
await sock.newsletterMetadata('jid', jid);
await sock.newsletterSubscribers(jid);
```

> Meම fork eke **automatic/forced channel follow eka නෑ** — user/bot eken explicit widihata call karanනම් witharai run wenne.

---

## 🛠 Requirements

- Node.js `>= 20.0.0`
- One of: `sharp`, `@napi-rs/image`, or `jimp` (image processing, optional peer dep)

---

## 📄 License

MIT — © Nimeshka Mihiran (Neno) / Black Cat Studio

---

## 🖤 Credits

Built on top of [WhiskeySockets/Baileys](https://github.com/WhiskeySockets/Baileys). Maintained and extended by **Black Cat Studio**.

