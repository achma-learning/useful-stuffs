# useful-stuffs — AI Context File
_Last synced: 2026-08-30_

## 1. What This Is (Plain English)

- **In one sentence:** A single web page that explains, step by step, how to get the Moroccan documents, money moves, and life-event procedures a young person (16–30) actually needs — national ID card, a youth discount app, passport, bank account, savings/investment, self-employment status, driving licence, car insurance and accident reports, health insurance, population/social registration, marriage, criminal record, car registration, utility contracts, complaint portal, voter registration, military service, scholarships, diploma legalization — with tickable checklists and links to the official government sites.
- **Why it exists:** Moroccan admin info is scattered across a dozen government portals, half of which are slow or block you, and each one tells you a different version of the required documents. This collects the real requirements in one place, sourced from the official pages, so you don't have to re-research it every time or show up at the counter missing a paper.
- **Who uses it:** Just the owner, for now. It's a personal pense-bête (memory aid). No accounts, no backend, nothing private in it — but it's public on GitHub, so don't put personal document numbers in it.
- **Vibe:** Polished personal tool. It's one hand-written HTML file with no build step, but the content is carefully sourced and it's been checked in a real browser.

## 2. How To Run It

- **Setup once:** Nothing. No install, no dependencies, no env file.
- **Run dev:** Open `index.html` in a browser. That's it.
  - If you need the PDFs to display in the embedded viewers, serve it over HTTP instead of `file://`:
    ```
    python3 -m http.server 8899
    # then open http://localhost:8899/index.html
    ```
- **Build / deploy:** No build. It's static — push to `main` and it's deployable as-is to GitHub Pages or any static host.
- **Required env vars:** None. There is no `.env.example` and the page makes no API calls.

## 3. Tech Stack

- **Language + runtime:** Plain HTML5, CSS, and vanilla JavaScript (ES5-style, runs in any browser). No runtime, no package manager — there is no `package.json`, no lockfile, no `.nvmrc`.
- **Framework / key libraries:** None. Zero dependencies, on purpose.
- **What kind of project:** A single-file static web page plus a folder of PDF assets.
- **External services:** None at runtime. The page only *links out* to Moroccan government, financial and EU youth portals (cnie.ma, passeport.ma, idarati.ma, wraqi.ma, consulat.ma, watiqa.ma, alhalalmadania.ma, sgg.gov.ma, maroc.ma, cnss.ma, cnops.org.ma, listeselectorales.ma, tajnid.ma, recrutement.far.ma, casablanca-bourse.com, ammc.ma, bkam.ma, albaridbank.ma, groupebcp.com, tamwilcom.ma, passjeunes.ma, ae.gov.ma, rn.ae.gov.ma, rnp.ma, rsu.ma, chikaya.ma, lydec.co.ma, redal.ma, amendis.ma, onee.ma, eadoul.justice.gov.ma, siaa.justice.gov.ma, minhaty.ma, diplomatie.ma, acaps.ma, daamsakane.ma, e-blagh.ma, tax.gov.ma, vignette.ma, epolice.ma, youth.europa.eu, and others). Nothing is fetched or posted.

## 4. Code Map (The Important Files Only)

- `index.html` — **the whole app.** ~1920 lines: a `<style>` block (~263 lines), the content markup (24 sections), and a `<script>` block (~42 lines) at the bottom. This is the only file you ever need to edit for content.
- `assets/normes-photos-cin.pdf` — official ID-photo standards chart for the national ID card, embedded in an `<iframe>` in the CIN section.
- `assets/normes-photos-passeport.pdf` — same, for the passport.
- `assets/alhalalmadania/*.pdf` — 22 civil-status circulars and laws (Arabic), archived from alhalalmadania.ma's Publications page as a backup in case that site goes down. Linked from the Portails section. **Filenames are Arabic and contain spaces** — hrefs in the HTML are percent-encoded; if you rename a file you must re-encode its link.
- `assets/visa-requirements-moroccan-citizens.svg` — world map of visa requirements for Moroccan passport holders, shown in a `<figure>` in the passport section. **Third-party, CC BY-SA 4.0 by "Eaaoulad"** (Wikimedia Commons). The attribution in the figcaption is a licence condition — do not remove it.
- `assets/permis/` — driving-licence documents behind the download buttons: the Highway Code, the model training contract (PDF + .docx), and the ministry's instructor guides for categories A, B, C/E(C), D/E(D). The Sanlam insurance PDF in this folder is deliberately surfaced in the **Assurance** section, not Permis — it is a private insurer's sample, not an official document. Filenames contain spaces, parentheses and Arabic; hrefs are percent-encoded.
- `assets/coat-of-arms-morocco.svg`, `assets/flag-morocco.svg` — state emblems used as the favicon and in the footer. Both public domain (Wikimedia Commons), no attribution required.
- `assets/certifications-fr/` — TCF/DELF/DALF documents for the "Certifications de français" section: the Campus France/Institut français comparison leaflet, the official IFM 2026 tariff leaflet, the full bilingual 2026-2027 exam calendar, and the TCF score grid. **`depliant-ifm-2026-tarifs.pdf` was rebuilt at screen resolution** — the original download from if-maroc.org was 72MB (very high-res source images); this version is ~1.2MB and still fully legible. Don't replace it with the raw original.
- The 14-centre table in that same section (`#francais`) links each city to its `if-maroc.org` page and each address to a Google Maps URL. **All 14 map links are real `maps.app.goo.gl` short links the user supplied directly** (not constructed search URLs). **Oujda has two**, shown as "Site 1" / "Site 2" — the user flagged it as having two locations; no further distinction (e.g. which is the médiathèque) is known, don't invent one. If a centre moves, ask the user for the new Maps link rather than reconstructing one.
- `quotes.md` — unrelated personal notes. Not part of the site.
- `README.md` — the human-facing overview: what the page is, the list of 24 topics, how to run it locally, and a short contributing note. Keep it simple and short — it's for a person skimming GitHub, not an exhaustive doc. `CONTEXT.md` (this file) is the detailed one for AI assistants/contributors.

