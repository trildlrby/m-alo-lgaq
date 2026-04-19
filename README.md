# Horizon

*A private five-year plan, lived one month at a time.*

Horizon is a personal long-term planning app. You write a vision of where you want to be in five years, break it into year-by-year milestones, then check in once a month to log what went well, what didn't, and whether you're trending toward the life you said you wanted.

Your data never leaves your device. Ever.

---

## Why this exists

Most goal apps are either sprint trackers dressed up as life tools — daily streaks, habit chains, productivity theater — or journaling apps that forget the goal part. Neither helps with the question Horizon is built around:

> *Over five years, am I actually becoming the person I said I wanted to be?*

That question only answers itself if you can see the whole arc. Horizon gives you that arc at a glance — your vision, your milestones per year, and the trajectory of your last six months — and makes it light enough to open once a month and actually use.

No streaks to protect. No notifications. No gamification. Just you, your future, and an honest record.

## Screenshots

<!-- Add screenshots here:
  - Home screen (light + dark)
  - Plan view with milestones
  - Check-in modal
  - Appearance picker
-->

## Features

- **Five-year vision.** Write your destination. Edit it anytime — people change.
- **Year-by-year milestones.** Concrete goals that bridge today and that future.
- **Monthly check-ins.** Log positives, negatives, next month's actions, and a reflection.
- **Trajectory indicator.** A rolling read of your last six check-ins — are you moving toward it, holding steady, or drifting off course?
- **Light, dark, or system theme.** Designed for both.
- **End-to-end encrypted.** AES-256-GCM with a password-derived key. No hash stored.
- **Installable as a PWA.** Add to iPhone or Android home screen. Opens fullscreen. Works offline.
- **Portable data.** Export and import as JSON. Bring your history to any device.
- **No servers, no tracking, no accounts.** There's no backend to run or trust.

## Privacy & security

The honest, technical version:

- All data lives in your browser's `localStorage`. Nothing is ever sent to a server.
- On first launch, you set a password. A random 16-byte salt is generated, and your password is stretched via **PBKDF2-SHA256** with **310,000 iterations** to derive a 256-bit AES key.
- Your plan data is encrypted with **AES-256-GCM** (authenticated encryption) before being stored.
- localStorage contains only three things: the salt, the IV, and the ciphertext. **No password hash is stored anywhere.**
- Wrong password = decryption fails with an authentication error. No offline guessing, no hash to crack.
- If you forget your password, your data is genuinely unrecoverable. Export backups.

The source code being public doesn't affect security — the protection lives entirely in your password. AES-256-GCM with a strong password-derived key is cryptographically infeasible to break.

## Tech stack

- **Single HTML file.** No build step, no bundler, no npm.
- [Preact](https://preactjs.com/) for components
- [htm](https://github.com/developit/htm) for JSX-like templates without a compiler
- Web Crypto API for PBKDF2 + AES-GCM
- CSS custom properties for theming
- [Fraunces](https://fonts.google.com/specimen/Fraunces) + [Inter Tight](https://fonts.google.com/specimen/Inter+Tight) for typography

That's the whole dependency list. Open the file in any modern browser and it runs.

## Deploy your own

You need a static host. GitHub Pages, Netlify, Vercel, Cloudflare Pages — anywhere that serves an HTML file.

### GitHub Pages (recommended)

1. Fork this repo (or create a new one and upload `index.html`).
2. Make it public (required for free Pages) or upgrade to Pro for private repos.
3. Go to **Settings → Pages**. Set source to **Deploy from a branch**, branch: **main**, folder: **/ (root)**.
4. Wait about a minute. Your app is live at `https://your-username.github.io/your-repo/`.

### On iPhone (install as a PWA)

1. Open your URL in **Safari** (must be Safari for PWA install).
2. Tap the Share button → **Add to Home Screen**.
3. Name it "Horizon" or whatever you like.
4. Tap the icon — opens fullscreen, no browser chrome.

When you create your password, iOS will offer to save it to Keychain. Accept — then Face ID autofill works on every subsequent unlock.

### On Android

Chrome → three-dot menu → **Install app** (or "Add to Home screen"). Same behavior.

## Using the app

**First run:**
- Set a password (6+ characters). Don't lose this.
- Tap the dark vision card to write your five-year aspiration.
- Go to **Plan** → tap `+` to add milestones for each year.
- Tap **Start your first check-in** when ready.

**Monthly ritual (about 5 minutes):**
- Unlock the app.
- Tap **New check-in**.
- Select trajectory (on track / mixed / off track).
- List the month's positives, negatives, and next month's actions.
- Add an optional reflection.
- Save.

**Over time:**
- The home screen shows your rolling trajectory across the last six check-ins.
- Tap **History** to browse every past check-in in full.
- Export a JSON backup monthly as a safety net.

## Data portability

- **Export:** Settings (gear) → Export my data → Copy or Download. Produces clean, readable JSON.
- **Import:** Settings → Import data → paste JSON → replaces current data.
- **Sync across devices:** export on one device, import on another.

### Schema

```json
{
  "vision": {
    "text": "...",
    "startYear": 2026,
    "targetYear": 2031
  },
  "milestones": [
    { "id": "...", "text": "...", "year": 2027, "done": false }
  ],
  "checkins": [
    {
      "id": "...",
      "createdAt": 1735200000000,
      "month": 3,
      "year": 2026,
      "trajectory": "on",
      "positives": ["..."],
      "negatives": ["..."],
      "nextActions": ["..."],
      "reflection": "..."
    }
  ],
  "settings": {
    "theme": "system"
  }
}
```

`trajectory` is one of `"on"`, `"neutral"`, `"off"`. `theme` is one of `"light"`, `"dark"`, `"system"`.

Older exports without a `settings` block are migrated automatically on import.

## Philosophy

A few design decisions that may seem like limitations but are intentional:

- **No cloud sync.** Syncing means servers means accounts means trust. Export/import is the escape hatch for multi-device.
- **No notifications or streaks.** Horizon is not trying to manufacture engagement. A tool you open once a month by choice is more valuable than one that pesters you daily.
- **No analytics.** Not even anonymous ones.
- **Monthly, not weekly.** Weekly is too noisy for a five-year arc. Daily is theater. Monthly is the right cadence for a question this big.
- **One file.** Auditable in an afternoon. Forkable. Future-proof.

## Roadmap

Things being considered:

- Year-end retrospective view that summarizes a full year of check-ins
- Exportable PDF annual report
- Optional quarterly check-in mode alongside monthly
- File-picker based iCloud Drive / Drive sync (still local-first, just user-initiated)

Things deliberately not planned:

- User accounts, cloud sync servers — this is a local-first tool by design
- Social features, sharing, or collaboration — this is a private tool by design
- AI "insights" on your data — your data is yours

## Contributing

Bug reports and thoughtful feature ideas are welcome as issues. PRs welcome too — but please respect the spirit: single file, no build step, no new dependencies, no servers, no tracking.

## Acknowledgments

- Typography: Fraunces by Undercase Type, Inter Tight by Rasmus Andersson
- Libraries: Preact, htm, Lucide (icon paths)
- Initial scaffolding built collaboratively with Claude (Anthropic)

## License

MIT. Do what you want with it. If you ship something meaningful built on this, I'd love to hear about it.

---

<p align="center"><i>The best time to plant a tree was twenty years ago. The second best time is now.</i></p>
