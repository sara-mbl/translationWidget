# Translation Widget Studio

A no-build, single-file design studio for creating and customizing Google Translate website widgets. Pick a layout, tune colors and typography with live preview, and copy a ready-to-paste embed — no build step, no dependencies, no signup.

![License: MIT](https://img.shields.io/badge/license-MIT-blue.svg)

## Live demo

Open [`translate_studio.html`](./translate_studio.html) directly in a browser, or visit the [GitHub Pages demo](https://sara-mbl.github.io/translationWidget/translate_studio).

## Features

- **4 layout options** — corner tray, floating action button (FAB), top banner, and inline bar
- **Live preview** — every change renders instantly before you copy anything
- **Full styling control** — background, text, and icon color; border color/thickness; shadow blur, spread, opacity, and 9-direction position; corner radius; line height; per-layout label and button text
- **Custom dropdown UI** — Google's default translate widget is hidden entirely and replaced with a fully styleable dropdown that triggers translation via the `googtrans` cookie
- **22+ languages built in**, plus a language picker for more, plus a custom code/name input for anything not listed
- **Responsive** — the generated embed adapts to mobile (collapses to icon-only on small screens); the studio itself is responsive down to phone width

## Usage

1. Open `translate_studio.html` in any browser
2. Pick a layout, languages, and source language
3. Adjust colors, shadow, border, and spacing in the side panel
4. Copy the generated embed code
5. Paste it into your site — see [Embedding](#embedding) below

## Embedding

The generated snippet is self-contained: CSS, HTML, and JavaScript in one block. Paste it directly into your page, ideally near the end of `<body>`.

**Webflow users:** paste into a single Embed (`w-embed`) element. Do **not** duplicate this embed elsewhere on the same page — having more than one copy of the Google Translate script on a page will cause it to crash silently (`Maximum call stack size exceeded`) and translation will stop working.

## ⚠️ Important: built on an unsupported Google feature

This tool generates embeds based on Google's **legacy Website Translator widget**, which Google officially discontinued in December 2019. It still works as of this writing by:

- Loading `https://translate.google.com/translate_a/element.js` directly
- Reading/writing the `googtrans` cookie to set the page's target language
- Hiding Google's default UI and substituting a custom-styled dropdown

**Because this relies on undocumented, unsupported behavior:**
- Google could change or remove this functionality at any time, without notice
- There is no SLA, support channel, or guarantee of continued availability
- Translation quality and language coverage are entirely dependent on Google's free, automated translation — not a professional/certified translation

**Recommendation:** use this for prototypes, internal tools, small business sites, or situations where "good enough, free, automated translation" is an acceptable tradeoff. For anything mission-critical, legally sensitive, or requiring guaranteed uptime, use a maintained, supported translation API or service instead (e.g. Google Cloud Translation API, DeepL API, or a proper i18n setup with human-reviewed translations).

## How it works (technical notes)

- `window.googleTranslateElementInit` must be defined in the global scope before Google's script loads it — if your page sandboxes embeds (some CMSs do), this can break silently
- A `MutationObserver`/cookie-reload pattern is used instead of directly manipulating Google's injected `<select>`, since that element is unreliable to style and interact with across browsers
- Selecting a language sets a cookie (`googtrans=/{source}/{target}`) and reloads the page; Google's script reads that cookie on load and translates automatically
- Ad blockers and some privacy extensions block `translate.google.com` outright — test in an unmodified browser/profile if translation appears not to work

## Browser support

Works in all modern browsers. Some browsers show their **own native translate prompt** in addition to this widget — this is OS/browser-level UI, not part of the page, and cannot be hidden, styled, or suppressed by any code on the page:

- **Chromium desktop** (Chrome, Edge, Opera, Brave) — "Translate this page?" popup
- **Safari on iOS** — "View this page in: [language] / Translate" bar

These prompts are rendered by the browser shell itself, outside the page's DOM — there is no iframe, no element, nothing CSS or JavaScript can target. Setting an accurate `lang` attribute on `<html>` (matching your page's actual source language) reduces how often these prompts appear, but cannot eliminate them once a browser decides to offer translation. This is expected, unavoidable behavior — not a bug in this widget.

## License

MIT — free to use, modify, and distribute. See [LICENSE](./LICENSE).

## Author

**Sara Bustamante**
[GitHub](https://github.com/sara-mbl/) · [LinkedIn](https://www.linkedin.com/in/sarambl/)
