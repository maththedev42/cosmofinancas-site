# 01 — CosmoFinanças site: the app is live, the page still says "Em breve"

cosmofinancas.org (this repo, GitHub Pages via CNAME, static `index.html` 185 lines +
`style.css`) still shows `<span class="button button-disabled">Em breve na App Store</span>`
(index.html:84) and has NO store link anywhere. The app has been on the App Store since
2026-03-06; version 2.1.0 shipped 2026-10-09:
https://apps.apple.com/br/app/cosmo-finan%C3%A7as/id6758966021 (iPhone, iPad, Mac; iOS 17+).
Every visitor today hits a dead end. Also the hero sells "clareza", which every finance app
claims; the differentiator we can defend is **no bank connection**: the user imports the
statement the bank already gives them, we never hold bank credentials, no Open Finance consent
to renew. That is also the honest reason for the price (R$ 9,90/mês · R$ 79,90/ano).

---

```
PROJECT: cosmofinancas-site — static landing for CosmoFinanças (pt-BR copy; code/commits English).
Deploys to cosmofinancas.org on push to main (GitHub Pages), so NO push in this prompt.
ANCHOR CHECK: you are ALREADY at the site repo root. Run `ls CNAME index.html style.css`. If any is
missing, STOP and report "wrong anchor". Never cd to an absolute path.

RULES:
- Hand-written HTML/CSS, no framework, no build step, no new JS unless a CTA needs it (it doesn't).
- Keep every existing anchor id (#recursos, #como-funciona, #planos, #duvidas) — they're linked.
- Keep the JSON-LD FAQ (index.html ~58) valid; run `python3 -I -c "import json,re,sys;..."` or any
  JSON check on the extracted block after editing.
- pt-BR copy, short sentences, no exclamation marks, no "revolucionário"/"inteligente"/"IA".
- Commit path-limited after each green step, never --amend, NO push.
- Gate for every step: `python3 -I -m http.server 8123 &` then `curl -s localhost:8123/ | rg -c "apps.apple.com"` ≥ 2, and an HTML check with `tidy -q -e index.html` if tidy is installed (else skip and say so). Kill the server after.

STEP 0 — `git log --oneline -8`, `git branch -a`. Branches `feat/landing`, `eula-links`,
`contato-delete-pages` exist; run `git log --oneline main..<branch>` for each and report whether any
unmerged commit touches index.html. Do not merge them; just report. Read index.html fully.

STEP 1 — "feat(site): App Store download CTA replaces 'Em breve'"
Replace the disabled span at index.html:84 with a real link to the store URL above, pt-BR label
"Baixar na App Store", `rel="noopener"`, `target="_blank"`. Add the official Apple "Download on the
App Store" badge SVG (pt-BR variant, black) under `public/` — fetch it from Apple's marketing
badge page; if download is not possible offline, use a styled text button and say so. Add a second
smaller line "Também para Mac" linking the same URL. Add the same CTA at the end of #planos (both
cards) and in the footer. Add the `apple-itunes-app` smart banner meta:
`<meta name="apple-itunes-app" content="app-id=6758966021">`.

STEP 2 — "feat(site): positioning — no bank connection"
Hero: keep the h1 "Saiba para onde vai o seu dinheiro." (it is the <title> too) but change the
eyebrow/subheadline to say the method, e.g. "Importe a fatura e o extrato que o banco já te dá.
Sem conectar sua conta, sem senha do banco." Add a short reassurance row under the hero CTA with
three items: "Sem conexão bancária" · "Sem anúncios" · "Dados na sua conta Apple/Google".
In #recursos, rename card 1 to lead with the method ("Importe a fatura, não a senha") and keep the
CSV/Excel/PDF + duplicates + parcelas claims (all true, see app). In #duvidas, expand "Quais bancos
são suportados?" to name the exported formats and say explicitly that we do not use Open Finance
and never see bank credentials. Do not invent bank names.

STEP 3 — "feat(site): real screenshots and social preview"
Replace the CSS mockup "Visão do mês" with the real App Store screenshots: fetch the 6.7" PNGs
from `https://itunes.apple.com/lookup?bundleId=apps.mjkweber.MinhasFinancas&country=br`
(`screenshotUrls`, swap the `320x480bb.jpg` suffix for `1290x2796bb.png`), save 2–3 under
`public/screens/`, compress (`sips -Z 1400` is fine), add real alt text in pt-BR. Add Open Graph +
Twitter meta (title, description, `og:image` = a 1200×630 composed from one screenshot on the
brand background — produce it with Python/Pillow, no external service) so a shared link shows
the product. Update `sitemap.xml` lastmod.

STEP 4 — "chore(site): version, year and dead text"
Grep for "Em breve", "TestFlight", "beta", "2025" and fix what is stale. Footer keeps
"Feito para iPhone e Mac." but add "Disponível na App Store".

FINAL REPORT: commits, the exact CTA count (`rg -c apps.apple.com index.html`), the branch report
from STEP 0, before/after text of the hero, and the list of files added under public/.
```
