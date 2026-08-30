# useful-stuffs — AI Context File
_Last synced: 2026-08-30_

## 1. What This Is (Plain English)

- **In one sentence:** A single web page that explains, step by step, how to get the Moroccan documents, money moves, and civic obligations a young person (16–30) actually needs — national ID card, a youth discount app, passport, bank account, savings/investment, self-employment status, driving licence, car and health insurance, a national population registration, criminal record, car registration, utility contracts, complaint portal, voter registration, military service — with tickable checklists and links to the official government sites.
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
- **External services:** None at runtime. The page only *links out* to Moroccan government and financial portals (cnie.ma, passeport.ma, idarati.ma, wraqi.ma, consulat.ma, watiqa.ma, alhalalmadania.ma, sgg.gov.ma, maroc.ma, cnss.ma, cnops.org.ma, listeselectorales.ma, tajnid.ma, recrutement.far.ma, casablanca-bourse.com, ammc.ma, bkam.ma, albaridbank.ma, groupebcp.com, tamwilcom.ma, passjeunes.ma, ae.gov.ma, rn.ae.gov.ma, rnp.ma, rsu.ma, chikaya.ma, lydec.co.ma, redal.ma, amendis.ma, onee.ma, daamsakane.ma, e-blagh.ma, tax.gov.ma, vignette.ma, and others). Nothing is fetched or posted.

## 4. Code Map (The Important Files Only)

- `index.html` — **the whole app.** ~1715 lines: a `<style>` block (~263 lines), the content markup (19 sections), and a `<script>` block (~42 lines) at the bottom. This is the only file you ever need to edit for content.
- `assets/normes-photos-cin.pdf` — official ID-photo standards chart for the national ID card, embedded in an `<iframe>` in the CIN section.
- `assets/normes-photos-passeport.pdf` — same, for the passport.
- `assets/alhalalmadania/*.pdf` — 22 civil-status circulars and laws (Arabic), archived from alhalalmadania.ma's Publications page as a backup in case that site goes down. Linked from the Portails section. **Filenames are Arabic and contain spaces** — hrefs in the HTML are percent-encoded; if you rename a file you must re-encode its link.
- `assets/visa-requirements-moroccan-citizens.svg` — world map of visa requirements for Moroccan passport holders, shown in a `<figure>` in the passport section. **Third-party, CC BY-SA 4.0 by "Eaaoulad"** (Wikimedia Commons). The attribution in the figcaption is a licence condition — do not remove it.
- `assets/permis/` — driving-licence documents behind the download buttons: the Highway Code, the model training contract (PDF + .docx), and the ministry's instructor guides for categories A, B, C/E(C), D/E(D). The Sanlam insurance PDF in this folder is deliberately surfaced in the **Assurance** section, not Permis — it is a private insurer's sample, not an official document. Filenames contain spaces, parentheses and Arabic; hrefs are percent-encoded.
- `assets/coat-of-arms-morocco.svg`, `assets/flag-morocco.svg` — state emblems used as the favicon and in the footer. Both public domain (Wikimedia Commons), no attribution required.
- `assets/certifications-fr/` — TCF/DELF/DALF documents for the "Certifications de français" section: the Campus France/Institut français comparison leaflet, the official IFM 2026 tariff leaflet, the full bilingual 2026-2027 exam calendar, and the TCF score grid. **`depliant-ifm-2026-tarifs.pdf` was rebuilt at screen resolution** — the original download from if-maroc.org was 72MB (very high-res source images); this version is ~1.2MB and still fully legible. Don't replace it with the raw original.
- The 14-centre table in that same section (`#francais`) links each city to its `if-maroc.org` page and each address to a Google Maps URL. **All 14 map links are real `maps.app.goo.gl` short links the user supplied directly** (not constructed search URLs). **Oujda has two**, shown as "Site 1" / "Site 2" — the user flagged it as having two locations; no further distinction (e.g. which is the médiathèque) is known, don't invent one. If a centre moves, ask the user for the new Maps link rather than reconstructing one.
- `quotes.md` — unrelated personal notes. Not part of the site.
- `README.md` — one line, effectively empty. `CONTEXT.md` (this file) is the real documentation.

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

