# Three Couplings of Two Fair Dice

An interactive, single-page explainer for **couplings** in probability and **optimal transport**, told through the classic two-fair-dice example.

Roll a pair of dice under three different *couplings* of the **same** fair-die marginals, watch the dice tumble, and read off every relevant quantity — both the exact closed form and a live empirical estimate that converges as you roll.

> **The one idea:** look at either die alone and all three set-ups are identical (six faces, each 1/6). The difference lives entirely in the **joint** law. Optimal transport asks which joint law minimises a chosen expected cost; for squared distance the **comonotone** coupling wins.

## The three couplings

Two fair dice `X`, `Y`, each uniform on `{1,…,6}`. Cost `c(x,y) = (x − y)²`, with `Var(X) = 35/12`.

| Coupling | Rule | E[(X−Y)²] | Correlation | P(double) | Support of X+Y | Status |
|---|---|---|---|---|---|---|
| **Independent** | `X ⫫ Y` | `35/6 ≈ 5.833` | `0` | `1/6` | `{2,…,12}` | baseline |
| **Comonotone** | `Y = X` | `0` | `+1` | `1` | `{2,4,6,8,10,12}` | **optimal** (min cost) |
| **Antithetic** | `Y = 7 − X` | `35/3 ≈ 11.667` | `−1` | `0` | `{7}` | **worst** (max cost) |

All three share **identical** uniform marginals — same edges on the lattice, very different interiors. Every number above is reproduced exactly in the app and was verified by enumerating the full 36-cell joint distribution.

## Features

- **Coupling selector** with a one-line description of each.
- **3D CSS dice** that tumble on *Roll* and settle on outcomes that respect the selected coupling (comonotone → matching faces; antithetic → faces sum to 7; independent → drawn separately).
- **6×6 joint-probability lattice** with the support highlighted and the always-uniform marginals framing the edges. An inner square in each cell grows toward its empirical frequency as you roll.
- **Live empirical statistics** — running `E[(X−Y)²]`, correlation, and `P(double)` shown beside their exact theoretical values, with an **Auto-roll ×1000** button so you can watch them converge.
- **Sum distribution panel** — the theoretical law of `X + Y` as bars, overlaid with the empirical histogram so far.
- **Exact-values panel** with a badge marking the optimal / worst coupling for the quadratic cost.
- Responsive down to mobile, keyboard-accessible tabs (arrow keys), visible focus, and `prefers-reduced-motion` respected. Press **Space** to roll.

## Run locally

It is a single static file with no build step and no dependencies (fonts load from Google Fonts; everything else is inline).

```bash
# simplest: just open the file
open index.html            # macOS  (use: xdg-open index.html on Linux)

# or serve it (recommended, mirrors GitHub Pages)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploy to GitHub Pages

### Option A — GitHub Actions (included, recommended)

A workflow is provided at `.github/workflows/deploy.yml`. After pushing this repo to GitHub:

1. Go to **Settings → Pages → Build and deployment** and set **Source** to **GitHub Actions**.
2. Push to the `main` branch (or run the workflow manually from the **Actions** tab).

The workflow uploads the repository root as the Pages artifact and deploys it — no build required. Your site appears at `https://<user>.github.io/<repo>/`.

### Option B — Branch / folder, no Actions

1. Push the repo to GitHub.
2. **Settings → Pages → Build and deployment → Source: Deploy from a branch**.
3. Choose branch `main` and folder `/ (root)`. (If you prefer, move `index.html` into a `docs/` folder and pick `/docs` instead.)
4. Save; the site builds in a minute or two.

### A note on the base path

The brief mentions setting a base path (e.g. Vite's `base: '/REPO_NAME/'`). **This project does not need one.** It is a single self-contained `index.html` whose only external reference is an absolute Google Fonts URL, so it works at any sub-path — `https://user.github.io/repo/` — without configuration.

The base-path footgun only appears once you introduce a bundler that emits *root-absolute* asset links like `/assets/app.js`. If you later port this to **Vite + React**, you would:

```js
// vite.config.js
export default { base: '/<REPO_NAME>/' }  // must match the GitHub Pages sub-path
```

and build with `npm run build`, then deploy the `dist/` folder (swap the upload path in the workflow to `./dist` and add a build step). Keeping it dependency-free sidesteps all of that.

## Project structure

```
dice-couplings/
├── index.html                  # the entire app: HTML + CSS + JS, no dependencies
├── README.md
├── LICENSE
└── .github/
    └── workflows/
        └── deploy.yml          # GitHub Pages deployment (Option A)
```

## Why no framework?

The brief recommends React + Vite but allows plain HTML/JS "if simpler." For a self-contained single-page visualisation, vanilla HTML/CSS/JS is the more robust GitHub Pages target: zero build, no dependency drift, no base-path configuration, and it renders instantly. Charts are hand-drawn with inline SVG and the dice are pure CSS 3D, so the whole site is one file.

## License

MIT — see [LICENSE](LICENSE).
