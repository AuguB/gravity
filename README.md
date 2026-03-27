# gravity

Interactive force-directed graph of a GitHub user's repositories and the repos they've contributed to.

## Host this for your account (5 minutes)

1. **Fork or import this repo** into your own GitHub account:
   - **Public**: click **Fork** and select your account as the destination.
   - **Private**: go to [github.com/new/import](https://github.com/new/import), paste this repo's URL, set visibility to private, and import.

2. **Create a Personal Access Token (PAT)** with `repo` scope (or `public_repo` for public-only):
   - Go to **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**
   - Add it as a repository secret named `GH_TOKEN` under **Settings → Secrets and variables → Actions**.

3. **Enable GitHub Pages**: go to **Settings → Pages → Branch: `main` / Folder: `/docs`** and save.

4. **Run the workflow**: go to **Actions → Update visualization → Run workflow**.
   Data is fetched automatically every Monday at 03:00 UTC after that.

Your graph will be live at `https://<your-username>.github.io/<repo-name>/`.

---

## What you'll see

- **Repos tab** — repos you own, plus repos you've recently contributed to (discovered from your public events, ~last 90 days). Contributed repos have a green "contributed" badge and a subtle green left border. Edges connect repos that depend on each other.
- **Contributors tab** — everyone you've co-contributed with (both committed to the same repo), shown as a network.

---

## Local usage

```bash
# Install dependencies
pip install requests python-dotenv

# Run (token optional for public repos, but recommended to avoid rate limits)
GITHUB_TOKEN=ghp_xxxx python fetch.py <your-username>

# Or use a .env file
echo 'GITHUB_TOKEN=ghp_xxxx' >> .env
echo 'GITHUB_USER=your-username' >> .env
python fetch.py
```

Then serve the `docs/` folder locally:

```bash
python3 -m http.server 8000 --directory docs
# Open http://localhost:8000
```

## Creating a GitHub Token

A token is optional for public repos but strongly recommended (unauthenticated requests are limited to 60/hour).

1. Go to https://github.com/settings/tokens
2. Click **Generate new token (classic)**
3. Grant scope: `repo` (includes private repos) or `public_repo` (public only)
4. Copy the token

## Options

```
python fetch.py <username> [--token TOKEN] [--output PATH]
                           [--skip-forks] [--skip-archived] [--skip-private]
                           [--no-contributed] [--limit N]
```

| Flag | Default | Description |
|---|---|---|
| `--token` | `$GITHUB_TOKEN` | GitHub personal access token |
| `--output` | `docs/data.json` | Output path for the JSON data |
| `--skip-forks` | false | Skip forked repos (owned repos only) |
| `--skip-archived` | false | Skip archived repos (owned repos only) |
| `--skip-private` | false | Skip private repos (owned repos only) |
| `--no-contributed` | false | Only show owned repos, skip contributed-to repos |
| `--limit N` | all | Process at most N repos total (useful for testing) |

## Notes on contributed repos

Contributed repos are discovered from your recent public GitHub events (last ~300 events, roughly 90 days of activity). Repos you contributed to before that window may not appear. Only repos not owned by you are included.
