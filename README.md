# nikosamofa.com

Two versions of the personal site, both single-file and fully portable. Pick one and deploy it as `index.html` on Cloudflare Pages (or any static host). Everything below applies to both versions unless noted.

---

## Which version to ship

**`index.html`** — the clean, production version.
- 251KB, one file, no dependencies.
- Operator-file aesthetic. Fast, quiet, professional.
- Best for: recruiters loading on locked-down corporate networks, law firm partners on old browsers, anyone on mobile. Loads instantly, renders identically everywhere.

**`index-immersive.html`** — same content, plus a subtle 3D "Operator Field" background.
- 261KB HTML + a ~600KB three.js library loaded from a CDN on page load.
- WebGL point-field behind the content: muted gold points with pulsing "beacons," a subtle wave sweep, and camera dolly on scroll.
- Auto-disables on mobile (< 720px), on `prefers-reduced-motion`, and on browsers without WebGL. Falls back to the clean version seamlessly.
- Best for: your main personal site where you want a signature moment, and where visitors are likely on desktop with modern browsers.

You can ship either one — same URL, same content, same brand. The immersive version rewards visitors who linger; the clean version is bulletproof.

To use the immersive version, just rename `index-immersive.html` to `index.html` before uploading (or set it as the entry file in your host).

---

## Hosting on Cloudflare Pages

1. Cloudflare dashboard → **Workers & Pages** → **Create** → **Pages** → **Upload assets**.
2. Name the project `nikosamofa` (or whatever).
3. Drag your chosen `index.html` onto the drop zone. If you want proper social-share previews, also upload the `assets/` folder so the `og:image` URL resolves.
4. Click **Deploy**. You'll get a `nikosamofa.pages.dev` URL in seconds.
5. In that project → **Custom domains** → add `nikosamofa.com`. Since your DNS is on Cloudflare, one click configures it.

---

## About the "Operator Field" (immersive version only)

Cyber + three.js is dense cliché territory (Matrix rain, spinning wireframe globes, neural network graphs). This effect deliberately avoids all of that. What it does instead:

- A 3D field of ~5,800 muted-gold points drifting behind everything at low opacity
- ~5% of points are **beacons** — larger, brighter, pulsing on a slow travelling wave
- The wave sweeps through the Y axis over time, so the field feels alive but never fast
- **Scroll** dollies the camera forward into the field and tilts it down
- **Mouse movement** gently parallax-shifts the field for depth
- Content sits above with a light backdrop-blur so text stays crisp

Design language: security dashboard, dataspace, operator's console. Not hacker movie.

Performance safeguards baked in:
- Skipped entirely on mobile (< 720px viewport)
- Skipped on `prefers-reduced-motion`
- Skipped if the browser lacks WebGL
- Paused when the tab is hidden (saves battery)
- Uses `low-power` GPU preference
- Antialiasing off (points are small; not worth the cost)

If you ever want to tune it:
- Search for `gridW = 42, gridH = 28, gridD = 5` — density of the field
- Search for `sizes[i]` — point sizes (ambient vs. beacon)
- Search for `uColorBase` / `uColorHi` — color palette (currently `#8E7547` and `#DDB97A`)
- Search for `alpha = shape * vBrightness` in the fragment shader — brightness

---

## Wiring up the newsletter form

Both versions include a placeholder newsletter form that submits to `#` and shows a fake success message. To make it real, pick a provider and swap in their form endpoint.

**Recommended: Beehiiv.** Free up to 2,500 subscribers. Built for growth (referral system, paid newsletter optional later). Doesn't trap your list the way Substack does.

1. Create a Beehiiv account → create your publication ("The Operator's Notebook").
2. Beehiiv → Settings → Subscribe Forms → Embed form → copy the form's `action` URL and input `name` attribute.
3. In your chosen HTML file, find the `<form class="newsletter-form">` block (search for "NEWSLETTER FORM INTEGRATION") and:
   - Replace `action="#"` with the Beehiiv URL.
   - Make sure the `<input>` `name` attribute matches what Beehiiv expects (usually `email`).
   - Delete the JavaScript block that fakes the submit — once `action` is real, the script falls through and the browser submits normally.

