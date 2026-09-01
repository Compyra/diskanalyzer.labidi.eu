# diskanalyzer.labidi.eu

Download page for **Labidi Disk Analyzer**, the portable disk space analyzer
and snapshot differ built in [Compyra/Labidi-Disk-Analyzer](https://github.com/Compyra/Labidi-Disk-Analyzer).
Static site, GitHub Pages behind Cloudflare, family style of labidi.eu and
compyra.com: no build step, no cookies, no analytics, no external requests.

## What is served

| File | Purpose |
|---|---|
| `index.html` | The page: hero with a window replica, features, CLI, download, FAQ |
| `style.css` / `script.js` | Versioned assets (`?v=N`, bump both on redeploy) |
| `LabidiDiskAnalyzer.exe` | Windows x64 build (window + CLI) |
| `LabidiDiskAnalyzer-windows-arm64.exe` | Windows arm64 build |
| `LabidiDiskAnalyzer-linux-amd64` | Linux x64 build (CLI) |
| `SHA256SUMS.txt` | Hashes of the three binaries above |
| `og-image.html` | Source of `og-image.png` (1200x630, shot with headless Edge) |

The binaries are self-hosted because the source repository is private, so
GitHub Releases links would 404 for visitors. If the repo ever goes public,
the download buttons can point at
`https://github.com/Compyra/Labidi-Disk-Analyzer/releases/latest/download/<name>`
instead.

## Shipping a new build

1. In the analyzer repo: `.\build.ps1 -All -Release` (or take a tagged CI build).
2. Copy the three binaries from `dist\` over the ones here.
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
