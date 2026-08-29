# useful-stuffs — AI Context File
_Last synced: 2026-08-29 @ dbc17cb_

## 1. What This Is (Plain English)

- **In one sentence:** A single web page that explains, step by step, how to get the Moroccan documents and civic obligations a young person actually needs — national ID card, passport, bank account, savings/investment, driving licence, car and health insurance, voter registration, military service — with tickable checklists and links to the official government sites.
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
- **External services:** None at runtime. The page only *links out* to Moroccan government and financial portals (cnie.ma, passeport.ma, idarati.ma, wraqi.ma, consulat.ma, watiqa.ma, alhalalmadania.ma, sgg.gov.ma, maroc.ma, cnss.ma, cnops.org.ma, listeselectorales.ma, tajnid.ma, recrutement.far.ma, casablanca-bourse.com, ammc.ma, bkam.ma, albaridbank.ma, groupebcp.com, tamwilcom.ma, and others). Nothing is fetched or posted.

## 4. Code Map (The Important Files Only)

- `index.html` — **the whole app.** ~1445 lines: a `<style>` block (~263 lines), the content markup (13 sections), and a `<script>` block (~42 lines) at the bottom. This is the only file you ever need to edit for content.
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

- **Last shipped:** Four new sections aimed at 16–25 year-olds, inserted between the existing ones (all sourced per §5, all with Darija `data-ary` translations):
  - **`#epargne` (Épargne et investissement)**, after `#banque` — youth/national savings accounts (Al Barid Bank *Tawfir Al Ghad*, CEN, compte sur carnet incl. Banque Populaire and CIH's *Iskane*), compte-titres + PEA + OPCVM on the Bourse de Casablanca (BCP's own "Bourse en ligne" included), Intelaka/Forsa financing, and a crypto section framed as a **legal-risk warning, not a recommendation** (still effectively banned pending bill 42.25 as of 2026).
  - **`#sante` (AMO, CNSS, CNOPS)**, after `#assurance` — who manages what (CNSS = privé, CNOPS = public, a CNOPS/CNSS merger law adopted late 2025 with the effective timeline still unclear), the AMO Étudiants scheme (400 DH/12 mois, CNOPS-managed), and first-job affiliation.
  - **`#electoral` (Listes électorales)** and **`#tajnid` (Service militaire — Tajnid)**, both after `#cartegrise` — voter registration via listeselectorales.ma (conditions, the annual and *date-shifting* registration window, SMS verification via 2727), and the tajnid.ma military census under loi 44-18 (19–25 ans, 12 mois, 20-day census deadline), with recrutement.far.ma also linked as the FAR's own recruitment portal.
  - Nav pills, footer "last updated" date, and CONTEXT.md's own code map/external-services list were all updated to match.
- **Working on now:** Nothing in flight. The four new sections have been checked with Python's `html.parser` (no structural errors) and a headless-Chrome screenshot of the nav bar and CIN section; the Darija translations have not been proofread by a native speaker.
- **Next up:**
  1. Fill the gaps the Idarati chatbot was supposed to surface (it errored out) — likely candidates are casier judiciaire (already covered) and carte grise (already covered); CNSS is now covered too via `#sante` — re-check if anything else was on that original list.
  2. Possibly archive backup copies of the passeport.ma legal-text PDFs the way the alhalalmadania ones were archived.
  3. Have a Darija speaker proofread the `data-ary` translations added in `#epargne`, `#sante`, `#electoral` and `#tajnid` — they were written by the AI assistant mirroring the file's existing vocabulary, not verified by a native speaker.
  4. Verify under-18 eligibility details for the youth savings accounts in `#epargne` (legal-guardian angle) — not yet confirmed against an official source.

## 8. Update Protocol (Verbatim)

> **For the AI Assistant:** When asked to "Update CONTEXT.md":
> 1. Re-run Phase 0 — check for new `GEMINI.md` / `CLAUDE.md` / `.github/` files.
> 2. Re-scan the tree, manifests, and `.github/workflows/` for drift.
> 3. Read our recent conversation for new decisions, fragile bits discovered, or shifted goals.
> 4. Refresh the `_Last synced_` line with today's date and current commit SHA.
> 5. Rewrite — do not append. One clean source of truth. Preserve still-true content, revise the rest.
> 6. Keep §1 and §2 in plain English. Keep the file under ~350 lines.
