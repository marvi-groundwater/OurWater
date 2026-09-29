# OurWater — landing site

The public front door for OurWater: a single static page, published on
GitHub Pages, with its content managed through a git-based CMS.

**Live site:** https://ourwater.org.au/
**Content admin (CMS):** https://ourwater.org.au/admin/

**Created by Alan Ng ([@alanntl](https://github.com/alanntl)).** It was first
published as [alanntl/OurWater](https://github.com/alanntl/OurWater) and moved
here with its full history. That repo is now archived, and its old site
address forwards here.

## How it fits together

```
content/landing.json   ← everything an editor can change (the CMS edits this)
template.html          ← the page design (layout, styles, animations)
scripts/build.mjs      ← content + template → _site/index.html
scripts/verify.mjs     ← refuses to publish a broken build
admin/                 ← Sveltia CMS (vendored), edits content/ via GitHub
get/, s/               ← the QR landing pages (install, and open a station)
.github/workflows/     ← every push to main rebuilds and republishes Pages
```

Editing flow: open `/admin/`, sign in, change the text, save. The save is a
commit to `main`; the deploy workflow rebuilds the site from it. Nothing to
install, no server anywhere.

## Signing in to the CMS

Sveltia is configured with `auth_methods: [token]`: editors sign in with a
GitHub **fine-grained personal access token** — create one at
github.com/settings/personal-access-tokens with access to only this
repository and the **Contents: read and write** permission. Tokens expire
(GitHub caps them at about a year); when saving stops working, issue a new
one and sign in again.

## Local development

```
node scripts/build.mjs && node scripts/verify.mjs
```

then serve `_site/` (for example `python3 -m http.server --directory _site`).
No dependencies to install.

## Domain

`ourwater.org.au` is registered at WebCentral and uses WebCentral's own DNS
("Client Area DNS" on `ns1–3.netregistry.net`). It has two records:

- `A` on `ourwater.org.au`, pointing at GitHub Pages: `185.199.108.153`,
  `.109.153`, `.110.153` and `.111.153`.
- `CNAME` `www` → `marvi-groundwater.github.io.`

The custom domain is also set in this repo's Pages settings. GitHub serves
the HTTPS certificate and redirects `www` and the old github.io address to
`ourwater.org.au`.

To edit the records, go to the WebCentral console: **Products & Services →
DNS → DNS Records**. That table sits in an embedded frame whose buttons may
not respond in Chrome. If so, open the frame on its own page
(`domainservices.webcentral.au`); it works there.

## Where the buttons go

OurWater has no app or store listing of its own yet, only this site's
domain: today it runs as a mode of the MyWell app. So the page's working links still point there —
Sign in and Create account (`https://app.mywell.au`), the Google Play and
App Store listings, the Android package and App Store id behind the
open-in-app bar and the iOS banner, the `mywell://` link that bar opens on an
iPhone, and the tutorial videos, which are hosted with the app. Everything a
visitor reads says OurWater.

When OurWater gets endpoints of its own:

- **App links** and **App store links** in the CMS (`content/landing.json`)
  move the page's buttons, banner and smart bar.
- The tutorial `src`/`poster` URLs are in `content/landing.json` under
  `tutorials` (not exposed in the CMS form).
- `get/index.html` and `s/index.html` carry their own copies of the app and
  store URLs; `template.html` holds the `mywell://` scheme.

## Where it came from

A rebranded copy of the mywell.au landing site
([marvi-groundwater/mywell](https://github.com/marvi-groundwater/mywell) at
`2910c17`): the same design, CMS and deploy, with MyWell renamed to OurWater
throughout.

The logo keeps the MyWell design (the umbrella catching rain) with the name
changed, redrawn by an image model from the two MyWell logos.
`assets/logo/ourwater-logo.png` is the full logo with its tagline.
`ourwater-mark.png` is the umbrella alone, cut from the full logo, used as the
favicon and phone app-bar icon.

The header and footer logo, `ourwater-lockup.png`, follows the OurWater
app-icon badge instead, without its ring. It shows the umbrella, then "Our"
in black and "Water" in blue, with "for Water Sustainability" beneath. It
appears 46px tall rather than MyWell's 38px so the tagline stays readable, and
it shrinks on phones to keep Sign in and Get started on its row.
