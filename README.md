# Alendis — Landing Page

Static landing page. No build step: plain HTML + JS, served as-is.

## Deploy to Vercel

1. Push this folder as the repo root:
   ```bash
   git init && git add . && git commit -m "Alendis landing v3"
   git remote add origin git@github.com:<org>/alendis-landing.git
   git push -u origin main
   ```
2. In Vercel → **Add New Project** → import the repo.
3. Settings:
   - **Framework Preset:** Other
   - **Build Command:** *(leave empty)*
   - **Output Directory:** `.` (root)
   - **Install Command:** *(leave empty)*
4. Deploy. `vercel.json` handles clean URLs and asset caching.

## Structure

```
index.html        landing page (entry)
support.js        runtime
image-slot.js     image placeholder component
assets/           logos, horse-gait icons, video loops, stills
vercel.json       routing + cache headers
```

## Notes

- `#pricing` is the anchor for intent-driven buyers arriving without a `/pricing` page.
- Two-path CTA: `Get started` (primary) + `Try one free analysis first` (secondary).
- Palette matches the dashboard: `#9A5735` primary, `#C08457` accent, `#FAF7F3` light sections.
- Videos are committed directly. If the repo grows past comfort, move `assets/*.mp4` to a CDN or Vercel Blob and swap the `src` paths in `index.html`.
