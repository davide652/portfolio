# Davide Mirza — Portfolio Website

A professional static portfolio site, ready to deploy to GitHub Pages with your custom domain.

## Structure

```
site/
├── index.html                  Home: single scrolling page (opener, experience,
│                               12 project carousels, archives, about, contact)
├── projects.html               Full project grid
├── resume.html                 Resume page (embeds assets/Davide-Mirza-Resume.pdf)
├── css/style.css               ALL styling — every page links this one file
├── images/                     All photos, local (121 recovered from the old
│                               Google Site, resized to max 2000px and
│                               re-encoded as progressive JPEG: 38.5MB -> 9.4MB)
├── assets/                     Davide-Mirza-Resume.pdf (served + downloadable)
├── experience/
│   ├── sharkninja.html         SharkNinja internship (NPD, Summer 2026)          
│   ├── apoteca.html            Loccioni APOTECA internship (full detail)
│   └── loccioni.html           About Loccioni & its departments
└── projects/                   12 project case-study pages
```

## Design system

Editorial / grotesk: off-white `#fafaf8`, near-black `#1a1a1a`, no accent colour,
no rounded corners, no shadows. Type is **Inter Tight** (display + UI) with
**JetBrains Mono** used only for image counters. Every colour and size lives in
the `:root` block at the top of `css/style.css` — change it there and the whole
site follows.

### Adding a project carousel to the homepage

Copy an existing `<section class="carousel" data-carousel>` block in
`index.html`, change the year / title / link, and list your images inside
`.stage`. Put `class="on"` on the first image only. The counter, click zones and
swipe handling are wired up automatically; if a carousel has only one image the
counter hides itself.

## Deploy to GitHub Pages — davide-mirza.com

The site is already prepared: `CNAME` (davide-mirza.com), `.nojekyll`, and a styled
`404.html` are in place, and every internal link and image has been crawl-tested.

### 1. Create the repo

1. On GitHub: **+ → New repository**. Name it `portfolio`. Set it **Public**. Don't add a README.
2. On the empty repo page click **uploading an existing file**.
3. Open `Portfolio/site/` in Finder, select **everything inside it** (not the `site` folder
   itself — `index.html` must land at the repo root), and drag it into the browser.
   Do a second drag for the `css`, `images`, `assets`, `projects` and `experience` folders
   if they don't come across the first time.
4. macOS hides dotfiles: press **Cmd+Shift+.** in Finder to reveal `.nojekyll` and include it.
   If it won't upload, use **Add file → Create new file**, name it `.nojekyll`, leave it empty, commit.
5. Click **Commit changes**.

### 2. Turn on Pages

**Settings → Pages → Source: Deploy from a branch → Branch: `main` / `(root)` → Save.**
Wait ~1 minute; the site appears at `https://YOURUSERNAME.github.io/portfolio/`.

### 3. Add the custom domain (do this BEFORE touching DNS)

**Settings → Pages → Custom domain →** type `davide-mirza.com` → **Save**.
GitHub warns that configuring DNS first can let someone else claim your subdomain, so this
order matters.

### 4. GoDaddy DNS

**My Products → davide-mirza.com → DNS → Manage DNS.**

First **delete GoDaddy's defaults**: the parked `A` record on `@` (points at a GoDaddy IP)
and the `CNAME` on `www` pointing to `@`. GitHub's docs are explicit that a leftover
default record must be removed.

Then add:

| Type | Name | Value | TTL |
|---|---|---|---|
| A | @ | 185.199.108.153 | 1 hour |
| A | @ | 185.199.109.153 | 1 hour |
| A | @ | 185.199.110.153 | 1 hour |
| A | @ | 185.199.111.153 | 1 hour |
| CNAME | www | YOURUSERNAME.github.io | 1 hour |

The CNAME value is `YOURUSERNAME.github.io` — **no repo name**, and it needs the trailing
`.github.io`. Optionally also add AAAA records on `@` for IPv6:
`2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`.

### 5. HTTPS

DNS can take up to 24 hours (usually 15–60 minutes on GoDaddy). Once GitHub shows a green
check under Custom domain, tick **Enforce HTTPS**. That checkbox is greyed out until the
certificate is issued — that's normal, come back later.

### Checking it worked

```
dig davide-mirza.com +noall +answer -t A      # should list the four 185.199.x.153 addresses
dig www.davide-mirza.com +noall +answer       # should show a CNAME to YOURUSERNAME.github.io
```

### Updating the site later

Edit files locally, then in the repo: open the file → pencil icon → paste → commit. Or
**Add file → Upload files** and drop in replacements. If you get comfortable with git,
`git clone`, edit, `git push` is faster.

