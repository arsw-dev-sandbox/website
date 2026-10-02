# arsw.cloud sandbox

The website for `arsw.cloud`, served from Cloudflare Workers in the site owner's own Cloudflare account.

## How it's hosted

- **Hosting:** a Worker named `sandbox` serving the files in `public/` (static assets). Unknown paths get
  `public/404.html` with a real 404 status. Response headers come from `public/_headers`.
- **Domains:** `arsw.cloud` and `www.arsw.cloud` are attached to the Worker; `www` redirects to the apex. Both are set up
  in Cloudflare, outside this repository.
- **DNS:** Cloudflare, in the same account. Email records are untouched by the site.

## Deploying

- **Every push to `main` deploys**, through Workers Builds (Cloudflare's CI, connected to this repository). Build
  logs: Cloudflare dashboard → Workers & Pages → `sandbox` → Deployments.
- **Every other branch gets a preview**: its pull request shows a preview link from Cloudflare.
- **Pull requests run CI** (`.github/workflows/ci.yml`): install, build and a dry-run deploy. "CI Result" and
  "Workers Builds: sandbox" must pass before merging.
- **Rollback:** Cloudflare dashboard → the Worker → Deployments → choose an earlier version → Rollback. Then revert the
  change on `main`, or the next merge deploys it again.

This repository holds no secrets. Workers Builds deploys with a token that belongs to the site owner's Cloudflare
account.

## Working on it

Needs Node (the version in `.node-version`) and pnpm.

```sh
pnpm install
pnpm run dev      # http://localhost:8787
pnpm run build
```

New package versions install only once they're 3 days old (`pnpm-workspace.yaml`); Dependabot waits the same.

## Taking it over

Everything is in the owner's accounts: this repository, the Cloudflare account (DNS, the Worker, Workers Builds) and
the domain. A new developer needs admin on this repository and a member role in the Cloudflare account; nothing else.
