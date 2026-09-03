# diskanalyzer.labidi.eu

Download page for **Labidi Disk Analyzer**, the portable disk space analyzer
and snapshot differ built in [Compyra/Labidi-Disk-Analyzer](https://github.com/Compyra/Labidi-Disk-Analyzer).
Static site, GitHub Pages behind Cloudflare, family style of labidi.eu and
compyra.com: no build step, no cookies, no analytics, no external requests.
scanner.labidi.eu shares this exact layout; when updating one site, mirror
the other.

## What is served

| File | Purpose |
|---|---|
| `index.html` | The page: hero with a window replica, features, CLI, download, FAQ |
| `style.css` / `script.js` | Versioned assets (`?v=N`, bump both on redeploy) |
| `download/LabidiDiskAnalyzer.exe` | Windows x64 build (window + CLI) |
| `download/LabidiDiskAnalyzer-windows-arm64.exe` | Windows arm64 build |
| `download/LabidiDiskAnalyzer-linux-amd64` | Linux x64 build (CLI) |
| `SHA256SUMS.txt` | Hashes of the three binaries (per-file `.sha256` sidecars sit next to them) |
| `og-image.html` | Source of `og-image.png` (1200x630, shot with headless Edge) |

The page has one ambient touch: `.disk-fill` bars quietly fill once on load
and then once every 2 minutes (a single 120s CSS cycle, visible in its first
~5%), hidden under `prefers-reduced-motion`. scanner.labidi.eu does the same
with its `.radar-ping`.

The binaries are self-hosted because the source repository is private, so
GitHub Releases links would 404 for visitors. If the repo ever goes public,
the download buttons can point at
`https://github.com/Compyra/Labidi-Disk-Analyzer/releases/latest/download/<name>`
instead.

## Shipping a new build

1. In the analyzer repo: `.\build.ps1 -All -Release` (or take a tagged CI build).
2. Copy the three binaries from `dist\` into `download\` here, together with
   their `.sha256` sidecars.
3. Regenerate `SHA256SUMS.txt` from the new files and update in `index.html`:
   the three hashes (both the visible prefix and the `data-copy`/`title`
   values), the verify example hash, the build commit and date line, and the
   file sizes if they changed.
4. Bump `?v=N` on `style.css`/`script.js` references only when those files
   change (Cloudflare edge-caches assets for hours; HTML only 10 minutes).

## Local check

```
python -m http.server 8741 --bind 127.0.0.1
```

The page is theme-aware (dark default, light via the header toggle,
`prefers-color-scheme` respected on first visit) and fully functional
without JavaScript apart from the theme toggle and copy buttons.
