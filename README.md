# 游戏经历库 / Game Library — by Yanghuan Tan

An Astro-powered, bilingual, cover-first game gallery with an optional Sanity CMS. Initial records were transcribed from the user's supplied Steam screenshots and console/mobile notes.

## Run the website locally

```bash
npm install
npm run dev
```

Open the local address printed by Astro (usually `http://localhost:4321`). Without Sanity credentials the website works with the included `src/lib/games.json` starter records.

## Connect the visual Sanity admin

1. Create a free Sanity project at https://www.sanity.io/manage and a `production` dataset.
2. Copy `.env.example` to `.env` and set `PUBLIC_SANITY_PROJECT_ID` and `SANITY_STUDIO_PROJECT_ID` to your project ID. The public dataset must be readable by unauthenticated visitors for this architecture.
3. In Sanity project settings, configure CORS origins for your development and deployed website domains (enable credentials only if needed).
4. Run `npm run studio` to access the independent visual content editor. Sign in with your authorized Sanity account.
5. Add/publish game documents. **Once the Sanity connection is enabled, the public page reads the published CMS dataset, not the starter JSON.** Import your initial records before enabling the CMS in production, or the gallery will initially be empty.
6. Build/deploy the Astro site using `npm run build` and host the `dist/` directory (Netlify, Cloudflare Pages, Vercel etc.). For this browser-fetch design, edits to published Sanity records appear on refresh without a website rebuild.
7. The Sanity Studio itself can be hosted separately via `npx sanity deploy` after setting up the project.

### Importing initial records

The starter data is in `src/lib/games.json`. You can import it into Sanity with a one-off script using an editor token, or manually add/edit entries in Studio. Never expose write tokens in `PUBLIC_` variables or frontend code.

### Notes on data quality

- The gallery is **curated**, not a full Steam inventory. Short unfinished games and incidental online games were excluded by default.
- Steam recorded time and user-reported cross-platform total are distinct fields. Public display uses `hours`.
- Unknown playtime is omitted; completion is shown only when user-confirmed. Achievements are independent of completion.
- Rusty Lake / Cube Escape entries are individual cards, with no invented playtime.
- `coverUrl` for some Steam games is a best-effort Steam CDN link; some may fail and show a typographic fallback. Upload official covers to Sanity for reliable final visuals.
- Some localized game titles and genre tags are provisional and can be revised in Studio.
- The homepage total counts **published game documents**, not the number of results after filtering.
- This is a local project, **not a deployed website**. Publishing requires your own hosting and Sanity credentials.

### Bulk import (recommended)

A ready-to-import `sanity-initial-games.ndjson` file is included. After configuring Sanity and authenticating the CLI with `npx sanity login`, run:

```bash
npx sanity dataset import sanity-initial-games.ndjson production
```

Review the imported game entries in Studio. Imported documents have no image assets yet; add covers individually in Studio. The original starter JSON remains an offline fallback only when no CMS is configured.
