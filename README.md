# Démarches Utiles — Maroc 🇲🇦

A single-page, no-nonsense guide to the Moroccan admin procedures a young person (16–30) actually runs into: getting your ID card, opening a bank account, passing your driving licence, registering your car, applying for a scholarship, and more — all in one place, with the real requirements, official links, and a checklist you can tick off.

**Why:** Moroccan admin info is scattered across dozens of government portals that each tell you a slightly different story. This page collects the real, sourced requirements so you don't have to re-research everything every time.

🔗 Just open [`index.html`](./index.html) in your browser — no install, no build, no server needed.

## What's inside

24 topics, each with the required documents, fees, delays, and links to the official portals:

| Topic | Topic |
|---|---|
| 🪪 Carte d'Identité Nationale (CNIE) | 🩺 Assurance maladie (santé) |
| 🎫 Pass Jeunes | 👤 Registre National de la Population (RNP) |
| 📘 Passeport biométrique | 💍 Mariage et Livret de Famille (Adoul) |
| 🏦 Compte bancaire jeune | 📄 Casier judiciaire |
| 💰 Épargne / investissement | 🚙 Carte grise |
| 🧾 Auto-entrepreneur | 🔌 Contrats eau / électricité |
| 🚗 Permis de conduire | 💵 Impôts locaux (vignette, taxe d'habitation) |
| 🛡️ Assurance auto | 📢 Portail des réclamations (Chikaya) |
| 🚨 Constat amiable (accident) | 🗳️ Inscription sur les listes électorales |
| | 🎖️ Service militaire (Tajnid) |
| | 🎓 Bourse Minhaty |
| | 📜 Légalisation de diplômes |
| | 🇫🇷 Certifications de français (TCF, DELF, DALF) |
| | 🇪🇺 Corps Européen de Solidarité |
| | 🌐 Portails numériques utiles |

Each section has a tickable checklist (saved locally in your browser) and a 🇫🇷 / Darija (دارجة) language toggle.

## Running it locally

No install needed — just open `index.html`. If you want the embedded PDF viewers (ID photo standards, etc.) to work, serve it over HTTP instead of `file://`:

```bash
python3 -m http.server 8899
# then open http://localhost:8899/index.html
```

## Tech

Plain HTML, CSS and vanilla JavaScript — zero dependencies, no build step, no backend. `index.html` is the entire app; `assets/` holds the PDFs and images referenced from it.

## Project files

- `index.html` — the whole app (content, styles, and the small script that handles the checklist + language toggle).
- `assets/` — official PDFs (photo norms, driving-licence guides, civil-status circulars, French-certification leaflets) and images (flag, coat of arms, visa map) used in the page.
- `quotes.md` — unrelated personal notes, not part of the site.
- `CONTEXT.md` — the detailed technical/context doc for AI assistants and contributors picking this project back up later.

## Contributing / editing

This is a personal reference, kept public and zero-dependency on purpose. If you're extending it:

- Only add facts you can cite an official source for.
- Keep it a single HTML file — no frameworks, no build tooling.
- Add both the French text and its Darija translation (`data-ary` attribute) for every new piece of visible text.

See [`CONTEXT.md`](./CONTEXT.md) for the full details, fragile bits, and current status.

---

*Personal project — not an official government resource. Always double-check against the linked official portals.*
