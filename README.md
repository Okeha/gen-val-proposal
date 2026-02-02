# Valentine Generator — Personal Link Creator

A small, static web project that helps you create a personalized "Valentine" surprise link you can share with someone special. The generator creates a URL with configuration parameters and `valentine.html` reads those parameters to render a themed, animated, portrait-friendly experience (loader, slot machine, portrait video, and final message).

---

## Quick overview

- Open `index.html` in a browser to build a custom valentines link.
- Fill in the form (partner name, pet name, choose a loader style, video, and final message). Click "Generate Link".
- The app produces a shareable URL (points to `valentine.html` with URL query parameters).
- When the recipient opens that URL, `valentine.html` reads the parameters, applies the selected theme and video, runs a short loader/slot flow, and reveals the final message with confetti/hearts.

## Features

- Simple generator UI to build a personalized link.
- Several loader styles (PrettyLittleThing, TikTok, Instagram) to make the entrant feel native to a platform.
- Portrait-optimized video preview (for reels/portrait clips) using a masked container so videos look great on mobile.
- Personalization parameters passed via URL: loader, name, pet, color, video, msg.
- Charming animations: floating hearts, slot-machine animation, confetti on success.

## URL format / Example

Generated URLs look like:

`https://your.domain/valentine.html?loader=plt&name=Sarah&pet=Baby&color=pink&video=dog&msg=I%20love%20you%20so%20much`

- `loader` — one of `plt` (PRETTYLITTLETHING), `tiktok`, `instagram`.
- `name` — partner full name / display name (used in some headings).
- `pet` — pet name / nickname (used throughout the copy).
- `color` — theme key used by `valentine.html` (e.g., `pink`, `purple`, `red` — check the color map in the file).
- `video` — video key (`dog`, `cat`, `millionaireFrog`) that maps to an MP4 in the project.
- `msg` — final message (URL-encoded).

## Files of interest

- `index.html` — generator UI. Builds the URL from form inputs and shows the result so you can copy/share it.
- `valentine.html` — the landing/experience page that reads URL parameters and runs the animated flow (loader → slot → question → final celebration).
- `*.mp4` — the video files used by the experience (portrait reels are supported and styled by the new `.video-mask`).

If you'd like to tweak anything, these are the primary places to look.

## How it works (high-level)

1. User fills the form on `index.html` and clicks generate.
2. The page constructs a query string using `URLSearchParams` and shows a link like `/valentine.html?name=...&video=...&msg=...`.
3. `valentine.html` parses the URL parameters via `new URLSearchParams(window.location.search)` and builds a `CONFIG` object.
4. The script applies theme variables (colors, glow, gradients) to `:root` CSS variables, updates text, sets the `video` source, then plays the loader animation.
5. After the loader finishes, the slot machine animation runs, then the question screen appears where the user can click the yes/no buttons. Choosing "yes" triggers confetti and the final message.

## Portrait videos and the new video mask

Portrait videos are displayed inside a `.video-mask` element that enforces a tall aspect ratio (9:16). The `video` element is sized to fill the mask using `object-fit: cover` so reels look full-bleed and natural on mobile devices. Captions are shown under the masked video.

If you add new portrait videos, place them in the project root (or update paths) and add a mapping in the `videoMap` in `valentine.html`.

## Customization

- Text and copy: edit text nodes in `index.html` and `valentine.html` (look for `landingTitle`, `finalMessage`, etc.).
- Colors: the theme is driven by CSS variables in `valentine.html` (`--accent-color`, `--accent-gradient`, `--accent-glow`, `--accent-bg`). Modify these or the `colors` map in the script to add new named themes.
- Fonts: swap the Google Fonts links at the top of the files.
- Videos: add new MP4 files and update the `videoMap` in `valentine.html`.

## Development / Local preview

This is a static site — you can preview it locally by opening `index.html` and `valentine.html` in your browser. For a better local dev experience (preventing certain browser restrictions on video autoplay / cross-origin), use a simple static server, for example with Python:

```bash
# Python 3
python -m http.server 8000

# then open http://localhost:8000/index.html
```

Or use any static server (`live-server`, `http-server`, GitHub Pages, Netlify, Vercel, etc.).

## Deployment

Since the project is static, you can deploy to any static host (GitHub Pages, Netlify, Vercel). If you host under a subpath (not root), ensure links in `index.html` generate correct base URLs — the generator currently builds relative links from `window.location.href` (see the `generateLink()` function).

## Notes & tips

- Autoplay: For videos to autoplay muted on mobile, `muted` and `playsinline` are set on the `video` elements. Browsers disallow unmuted autoplay.
- Accessibility: Consider adding meaningful `alt` text equivalents and improving keyboard control for interactive elements (slot machine, run-away button) if you plan broader distribution.
- Optimization: Large MP4s can bloat load time. Consider using short, compressed clips or hosting videos on a CDN.