## 5. Rules For Editing This Code

- **Keep it zero-dependency and single-file.** No npm, no build step, no framework, no CDN scripts. If a change needs a bundler, it's the wrong change.
- **Every new piece of visible text needs a `data-ary` attribute** with its Moroccan Darija translation, or the language toggle will leave it in French when the user switches. See §6.
- **Only add facts you can point to a source for.** Every number on this page (fees, validity periods, delays, document lists) came from an official portal or the cited Wikipedia article. Don't fill gaps from memory — mark them unknown instead.
- **Every external link gets `target="_blank" rel="noopener"`.**
- **Checkbox `id`s must stay unique and stable.** They're the localStorage keys for saved progress (`checklist:<id>`). Renaming an id silently wipes that item's saved state.
- **Don't put personal data in here** — no document numbers, no addresses. The repo is public.

## 6. Fragile Bits & Landmines

- **The translation system is attribute-based and easy to half-break.** At load, the script snapshots every `[data-ary]` element's `innerHTML` into `dataset.fr`, then swaps `innerHTML` between the two on toggle. Consequences:
  - A `data-ary` value must contain the **full inner HTML** of its element, including any `<a>` or `<strong>` tags. If you translate only the text, the links vanish in Darija mode.
  - Never nest one `data-ary` element inside another — the outer swap will clobber the inner one's snapshot.
  - `innerHTML` is used deliberately here because translations contain markup. This is safe **only** because every value is author-written and static. Never feed user input into a `data-ary`.