- **Last shipped (2026-08-30, second change):** Added a new `#impots` section (between `#utilities` and `#chikaya`, own nav pill) covering the two annual local taxes: the vignette (Taxe Spéciale Annuelle sur les Véhicules — cars by puissance fiscale, motorcycles by cylindrée, deadline 31 Jan / 60 days for new vehicles, payable free via bank channels or vignette.ma/Daribati) and the taxe d'habitation + taxe de services communaux (owner/usufruitier, declaration deadline 31 Jan but a *separate* payment deadline of 31 May — pushed to 1 June 2026 — within 2 months of the avis d'imposition; managed by DGI since June 2025 per loi 14-25). This was a fact-check-and-correct job: the user pasted AI-generated tax info that got the vignette/DGI facts right but wrongly conflated the TH/TSC declaration deadline (31 Jan) with its payment deadline (31 May), and wrongly described motorcycle vignette as computed like cars (puissance fiscale) instead of by cylindrée. Both corrections verified via WebSearch across independent sources (maroc.ma, lebrief.ma, fnh.ma, ovoiture.ma) before writing. vignette.ma and simpl@tax.gov.ma / 05 37 27 37 27 (the DGI call center) were confirmed live/accurate. Includes a `tip` box calling out the declaration-vs-payment distinction explicitly so it doesn't get "simplified" back into one date later.
- **Last shipped (2026-08-30, first change):** Added two new h3 blocks to `#portails`, between the Wraqi and Consulat.ma blocks — Daam Sakane (daamsakane.ma, the state's direct housing-purchase aid program, ADESL 2024-2028, up to 100,000 DH) and E-Blagh (e-blagh.ma, the DGSN's portal for reporting illicit online content). Both sourced from press coverage (aujourdhui.ma, le360.ma, mapnews.ma etc.) since the sites themselves return 503 to fetch tools, same as idarati.ma/alhalalmadania.ma — worth a manual re-check against the live sites when they're reachable. Both have full Darija `data-ary` translations (unproofread, same caveat as the rest).
- **Last shipped (two batches, same day):** Nine new sections total, aimed at 16–30 year-olds, inserted between the existing ones (all sourced per §5, all with Darija `data-ary` translations). Current section order: `cin` → `passjeunes` → `passeport` → `banque` → `epargne` → `autoentrepreneur` → `permis` → `assurance` → `sante` → `rnp` → `casier` → `cartegrise` → `utilities` → `chikaya` → `electoral` → `tajnid` → `francais` → `portails`.
  - **Batch 1:** `#epargne` (savings/investment: youth/national savings accounts, compte-titres + PEA + OPCVM, Intelaka/Forsa, crypto framed as a legal-risk warning); `#sante` (AMO/CNSS/CNOPS, AMO Étudiants); `#electoral` (voter registration via listeselectorales.ma); `#tajnid` (military census via tajnid.ma under loi 44-18, plus recrutement.far.ma).
  - **Batch 2** (in response to a follow-up review): `#passjeunes` (the free 16–30 discount app, legal framework since décret 2.25.153 of July 2026); `#autoentrepreneur` (self-employment status — 18+, 0.5%/1% libératoire tax, 500k/200k DH turnover ceilings, mandatory CNSS at 10.5% with a 1,200 DH/year minimum, registration on rn.ae.gov.ma); `#rnp` (Registre National de la Population / IDCS, pre-registration on rnp.ma + finalization in person, increasingly required for RSU social programs); `#utilities` (transferring water/electricity contracts when renting — which operator per city: Lydec, Redal, Amendis, RADEEMA, ONEE elsewhere — tied back to the CIN section's *attestation de résidence* requirement); `#chikaya` (promoted from a passing mention in Portails to its own section — 60-day legal response deadline, 3737 hotline).
  - Nav pills, and CONTEXT.md's own code map/external-services list, were updated to match both batches.
- **Working on now:** Nothing in flight. All new sections were checked with Python's `html.parser` (no structural errors) plus a section/id-balance script (18 sections, all nav-pill anchors match section ids, no duplicate checklist ids); batch 1 was also spot-checked with a headless-Chrome screenshot of the nav bar and CIN section. The Darija translations across both batches have not been proofread by a native speaker.
- **Next up:**
  1. Have a Darija speaker proofread the `data-ary` translations added across both batches (`#epargne`, `#sante`, `#electoral`, `#tajnid`, `#passjeunes`, `#autoentrepreneur`, `#rnp`, `#utilities`, `#chikaya`) — written by the AI assistant mirroring the file's existing vocabulary, not verified by a native speaker.
  2. Verify under-18 eligibility details for the youth savings accounts in `#epargne` (legal-guardian angle) — not yet confirmed against an official source.
  3. Fill the gaps the Idarati chatbot was supposed to surface (it errored out) — casier judiciaire, carte grise and CNSS are now all covered; re-check if anything else was on that original list.
  4. Possibly archive backup copies of the passeport.ma legal-text PDFs the way the alhalalmadania ones were archived.

## 8. Update Protocol (Verbatim)

> **For the AI Assistant:** When asked to "Update CONTEXT.md":
> 1. Re-run Phase 0 — check for new `GEMINI.md` / `CLAUDE.md` / `.github/` files.
> 2. Re-scan the tree, manifests, and `.github/workflows/` for drift.
> 3. Read our recent conversation for new decisions, fragile bits discovered, or shifted goals.
> 4. Refresh the `_Last synced_` line with today's date and current commit SHA.
> 5. Rewrite — do not append. One clean source of truth. Preserve still-true content, revise the rest.
> 6. Keep §1 and §2 in plain English. Keep the file under ~350 lines.
