# gravity

Interactive force-directed graph of a GitHub user's repositories and the repos they've contributed to.

## Host this for your account (5 minutes)

1. **Fork or import this repo** into your own GitHub account:
   - **Public**: click **Fork** and select your account.
   - **Private**: go to [github.com/new/import](https://github.com/new/import), paste this repo's URL, set visibility to private, and import.

2. **Create a Personal Access Token (PAT)** with `repo` (or `public_repo` for public-only) scope:
   - Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**
   - Add it as a repository secret named `GH_TOKEN` under **Settings → Secrets and variables → Actions**.

3. **Enable GitHub Pages**: go to **Settings → Pages → Branch: `main` / Folder: `/docs`** and save.

4. **Run the workflow**: go to **Actions → Update visualization → Run workflow**.
   Data is fetched automatically every Monday at 03:00 UTC after that.

Your graph will be live at `https://<your-username>.github.io/<repo-name>/`.

---

## What you'll see

- **Repos tab** — all repos you own, plus repos you've recently contributed to (from your public events). Owned and contributed repos have slightly different styling. Edges connect repos that have dependency relationships.
- **Contributors tab** — everyone you've co-contributed with (both committed to the same repo), shown as a network.

---

## Local usage

```bash
# Set your GitHub token (needs repo or public_repo scope)
export GITHUB_TOKEN=ghp_xxxx

uv run fetch.py <your-username>
```

Then serve the `docs/` folder locally:

```bash
python3 -m http.server 8000 --directory docs
```

Open `http://localhost:8000` in your browser.

## Creating a GitHub Token

`GITHUB_TOKEN` needs a Personal Access Token (PAT), **not** an SSH key.

1. Go to https://github.com/settings/tokens
2. Click **Generate new token (classic)**
3. Grant scopes: `repo` (or `public_repo` for public repos only)
4. Copy the token and export it or save to `.env`:

```bash
GITHUB_TOKEN="ghp_yourActualTokenHere"
GITHUB_USER="your-username"
```

## Options

```
uv run fetch.py <username> [--output PATH] [--skip-forks] [--skip-archived] [--no-contributed] [--limit N]
```

| Flag | Default | Description |
|---|---|---|
| `--output` | `docs/data.json` | Output path for the JSON data |
| `--skip-forks` | false | Skip forked repos (owned repos only) |
| `--skip-archived` | false | Skip archived repos (owned repos only) |
| `--skip-private` | false | Skip private repos (owned repos only) |
| `--no-contributed` | false | Only show owned repos, skip contributed-to repos |
| `--limit N` | all | Process at most N repos total (useful for testing) |

## Notes on contributed repos

Contributed repos are discovered from your recent public GitHub events (last ~300 events, ~90 days). If you contributed to a repo more than 90 days ago and haven't had any recent activity there, it may not appear. Only repos where your account is not the owner are included.
