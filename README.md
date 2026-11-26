# Tau Pore: phone version (portrait)

A one-minute, tap-through cartoon about tau at the nuclear pore, built for visitors who scan a QR code at Jack Tiller's poster (ISP conference, St. Petersburg, FL, December 2026). It is a self-contained static site made with vanilla HTML, CSS, and JS. It has no CDN, no tracking, and no build step. It is an offline-capable PWA.

The iPad booth app (`/workspace/tau-poster-app`) is untouched. This is a separate, public-safe app.

## Screens (11 in all, about 1 to 2 minutes)
| # | Screen |
|---|---|
| 0 | Landing: title, Start button |
| 1 | A healthy brain cell |
| 2 | In Alzheimer's, its nucleus fails (pores jam, nucleus leaks, DNA breaks, connections fade) |
| 3 | Tau holds microtubules together |
| 4 | Phosphates make tau let go |
| 5 | Tau drifts to the nuclear pore and sticks |
| 6 | Charge puzzle, two taps: "Both are positive... why do they stick?", then "Where the phosphates sit matters" |
| 7 | Takeaway: it's not just how much charge, it's where |
| 8 | Why it matters: three sourced Alzheimer's facts |
| 9 | Three-question tap quiz with friendly feedback and a score |
| 10 | About and learn more |

Navigation: tap the left 40% of a story screen to go back and the right 60% to go forward. Swiping, the Back and Next buttons, and the arrow keys also work. A segmented progress bar runs along the top. Portrait is the design target (390x844, works at 360 to 430 wide, safe-area aware). Landscape also works, with the text on the left and the drawing on the right.

## Deploy to GitHub Pages (repo name: `tau-pore`)
The public URL is `https://jacktiller.github.io/tau-pore/`. An empty `.nojekyll` file is in the folder so GitHub Pages does not run Jekyll, and every asset path is relative so the app works under the `/tau-pore/` subpath.

1. Create a new **public** GitHub repo called `tau-pore` under the `jacktiller` account, if it does not exist yet.
2. Put the deployable files in the repo root: `index.html`, `manifest.webmanifest`, `sw.js`, `icons/`, and `.nojekyll`. `README.md` is optional. Do not upload `screenshots/` or `tools/`.
3. In the repo go to Settings > Pages. Under Source choose "Deploy from a branch", then pick `main` and `/ (root)`, and save.
4. After a minute, the site is live at `https://jacktiller.github.io/tau-pore/`.
5. Open that link on your phone once while online to check it. The service worker then caches the app, so it also opens with no signal. The links on the About card and the facts card still need internet.

If you change any file, bump `VERSION` in `sw.js` (currently `tau-pore-phone-v5`). Phones then pick up the new copy on the next two loads, because the cache is served first and refreshed in the background.

## Make the QR code
`qr.png` in this folder already encodes `https://jacktiller.github.io/tau-pore/`. It is black on white, error-correction level Q, a 4-module quiet zone, and at least 1200 px square, using whole-pixel modules so print stays sharp. After printing, scan it with two different phones before you leave for the conference. To regenerate it:
```
pip install segno          # or: pip install "qrcode[pil]"
python3 make_qr.py https://jacktiller.github.io/tau-pore/
```
Options: `--out poster-qr.png`, `--size 2400`.

## What was removed versus the iPad app
- **Dive deeper (expert mode)** is gone: the charge plots, isoform and state tiles, the Nup98 contrast screen, and all charge computed from the UniProt sequences. The sequences, `data.js`, and the poster figures are not copied over.
- **No poster-specific numbers.** There are no region charges, no PAD12 sites or definitions, no KXGS site list, and no binding constants. The story is qualitative ("where the charge sits matters"). The code and page source contain none of these, because the link stays public.
- **The open question about phosphates on the grip and beside it** is dropped from beat 2. Beat 2 now says "Same kind of charge, different place, different effect." The two callouts (extra charge at the far end gives no change, phosphates beside the grip change sticking) match published findings in Diez et al. 2022 (checked against the full text: N-terminal insert charge did not change Nup98 binding, while phosphorylation strongly did).
- **TOUCH ME start screen, the 90-second idle reset, and the Add to Home Screen tip** are gone. They were for a shared booth iPad. A short landing screen with a Start button replaces them.
- **Landscape-only lock** and the "turn the iPad sideways" prompt are gone. The manifest is `orientation: any`.
- **Layout:** all cartoons were redrawn for portrait (a taller neuron, a bigger microtubule with two taus, and a vertical pore with tau coming down into it). The charge bars were rebuilt as stacked bars.
- **Added:** the Why it matters screen, the quiz, and the About and learn-more card.

## Facts on the "Why it matters" screen (checked against the source pages)
1. **7.4 million** Americans age 65 and older are living with Alzheimer's in 2026, about 1 in 9 people in that age group. This is from Alzheimer's Association, *Alzheimer's Disease Facts and Figures* ("Prevalence"), https://www.alz.org/alzheimers-dementia/facts-figures. The page was read on 2026-11-26.
2. **1 in 3** older Americans dies with Alzheimer's or another dementia. This is from the same page ("Quick facts" and "Mortality and morbidity").
3. **Tangles track symptoms.** Autopsy studies find that neocortical tau tangles correlate strongly with cognitive decline, more closely than amyloid plaques do. The source is Nelson PT, Braak H, Markesbery WR, *J Neuropathol Exp Neurol* 2009;68(1):1-14, https://pmc.ncbi.nlm.nih.gov/articles/PMC2692822/. The full text was checked, and its section on tangles and cognitive decline supports this wording.

## About card: contacts and published review
The last screen has LinkedIn (`https://www.linkedin.com/in/jacktiller/`), email (`mailto:jbtiller@utmb.edu`), GitHub (`https://github.com/jacktiller`), and one paper link, Jack's 2026 review (he is the second author, after Abbigael Aday):

Aday A, Tiller J, Melukote A, Walton ZC, King A, Castillo L, Kayed R, Montalbano M. Nuclear dysfunction in aging and neurodegeneration. *Alzheimer's & Dementia*. 2026;22(7):e71657. https://doi.org/10.1002/alz.71657

Metadata was checked against Crossref (`https://api.crossref.org/works/10.1002/alz.71657`) on 2026-11-26: title, author order, journal, volume 22, issue 7, article e71657, published online 2026-07-12. The card links to the Wiley page, `https://alz-journals.onlinelibrary.wiley.com/doi/10.1002/alz.71657`.

## Files
- `index.html`: the whole app (CSS, JS, and drawings inline).
- `manifest.webmanifest`, `sw.js`, `icons/`: install and offline support. `make_icons.py` regenerates the icons (needs pillow).
- `make_qr.py`: QR code generator.
- `screenshots/`: renders from headless Chrome at 390x844, 360x780, 430x932, and 844x390. `contact-sheet.png` shows all portrait screens at 390x844. `contact-sheet-landscape.png` shows the landscape renders.
- `tools/`: test scripts (`shoot.py` renders and checks overflow, small text, and console errors; `func.py` tests navigation, the quiz, and offline use; `sheet.py` builds the contact sheets). They need `pip install playwright` and Chrome.
- Test URLs: `?s=0..10` jumps to a screen, `&beat=2` shows the second charge beat, `&shot` freezes animations, `&qi=0..2&qa=0..2` previews a quiz answer, and `&qdone=0..3` previews the score.
