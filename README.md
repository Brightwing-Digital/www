# Brightwing Digital — website

A static site built with [Astro](https://astro.build) and Tailwind CSS, hosted on
GitHub Pages. The build output is plain HTML and CSS — no client-side JavaScript.

```
src/pages/            One file per page: index.astro → /, about.astro → /about/
src/layouts/          BaseLayout.astro — <head>, fonts, global styles
src/components/       SiteHeader, SiteFooter; navLinks.ts feeds both nav bars
src/styles/global.css Tailwind import and the Brightwing design tokens (@theme)
src/assets/           Images Astro optimises at build time (resized, converted to WebP)
public/               Copied to the site as-is (logos)
```

## Local development

Requires Node 22.12 or newer.

```bash
npm install
npm run dev
```

The site is at http://localhost:4321 and reloads as you edit. To check the
production output, run `npm run build` (writes `dist/`) and then `npm run preview`.

## Editing

- **Nav links** are defined once in `src/components/navLinks.ts`.
- **Icons** come from `@lucide/astro`: `import { Compass } from '@lucide/astro'`,
  then `<Compass size={22} />`. Browse names at https://lucide.dev/icons.
- **Images** that belong to page content go in `src/assets/` and are rendered with
  Astro's `<Image>` component, which handles sizing and WebP conversion.
- **Fonts** are self-hosted via Fontsource and imported in `BaseLayout.astro`.

## Adding a page

Create `src/pages/<name>.astro`, wrap it in `<BaseLayout>` (which sets the
title, description, canonical URL and link-preview tags), and put the content
between `<SiteHeader current="/<name>/" />` and `<SiteFooter />` inside a
`<main>`. Add it to `navLinks.ts` if it belongs in the nav. The sitemap picks up
new pages automatically.

## Icons and link previews

`public/` holds `favicon.ico` (16/32/48px), `icon-192.png`,
`apple-touch-icon.png` and `og-image.png` (the 1200×630 image shown when the
site is shared in iMessage, Slack, LinkedIn and so on). Replace those files to
change them; the tags that reference them are in `BaseLayout.astro`.

## Deploying

The site is served from https://brightwingdigital.com via GitHub Pages.
`.github/workflows/deploy.yml` builds and deploys on every push to `main`.

One-time setup in the GitHub repository, under **Settings → Pages**:

- **Source:** GitHub Actions
- **Custom domain:** `brightwingdigital.com`, then tick **Enforce HTTPS** once
  the certificate is issued

Because the site deploys from a custom workflow, GitHub ignores any `CNAME`
file; the custom domain lives in the repository settings. The domain is also set
as `site` in `astro.config.mjs`, which the canonical URLs, link previews and
sitemap use.
