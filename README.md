# demo-project

Single-page site for a health drink brand (HTML, CSS, JS).

## View locally

Open `index.html` in your browser, or serve the folder (for example `python -m http.server 8080`).

## Deploy to Hostinger (GitHub Actions)

Pushes to **`main`** run [`.github/workflows/deploy-hostinger.yml`](.github/workflows/deploy-hostinger.yml) and upload the project over **FTP** to your Hostinger account.

Repository: [wakralabconsulting/demo-project](https://github.com/wakralabconsulting/demo-project.git)

### 1. FTP details in Hostinger

In **hPanel**: **Websites** → your site → **Files** → **FTP Accounts** (or **Advanced** → **FTP**). Create or use an FTP user and note:

- **FTP hostname** (often `ftp.yourdomain.com` or the hostname shown in the panel)
- **Username** and **password**
- **Directory**: for the main domain this is usually `public_html`. Addon domains often use `domains/yourdomain.com/public_html`.

### 2. GitHub Actions secrets

In GitHub: **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Secret | Description |
|--------|-------------|
| `HOSTINGER_FTP_HOST` | FTP server hostname (no `ftp://` prefix). |
| `HOSTINGER_FTP_USERNAME` | FTP username. |
| `HOSTINGER_FTP_PASSWORD` | FTP password. |
| `HOSTINGER_FTP_REMOTE_DIR` | *Optional.* Remote folder path. If omitted, defaults to `public_html/`. Use your panel path if files must go elsewhere (must match what you see relative to the FTP login root). |

### 3. Push to `main`

```bash
git push origin main
```

The **Actions** tab shows logs for each deployment.

### FTPS or connection issues

If plain FTP is blocked, edit the workflow’s “Deploy files over FTP” step and set the action’s `protocol` input to `ftps` or `ftps-legacy` per [FTP-Deploy-Action](https://github.com/SamKirkland/FTP-Deploy-Action) docs. This workflow targets **FTP**; VPS **SSH/rsync** deployments need a different workflow.