Alternatives: **Buttondown** (indie, simple, $9/mo after free tier) or **ConvertKit / Kit** (creator standard, free tier). Avoid Substack — they own your list and defeat the purpose of owning `nikosamofa.com`.

---

## Email forwarding

Both versions use `hello@nikosamofa.com` as a contact. If you haven't set that up:

- If your domain is on Cloudflare → Cloudflare Email Routing (free, 5 minutes): forwards `@nikosamofa.com` mail to your real Gmail.
- If on Google Workspace already → just add the alias.
- If neither → ImprovMX free tier handles it.

---

## Things to update first (before going live)

1. **Email address** — change `hello@nikosamofa.com` to whatever you set up.
2. **GitHub URL** — confirm `github.com/nikosamofa` is correct. If different, search-replace.
3. **YouTube URL** — confirm `youtube.com/@nikosamofa` is correct.
4. **Newsletter form `action`** — see above.
5. **WGU degree title** — the credentials card says "M.S. — IT & Cybersecurity." Adjust to your actual program name if different.
6. **Credly link** — `https://www.credly.com/users/nikos-amofa` is already wired into the "Verify on Credly" link. Confirm it's correct.
7. **Open Graph image** (optional improvement) — your portrait is currently the `og:image`. It works, but a dedicated 1200×630 social card would look better in link previews. When you make one, save it as `assets/og-card.png` and swap the two `og:image` and `twitter:image` URLs in the `<head>`.

---

## File map

```
nikosamofa-site/
├── index.html                  ← clean production version (251KB, images inlined)
├── index-immersive.html        ← same content + three.js Operator Field (261KB)
├── assets/                     ← originals (referenced by og:image; images are also inlined in the HTMLs)
│   ├── portrait.jpg
│   ├── cert-isc2-cc.png
│   ├── cert-security-plus.png
│   ├── cert-cysa-plus.png
│   └── cert-pentest-plus.png
└── README.md                   ← this file (don't deploy)
```

Both HTML files are self-contained (images inlined as base64 data URIs) — the `assets/` folder is for the `og:image` and as reference originals.

---

## Strategic notes (why the site is built this way)

1. **The hero is a personnel file.** The Status / Domain / Day Role / Building / In Progress / Location data grid qualifies any visitor — recruiter, law firm partner, journalist — in about 5 seconds. That's the job of the hero.
2. **The newsletter is the primary CTA**, not Watusoft. Watusoft has its own site with its own conversion path. The personal brand's job is to grow the *owned* audience (email list), which compounds into Watusoft clients, job offers, speaking, and deal flow.
3. **§ section signs** as the structural device because your professional world (ABA Rule 1.6, NIST CSF, NIST 800-53) is a world of numbered legal/technical sections. The site mimics that vocabulary instead of using generic 01/02/03 markers.
4. **Three typefaces, three jobs.** Space Grotesk for display, Inter for body, JetBrains Mono for metadata, Instrument Serif italic for one tagline moment. Restraint reinforces the "Quiet Operator" positioning.
5. **The immersive version doesn't shout.** No Matrix rain. No wireframe globes. Just a subtle 3D dataspace that rewards attention without demanding it. Cyber-adjacent, brand-consistent.

---

## When you want to add things later

- **A blog/writing index** → add a new section `§ 08 — Writing` and link out to your Beehiiv archive URL (or embed past issues directly).
- **Press / speaking** → new section after Projects, same visual pattern as Credentials.
- **Resume / CV** → link a PDF in the Connect section, or in the hero as a tertiary action.
- **Testimonials** (once you have Watusoft clients) → a quiet quote section between Watusoft and Newsletter. Short, attributed, no logos farm.

That's it. Two versions, one folder, hosted free, owned by you, designed to compound.
