# Phinix — deploy package (Pi Desktop / SoloHost)

## What changed
The platform requires the docker-compose "image:" to point at an already-published
public container image — it does NOT support "build:" from local source. So the
image has to be built and pushed to a registry first, then referenced by URL.

## Steps
1. **Push this whole `phinix-deploy` folder to a new GitHub repo** (public or
   private, either works for this).
2. In that repo's **Settings → Actions → General → Workflow permissions**, select
   **"Read and write permissions"** and save. This lets the included GitHub Action
   push the built image for you automatically.
3. Push to the `main` branch (or just push once after creating the repo) — the
   workflow at `.github/workflows/publish.yml` builds the Docker image and pushes
   it to `ghcr.io/<your-github-username>/phinix-app:latest` automatically. Check
   the "Actions" tab in GitHub to see it run and confirm it succeeds (green check).
4. Go to your GitHub profile → **Packages**, open `phinix-app`, → **Package
   settings** → change visibility to **Public** (Pi Desktop needs to pull it
   without auth).
5. In `docker-compose.yml` (both here and pasted into the platform's form),
   replace `YOUR_GITHUB_USERNAME` with your actual GitHub username.
6. Paste the **exact contents** of `config_options.yml` and `docker-compose.yml`
   into the platform's "2. Package" step, then Validate package again.

## Files
- `public/index.html` — the whole Phinix app UI, bundled into one static file.
- `server.js` + `package.json` — minimal Express static file server.
- `Dockerfile` — builds the image the GitHub Action publishes.
- `.github/workflows/publish.yml` — builds & pushes the image to GHCR on every push to main.
- `config_options.yml` / `docker-compose.yml` — paste these into the platform form as-is
  (after swapping in your GitHub username).
