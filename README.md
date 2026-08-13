# InvoiceBoss website

A static site: landing page, privacy policy, terms of use and support page.
No build step, no dependencies, no JavaScript beyond setting the copyright
year. Open `index.html` in a browser and it works.

It deliberately loads **nothing from anyone else** — no font CDN, no
analytics, no embeds. The app promises it collects nothing; a site that
quietly reports every visitor to a third party would undercut that.

---

## Before you publish — three things to replace

Search the folder for `REPLACE-WITH-` and fix every hit. Apple checks these
links during review, and a page that 404s or shows a placeholder is a
rejection.

| Placeholder | Where | What to put |
|---|---|---|
| `REPLACE-WITH-YOUR-EMAIL@example.com` | privacy, terms, support | An address you actually read. Apple emails support addresses. |
| `REPLACE-WITH-YOUR-COUNTRY` | terms.html §10 | The country whose law governs the terms — normally where you or your company are based. |

```bash
grep -rn "REPLACE-WITH-" .
```

## Publishing on GitHub Pages

1. Push this repository to GitHub.
2. **Settings → Pages**.
3. Source: **Deploy from a branch**. Branch: `main`, folder: **`/docs`**.
4. Save. The site appears in a minute or two at:

```
https://<your-username>.github.io/<your-repo>/
```

So the two URLs the app needs will be:

```
https://<your-username>.github.io/<your-repo>/privacy.html
https://<your-username>.github.io/<your-repo>/terms.html
```

Open both in a browser before you submit. If they do not load for you, they
will not load for a reviewer.

## Point the app at them

Put the same two URLs into `lib/config/legal.dart`:

```dart
static const privacyPolicy = 'https://…/privacy.html';
static const termsOfUse    = 'https://…/terms.html';
```

`Legal.isConfigured` returns true once neither still points at the
`invoiceboss.app` placeholder domain.

## Where else the URLs are needed

- **App Store Connect** — Privacy Policy URL, and Support URL (`support.html`).
  The subscription screen in the app links to both, which Guideline 3.1.2
  requires.
- **Google Play Console** — Privacy Policy URL is mandatory for the listing.

## Using your own domain later

Add a file called `CNAME` in this folder containing just your domain
(`invoiceboss.app`), point the domain's DNS at GitHub Pages, then update
`legal.dart` and both store listings.

---

## A note on the legal text

These pages were written to describe **what this app actually does** — data
stays on the device, no analytics, purchases handled by the stores, camera
used only for the transfer code. That accuracy is the part that matters: a
policy copied from a template usually describes a server and an account that
do not exist, which is both wrong and worse than useless.

They are not legal advice. If you trade in the EU or UK, or you later add
analytics, crash reporting or any account system, have them reviewed and
update them — the privacy policy says outright that it will be updated before
any such version ships, so keep that promise.