Note: `404.html` links to `/css/style.css` with an absolute path, which is correct for the
custom domain. On the temporary `username.github.io/portfolio/` URL the 404 page will look
unstyled — that resolves itself once the domain is live.


## Things to finish

- **Company logos**: `images/logo-sharkninja.svg` and `images/logo-loccioni.svg` are **typographic placeholders I drew** — not the official marks. Download the real ones from each company's press/brand page and overwrite those two files, keeping the same filenames (SVG or PNG both fine; if PNG, update the `src` in `index.html`). Use a monochrome/black version if one is offered — the CSS greyscales them anyway.
- **SharkNinja page**: written from the sanitised summary Davide provided. Nothing was taken from the internal `Summer.odp` deck — no part numbers, images, recipe data or internal naming appears on the page. Re-check before publishing that the phrase "frozen-treat product" and the load/spring figures are cleared for public sharing.
- **Resume**: built from Orson's template — source in `../resume/resume.html`, exported to
  `../resume/Davide-Mirza-Resume.pdf` and `.docx`, with the PDF copied into `site/assets/` so the
  Resume page displays and downloads it. To update: edit `resume/resume.html`, re-export, re-copy.
  The **Portfolio** link in the header now points at https://davide-mirza.com.
- **Videos — CHECK SHARING**: all 12 demo videos are now embedded as Google Drive players
  (plus the YouTube one on Waffle House). They only play for visitors if each Drive file is
  shared as **Anyone with the link — Viewer**. Open the site in a private/incognito window to
  confirm; if a player shows a sign-in prompt, fix that file's sharing in Drive.
  The embedded file IDs, by page:

  | Page | Drive file ID |
  |---|---|
  | waffle-house | `1zoLy8J3ai6_UDca19MqkhuhCAdV2h0BO` (+ YouTube `1vtvt7qja5g`) |
  | create-3-object-recognition | `1zBVNSSbKW-2PJ5yuFgC5kM4qzWAre9Ga` |
  | create-3-navigation | `194iYjFzLy1kJS_cSKK-2D68mxwZC5dMG`, `1i1X-N-QLf7a7KmKCmkc3FasMSqaLxvsm`, `1vhSxjy_NMu80Z9wKmdTo-i1HY4z-xV8g`, `1ssh1xWRIr-f-8OoWVh1X4nvHcJ8OK6H-` |
  | camera-line-follower | `1njfgEzYctT73xEi0WnUr3ioRy3sU-ucB` |
  | ball-sorter | `1o13vPso8GDMHhth7YN-6qTfnHiOA9VTe` |
  | line-follower | `1bV9Rf02m2Pz0yYfnIkad1fX1egEIUKbB` |
  | demogorgon-grip | `1PsmJUv4rWZKqOavB_Qtek4lDwswLqp8a` |
  | assistive-technology-hackathon | `1lt9fElOlePaSqQkxv189V-ep1y9qee7f` |

- **Two unplaced videos**: the old Archives page had two more Drive clips,
  `1WCtv9fmbvvMbF0o4N-4X_AC3EKsReiGq` and `1bertwcWjUa4M73Sl3ONcmuu6lIzSbcHA`.
  The archive items are text-only rows on the homepage with no pages of their own,
  so they have nowhere to live yet.

- **Switching to self-hosted video later**: download the `.mov` files from Drive into the
  Portfolio folder, re-encode them (`ffmpeg -i in.mov -vcodec h264 -crf 26 -vf scale=1280:-2 -an out.mp4`
  typically cuts an iPhone clip by 80-90%), drop them in `videos/`, and replace each
  `<iframe>` with `<video src="../videos/name.mp4" controls playsinline></video>`.
  The `.video-embed` styling already fits either one.
- **Unused images**: 48 old files (~7.5 MB) in `images/` are superseded originals from the first migration — the `*-hero.jpg`, `lego-1..9.jpg`, `piper-1/2.jpg` and `hackathon-*` sets. Nothing references them any more; safe to delete whenever you like.
- **Archives images**: `archives-01` through `archives-12` are the Hair Dryer CAD renders, the ME21 mechanism and its plots, recovered from the old site but not yet placed — the archives block on the homepage is text-only. Say the word if you want them shown.
- **Image quality**: the Hackathon, Piper and Truss photos were uploaded small to the old Google Site (some under 400px) and that is the highest resolution Google holds. Re-export from your originals if you still have them.
- **27 missing images**: the old site references 148 images; 121 were recovered. The other 27 (mostly Truss and APOTECA) return errors from Google's servers — those tokens appear to be dead.

## Security note

Your old site's Create 3 Navigation page publicly shows a hardcoded Airtable personal access token. It has been redacted in this rebuild — but **revoke that token in Airtable** (Account → Developer hub → Personal access tokens) since it's still exposed on the old site.
