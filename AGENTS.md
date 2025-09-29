# Repository Guidelines

## Project Structure & Module Organization
- `index.html`: Single-page UI entry (zh-CN locale).
- `static/`: Front-end assets
  - `style.css`, `fontawesome.css`, `script.js`, images (`*.png`), `fonts/`.
- `worker.js`: Cloudflare Worker that serves JSON APIs and simple admin actions; expects KV binding `MY_HOME_KV`.
- `README.md`, `LICENSE`: Repo docs and licensing.

## Build, Test, and Development Commands
- Local preview (static site):
  - `python3 -m http.server 8080` → visit `http://localhost:8080`.
  - Alternatively: `npx serve .` (if Node is available).
- Worker (Cloudflare): Requires Wrangler and a KV binding.
  - Minimal `wrangler.toml` example:
    - `name = "home-page-worker"`
    - `main = "worker.js"`
    - `compatibility_date = "2024-01-01"`
    - `kv_namespaces = [{ binding = "MY_HOME_KV", id = "<your-kv-id>" }]`
  - Run locally: `wrangler dev` (in repo root with `wrangler.toml`).

## Coding Style & Naming Conventions
- HTML: Semantic sections; class names in kebab-case (e.g., `main-container`, `contribution-section`).
- CSS: 4-space indentation; keep styles in `static/style.css`; avoid inline styles.
- JavaScript: ES2015+ with `const/let`, semicolons, async/await; keep logic in `static/script.js`. Do not embed secrets or tokens.
- Assets: Lowercase, hyphen-separated filenames (e.g., `hero-bg.png`).

## Testing Guidelines
- Front-end: Open in a browser, verify layout and console (Network tab for API calls). Test at common widths (mobile/desktop).
- Worker APIs: With Wrangler dev running, e.g., `curl http://localhost:8787/api/data`.
- There is no unit test framework configured; manual verification and screenshots are required for UI changes.

## Commit & Pull Request Guidelines
- Commits: Short, imperative subject; Chinese or English acceptable. Prefer scope when helpful.
  - Examples: `fix: data auth check`, `更新主题暗色切换`.
- PRs: Provide a clear description, linked issues, before/after screenshots for UI, and test steps. Avoid unrelated formatting-only diffs.

## Security & Configuration Tips
- Configure KV binding `MY_HOME_KV` in Cloudflare; do not commit credentials.
- `static/script.js` contains `API_BASE_URL`—point this to your Worker domain if used.
- Review CORS needs; Worker defaults to permissive `Access-Control-Allow-Origin`. Restrict for production as needed.

