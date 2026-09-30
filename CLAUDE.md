# mfd-ot-recall - repo rules

**Retrofitted by `/init-client --retrofit` on 2026-08-21 from what was on disk.** Every line below
is either DETECTED or explicitly UNKNOWN. Nothing here was inferred and then stated as fact. Correct
it the first time you work in this repo.

## Stack
Single-file static HTML/CSS/JS. No framework.

## Deploy targets (two, measured 2026-09-29)
1. **Netlify `mfdr4rsoftware`, https://mfdr4rsoftware.netlify.app/ . Git-linked to `MFDR4R` `main`.**
   Netlify API (`GET /api/v1/sites`): `build_settings.repo_url` = this repo, branch `main`, published
   deploy `6a8fecee174df600084d3491` carries `commit_ref` `c6832b7`. It also serves `CLAUDE.md` and
   `README.md` (both 200), so it publishes a checkout of the whole repo root.
2. **Hostinger `mfdrecall.frontlinewebdesign.tech`** (account `u987655740`, root
   `public_html/mfdrecall`). Zip deploy only: it 404s `CLAUDE.md`. Its `.htaccess` holds only
   `Header set X-Robots-Tag "noindex, nofollow"`, so keep that file in every zip.

## Does a push publish?
**YES, to Netlify.** A push to `main` rebuilds https://mfdr4rsoftware.netlify.app/ . Verify locally
before every push. **Hostinger does NOT update on push**: redeploy it by zip through the Hostinger
MCP (`hosting_deploy-static-website`), or it falls behind. It sat on commit `e4ee423` from
2026-06-23 until 2026-09-29.

The 2026-08-21 retrofit said NO here. That was wrong: `gh api .../hooks` returned nothing, but the
Netlify link is real, per the API read above.


## Remote
`git@github.com:tannermosher2015-debug/MFDR4R.git`, branch `main`.

## Verify path
`shot.ps1` desktop + mobile, **both reviewed**, plus `impeccable detect` on the
built HTML. Every edit gets both shots before a deploy, including one-character ones.

## Landmines
- 2026-09-29: Netlify serves every file in the repo root, including this file and `docs/`. Keep
  nothing private in the repo. The repo is also PUBLIC on GitHub.