- **RTL is applied by setting `document.body.dir`**, with a few `[dir="rtl"]` CSS overrides for list padding and table alignment. If you add a new layout that relies on `padding-left` or `text-align: left`, check it in Darija mode or it will look wrong.
- **Arabic PDF filenames.** `assets/alhalalmadania/` filenames are Arabic with spaces. The links are percent-encoded in the HTML. Don't "tidy up" these filenames without regenerating the links — and note one file has a double space in its name that is load-bearing for the current link.
- **`assets/alhalalmadania/q`** and **`assets/permis/delete`** are stray 1-byte files (just a newline), created as placeholders so the folders could be made through GitHub's web UI. Harmless and unreferenced — don't be confused by them.
- **Two different photo-background rules coexist, and that's correct.** The passport *pieces-à-fournir* text says the background may be blue, white, or light gray; the separate official photo-norms chart only shows blue/gray. Both are quoted from their own official source. Don't "fix" the inconsistency by picking one.
- **Some source sites block automated fetching** (idarati.ma, alhalalmadania.ma, and cnie.ma's FAQ returned 503/403 to tooling). Content from those was taken from user-pasted text or search results. If you're asked to re-verify a fact and the fetch fails, say so rather than guessing.

## 7. Current State

- **Section order (24 total):** `cin` → `passjeunes` → `passeport` → `banque` → `epargne` → `autoentrepreneur` → `permis` → `assurance` → `constat` → `sante` → `rnp` → `mariage` → `casier` → `cartegrise` → `utilities` → `impots` → `chikaya` → `electoral` → `tajnid` → `bourses` → `legalisation` → `francais` → `esc` → `portails`. This is the merged result of two branches developed in parallel and merged back together on 2026-08-30 — `index.html` merged with **no conflicts** (the two branches touched disjoint sections); only this file's changelog needed manual reconciliation.
- **What's in each newer section, briefly:**
  - `#constat` — accident report (constat amiable): recto filled jointly on-site, verso alone after, 5-business-day deadline to notify the insurer.
  - `#mariage` — the Adoul marriage procedure: documents per spouse, family-court -> Adoul -> Livret de Famille flow, CNIE update with the epoux/epouse mention. Adoul fees deliberately left as "variable, verifier au tribunal" rather than a number - the only figure found (200-500 EUR) came from an MRE/consular context that likely bundles translation/apostille costs, not a domestic Adoul honorarium.
  - `#bourses` — the Minhaty university scholarship: eligibility (incl. the RNP+RSU prerequisite already documented in `#rnp`), the 2026/2027 campaign window, application via minhaty.ma.
  - `#legalisation` — diploma legalization for working/studying abroad: AREF for the bac vs. university/ministere for higher degrees, then Ministere des Affaires etrangeres, then the target country's embassy; typical delays by diploma level.
  - `#impots` — the two annual local taxes: the vignette (TSAV — cars by puissance fiscale, motorcycles by cylindree, deadline 31 Jan / 60 days for new vehicles, payable free via bank channels or vignette.ma/Daribati) and the taxe d'habitation + taxe de services communaux (owner/usufruitier; declaration deadline 31 Jan is a *separate* date from the payment deadline of 31 May, pushed to 1 June 2026; managed by DGI since June 2025 per loi 14-25). This was a fact-check-and-correct job — user-pasted AI-generated tax info wrongly conflated the TH/TSC declaration and payment deadlines, and wrongly described motorcycle vignette as computed like cars; both corrected after independent verification (maroc.ma, lebrief.ma, fnh.ma, ovoiture.ma).
  - `#esc` — the EU's European Solidarity Corps ("Youth Europa"): fully-funded volunteering for 17-30 year-olds, Morocco eligible as an EU-neighbouring third country, registration via EU Login on youth.europa.eu/solidarity, CSM Maroc named as a local Quality-Label-holding partner org.
  - `#casier` gained an h3 sub-block for epolice.ma's certificat de bonne conduite (shahada husn sira) — an 8-step checklist plus a tip distinguishing it from the bulletin n°3 casier judiciaire already covered there (different issuer: DGSN/police vs. ministere de la Justice; epolice.ma launched Dec 2024).
  - `#portails` gained Daam Sakane (daamsakane.ma, state housing-purchase aid, up to 100,000 DH) and E-Blagh (e-blagh.ma, DGSN's illicit-content reporting portal) — both sourced from press coverage since the sites themselves return 503 to fetch tools, same as idarati.ma/alhalalmadania.ma.
  - Earlier same-week batches added `#epargne`, `#sante`, `#electoral`, `#tajnid`, `#passjeunes` (promo-code mechanism), `#autoentrepreneur`, `#rnp` (restructured into a two-step RNP -> RSU flow), `#utilities`, `#chikaya`.
- **Working on now:** Nothing in flight. All sections were checked with Python's `html.parser` (no structural errors) plus a section/id-balance script (24 sections, all nav-pill anchors match section ids 1:1, no duplicate checklist ids across 74 checkboxes). The Darija translations added across all these batches have not been proofread by a native speaker.
- **Next up:**
  1. Have a Darija speaker proofread the `data-ary` translations — written by the AI assistant mirroring the file's existing vocabulary, not verified by a native speaker.
  2. Verify under-18 eligibility details for the youth savings accounts in `#epargne` (legal-guardian angle) — not yet confirmed against an official source.
  3. `#mariage`'s Adoul-fee fact is deliberately vague ("variable, vérifier au tribunal") because sourced figures conflicted (a consulate/MRE-context page quoted €200–500, which likely bundles translation/apostille costs, not a domestic Adoul honorarium) — replace with a real figure only if a clean domestic source turns up.
  4. Manually re-check the Daam Sakane / E-Blagh facts in `#portails` against the live sites once they stop returning 503 to fetch tools.
  5. Possibly archive backup copies of the passeport.ma legal-text PDFs the way the alhalalmadania ones were archived.

## 8. Update Protocol (Verbatim)

> **For the AI Assistant:** When asked to "Update CONTEXT.md":
> 1. Re-run Phase 0 — check for new `GEMINI.md` / `CLAUDE.md` / `.github/` files.
> 2. Re-scan the tree, manifests, and `.github/workflows/` for drift.
> 3. Read our recent conversation for new decisions, fragile bits discovered, or shifted goals.
> 4. Refresh the `_Last synced_` line with today's date and current commit SHA.
> 5. Rewrite — do not append. One clean source of truth. Preserve still-true content, revise the rest.
> 6. Keep §1 and §2 in plain English. Keep the file under ~350 lines.
> 7. **Also update `README.md`** whenever a section is added/removed/renamed in `index.html`, or the run/setup instructions change. Keep `README.md` short and human-friendly (topic list, how to run it, how to contribute) — the exhaustive detail belongs here in `CONTEXT.md`, not there.
