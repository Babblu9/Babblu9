# Setup — your GitHub profile README

This folder is your **profile repo**. Once pushed to GitHub, the README appears on
your profile at https://github.com/Babblu9.

## 1. Create the repo

GitHub requires the repo name to **exactly match your username**.

```bash
# from this folder (Babblu9/)
git init
git add .
git commit -m "feat: profile README + metrics workflow"
git branch -M main

# create the repo on GitHub (must be PUBLIC, named exactly Babblu9)
gh repo create Babblu9 --public --source=. --remote=origin --push
```

If you don't have `gh` installed, create the repo manually on github.com,
then:

```bash
git remote add origin https://github.com/Babblu9/Babblu9.git
git push -u origin main
```

## 2. Generate a Personal Access Token (required for metrics)

The lowlighter/metrics action needs a GitHub token with extra scopes
beyond the default `GITHUB_TOKEN` to render private contributions, followups,
and other plugins.

1. Go to https://github.com/settings/tokens/new
2. **Note**: `metrics-token`
3. **Expiration**: 90 days (or No expiration if you prefer)
4. **Select scopes**:
   - `public_repo`
   - `read:org` (for org membership/contributions)
   - `read:user`
   - `repo` (only if you want metrics from private repos too)
5. Click **Generate token** and copy the value.

## 3. Add the token as a repo secret

1. Go to https://github.com/Babblu9/Babblu9/settings/secrets/actions
2. Click **New repository secret**
3. **Name**: `METRICS_TOKEN`
4. **Secret**: paste the token from step 2
5. Click **Add secret**

## 4. (Optional) Create the production environment

The workflow uses `environment: production`. You can either:
- Create one at https://github.com/Babblu9/Babblu9/settings/environments
  and add `METRICS_TOKEN` there too, **or**
- Remove the `environment: production` lines from `.github/workflows/metrics.yml`
  if you don't want environment protection.

## 5. Trigger the workflow

```bash
gh workflow run "GitHub Metrics"
```

Or via the UI: go to the **Actions** tab → **GitHub Metrics** → **Run workflow**.

After ~2 minutes the action will commit these SVGs back to your repo:

- `github-metrics.svg`
- `metrics.plugin.languages.svg`
- `metrics.plugin.habits.svg`
- `metrics.plugin.activity.svg`
- `metrics.plugin.achievements.svg`
- `metrics.plugin.followup.svg`
- `metrics.plugin.topics.svg`
- `metrics.plugin.lines.svg`

The README references these SVGs, so they'll render automatically once they exist.

## 6. Personalize before pushing (optional)

Edit `README.md` to:
- Replace `youremail@example.com` with your real email
- Fill in the LinkedIn / X URLs (currently `#`)
- Adjust the "About me" object

## Refresh schedule

The workflow re-runs every 6 hours and on every push to `main`. You can trigger
it manually any time from the Actions tab.

## Troubleshooting

- **SVGs don't show up after push** → Wait for the first workflow run to finish,
  then refresh. Check the Actions tab for errors.
- **"Resource not accessible by integration"** → Your `METRICS_TOKEN` is missing
  scopes. Regenerate with the scopes listed in step 2.
- **Activity plugin shows nothing** → Make sure your token has `read:user` and
  the workflow has access to the events you want to track.
