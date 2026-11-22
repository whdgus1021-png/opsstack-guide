# OpsStack Guide

Static review site for small agencies: automation engines, CRMs and the reporting stack around them.
Published with GitHub Pages at `https://whdgus1021-png.github.io/opsstack-guide/`.

## Structure

```
index.html                      home + methodology
make-vs-zapier-vs-n8n.html      flagship comparison
zapier-alternatives-task-limits.html
n8n-self-host-vs-cloud.html
hubspot-starter-vs-free-crm.html
client-reporting-automation.html
about.html                      editorial policy
affiliate-disclosure.html       FTC 16 CFR 255 disclosure
privacy.html
404.html
assets/client-report-template.csv
styles.css                      single stylesheet, no build step
sitemap.xml · robots.txt · .nojekyll
```

No build step, no JavaScript, no cookies, no analytics. Edit the HTML and push.

## Swapping in affiliate codes

Every outbound vendor link that can become an affiliate link carries `data-aff="<key>"`:

| key | vendor | where the affiliate parameter goes |
| --- | --- | --- |
| `make` | Make | `https://www.make.com/en/register?pc=<code>` |
| `zapier` | Zapier | `https://zapier.com/sign-up?<param>` |
| `n8n` | n8n | `https://n8n.io/?<param>` or the partner link n8n issues |
| `systeme` | systeme.io | `https://systeme.io/?<param>` |
| `hubspot` | HubSpot | `https://www.hubspot.com/products/crm?<param>` |

To apply a code to the whole site, replace the `href` of every link with a given `data-aff` value:

```sh
# example: point every Make CTA at a referral code
grep -rl 'data-aff="make"' *.html | xargs sed -i \
  's|href="https://www.make.com/en/register[^"]*"|href="https://www.make.com/en/register?pc=YOURCODE"|g'
```

Rules that must hold after a swap:
- keep `rel="sponsored nofollow noopener"` on affiliate links;
- keep the disclosure block at the top of each article (`affiliate-disclosure.html` is linked from it);
- never change the order of a comparison to favour a paying vendor.

## Adding an article (weekly loop)

1. Pick a keyword with buying intent: `X alternatives`, `X vs Y`, `X pricing`, `how much does X cost`.
2. Check the vendor pricing page **the same day** and note the date. Record both annual and monthly prices when both exist, and label promotions.
3. Use the existing article head, `.layout` + `.toc` block, tables (`.table-scroll`), `.callout`, `.verdict` patterns from `n8n-self-host-vs-cloud.html`.
4. Requirements before publishing:
   - one table of real plan limits,
   - one worked cost calculation (per run / per seat / per client),
   - a "who should not buy this" section,
   - `<title>` under 60 characters, meta description under 160,
   - `Article` + `FAQPage` JSON-LD in `<head>`,
   - dated price note at the end of the article.
5. Add the URL to `sitemap.xml` and to the card grid on `index.html`.
6. Push to `main`; Pages redeploys in about a minute.

## Price check log

Keep a line per check in `price-check-log.md` (create on first use): date, vendor, plan, price, source URL.
The weekly loop re-checks one vendor per run rather than all of them at once.
