# The Village Chiropractic & Wellness

## Local preview and verification

This is a single-page static site. Edit `index.html` for markup, inline CSS,
JavaScript, and embedded images. The site has no build or install step.

From the repository root, start a local server and leave it running while
checking the site:

```sh
python3 -m http.server 8000 --bind 127.0.0.1
```

Open http://127.0.0.1:8000/ in a browser. For visual or interactive changes, check
desktop and mobile widths and exercise the affected navigation, booking menus,
or dialogs. Confirm booking links point to the intended patient flow.

From another terminal, check that all four public URLs return HTTP 200:

```sh
for path in / /og-image.jpg /robots.txt /sitemap.xml; do
  curl --fail --silent --show-error --output /dev/null \
    --write-out '%{http_code} %{content_type} %{url_effective}\n' \
    "http://127.0.0.1:8000$path" || exit 1
done
```

Run `git diff --check` before committing. For staged changes, also run
`git diff --cached --check`.

## Repeated content

- The three SKED booking destinations (`portal.sked.life`) appear in the
  desktop scheduling dropdown, mobile menu, hero booking buttons, and Patients
  section (`#new-patients`). Update the relevant destination in all four places,
  checking that each link still matches its patient category.
- Clinic details appear in the hero, Contact section, footer, legal dialogs, and
  the LocalBusiness/MedicalBusiness JSON-LD in `index.html`. Check the visible
  address and Google Maps links, phone and fax, `tel:`, `sms:`, and `mailto:`
  links, and the Contact hours against `openingHoursSpecification`. Also search
  the doctor and insurance sections for phone and email references. Preserve the
  distinct care-team and administrative email addresses.
- For domain or social-image changes, check the canonical and social metadata
  and JSON-LD in `index.html`, the root `og-image.jpg`, and the production URLs
  in `robots.txt` and `sitemap.xml`.

## Hosting

- `wrangler.jsonc` tells Cloudflare Workers to serve static assets from the
  repository root. `.assetsignore` excludes repository files from that asset
  upload. Keep `AGENTS.md` excluded and add other non-public files there when
  introducing them.
- `netlify.toml` defines Netlify's response headers, including its Content
  Security Policy (CSP). `index.html` also has a CSP meta tag. Review both when
  changing external resource origins. Python's preview server does not apply
  hosting-specific asset exclusions or response headers.
