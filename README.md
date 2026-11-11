# apollo-paints

A catalog website for Apollo Paints, an industrial paint maker. It is live at <https://apollopaintsglobal.com>.

I built it with AI tools. They wrote almost all of the code. I chose the tools, wrote the instructions and decided what went live. This page says what the site is for, which tool did which part, and how I check that the right code is live.

- **Live code:** [`site-v3/`](site-v3/), a React single-page app. The other folders are earlier versions and source material.
- **Made with:** Google Stitch (first screens), Google AI Studio (the React app) and Claude (changes and fixes).
- **Served by:** Cloudflare Workers.

## What the site is for

The maker sells to other businesses. The site has one job: a buyer finds a product line and sends an enquiry.

- It is one page with five sections: home, about, industries, products and contact.
- Four product families hold fifteen product lines. Each product line opens an enquiry form.
- A WhatsApp chat button sits in the bottom-right corner.
- It is an enquiry catalog, not a shop: no prices, no cart and no login.
- Old multi-page addresses such as `/catalog.html` redirect to the matching section, so old links still work.

## Which folder is live

| Folder | What it is |
|---|---|
| [`site-v3/`](site-v3/) | **Live.** React 19, Vite 6, Tailwind CSS 4, TypeScript. |
| [`site-v2/`](site-v2/) | Five static HTML pages with Tailwind CSS from a CDN. An earlier version, kept for reference. |
| [`site/`](site/) | The first prototype, built from the Stitch export. Kept for reference. |
| [`stitch_product_catalog/`](stitch_product_catalog/) | The Google Stitch export: screen HTML, screenshots and design notes. |
| [`apollo-paints.zip`](apollo-paints.zip) | The Google AI Studio export of the React app, before any change. |

## How it was made

1. **Google Stitch** made the first screens. Claude turned them into pages (`site/`) and then revised them (`site-v2/`).
2. **Google AI Studio** generated the React app that is live now. Nine of the eleven files in its `src/` folder are unchanged from the export.
3. **Claude** changed the other two (`Contact.tsx` and `Products.tsx`) so that both enquiry forms send through one shared module. It also added the redirects for the old addresses and removed unused dependencies.
4. **Claude Code** fixed a Cloudflare build that failed.

## Run it locally

```bash
cd site-v3
npm install
npm run dev       # dev server on http://localhost:3000
npm run build     # production build in site-v3/dist
npm run preview   # serve that build locally
npm run lint      # type check only (tsc --noEmit)
```

No API keys or environment variables are needed.

## How it deploys

Cloudflare Workers Builds watches this repository. A push to `main` runs a build and publishes the result. GitHub shows each run as a check named "Workers Builds: apollo-paints". The build settings are kept in the Cloudflare dashboard.

Two things I learned:

- A green build is a signal, not the outcome. To confirm the outcome, I compare the JavaScript file that the website serves with a local build. The same file name and the same bytes mean that the right code is live. On 12 Nov 2026 they matched.
- A `/* /index.html 200` rule in `_redirects` fails the deploy (Cloudflare error 100324). Cloudflare already serves the app for unknown paths, so [`site-v3/public/_redirects`](site-v3/public/_redirects) holds only the 301 redirects.
