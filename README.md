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
4. Deploy.

## Structure

```
index.html        landing page (entry)
support.js        runtime — index.html does not render without it
image-slot.js     image placeholder component
assets/           logos, gait icons, 7 video loops, stills
vercel.json       redirects + cache headers
```

## Routing handled in vercel.json

| Route | Behaviour | Why |
|---|---|---|
| `/pricing` | → `/#pricing` | no standalone pricing page exists; intent buyers type this |
| `/plans` | → `/#pricing` | same |
| `/signup` | → `/` | was 404ing; regression from the `/auth/login` redirect |

`/signup` currently lands on the homepage. If the `307 → /auth/login` behaviour
is restored upstream, remove that entry so the app owns the route again.

## Video autoplay — do not regress this

All seven videos must carry `muted` **and** `playsinline`, and the page sets
`muted`/`loop`/`playsInline` imperatively in `componentDidMount` because the
runtime drops empty boolean attributes. Without the imperative pass browsers
block autoplay and the hero renders black.

There is also a one-shot retry bound to the first `touchstart`/`pointerdown`/
`scroll` for mobile low-power mode.

After any change to the video markup, verify in the console:

```js
[...document.querySelectorAll('video')].map(v => ({
  src: v.src.split('/').pop(), muted: v.muted, paused: v.paused, loop: v.loop
}))
```

Expect `muted: true` on all seven. Expect `paused: false` on all except
`hero-loop-2.mp4`, which waits its turn in the hero crossfade.

## Weight

Videos are ~29 MB total, committed directly. If the repo gets uncomfortable,
move `assets/*.mp4` to a CDN or Vercel Blob and update the `src` paths.
