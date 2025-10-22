# Opename Ops Playbook (Advice Bomb)

**Snapshot (today)**
- **Site:** https://opename.org — deployed from `rickballard/Opename` (GitHub Pages, custom domain + CNAME).
- **Redirects:** `opename.com → 301 → opename.org` confirmed.
- **Repos:** `Opename` (site), `MeritRank` (tools/specs).
- **Flows:** Evidence submission (GitHub Issue template) + Evidence stream page listing `evidence` issues.
- **Docs:** `Opename/.github/workflows/pages.yml` syncs `MeritRank/docs/site → Opename/docs` at deploy time (optional).
- **Meta:** robots, sitemap, OG/Twitter tags, security.txt, 404, Privacy/Terms.

---

## Keep names vs. rename
- **Recommendation:** Keep `MeritRank` and `Opename` as-is for now (everything is wired).
- If you later want a cleaner brand: create org **`opename`**, then transfer:
  1) `rickballard/Opename` → `opename/opename`
  2) `rickballard/MeritRank` → `opename/meritrank`
  3) Update site links + workflow to point to `opename/meritrank`.
  4) Update local remotes:
     ```powershell
     git -C "$HOME\Documents\GitHub\Opename"   remote set-url origin https://github.com/opename/opename.git
     git -C "$HOME\Documents\GitHub\MeritRank" remote set-url origin https://github.com/opename/meritrank.git
     ```

---

## Pages & docs
- Site deploys from the **`main`** branch root of `Opename`.
- The Pages workflow copies any files from **`MeritRank/docs/site` → `Opename/docs`** during build.
- To publish new docs, add HTML into `MeritRank/docs/site` and push.

---

## Evidence flow (quick reference)
- **Submit:** https://opename.org/submit.html → prefilled issue in `MeritRank` with label **`evidence`**.
- **Stream:** https://opename.org/evidence.html → lists open `evidence` issues via GitHub API.

---

## DNS & redirects (GoDaddy)
- Apex `opename.org` → GitHub Pages A records.
- `www.opename.org` → `rickballard.github.io` (CNAME).
- `opename.com` forwarding (301) to `https://opename.org/` handled at registrar/CDN.
- Sanity checks:
  - A: 185.199.108.153 / .109 / .110 / .111
  - CNAME: `www.opename.org → rickballard.github.io`

---

## One-command smoke tests (PowerShell)
```powershell
'https://opename.org/',
'https://opename.org/submit.html',
'https://opename.org/evidence.html',
'https://opename.org/docs/',
'https://opename.org/privacy.html',
'https://opename.org/terms.html' |
  ForEach-Object { "{0} -> {1}" -f $_, (Invoke-WebRequest $_ -UseBasicParsing).StatusCode }

# Redirects (.com -> .org)
'http://opename.com','http://www.opename.com','https://www.opename.com','https://opename.com' |
  ForEach-Object {
    try { Invoke-WebRequest $_ -MaximumRedirection 0 -Headers @{ 'User-Agent'='curl/8.5.0' } }
    catch {
      "{0} -> {1} {2}" -f $_, $_.Exception.Response.StatusCode.value__, $_.Exception.Response.Headers.Location
    }
  }
<!-- BEGIN: STATUS -->
### Operational Status
CoDrift Index: n/a% (n/a)
<!-- END: STATUS -->

