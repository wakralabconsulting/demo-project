# demo-project

Single-page site for a health drink brand (HTML, CSS, JS).

## View locally

Open `index.html` in your browser, or serve the folder (for example `python -m http.server 8080`).

## Deploy to Hostinger (GitHub Actions)

Pushes to **`main`** run [`.github/workflows/deploy-hostinger.yml`](.github/workflows/deploy-hostinger.yml) and upload the site over **SSH (SCP)** using [appleboy/scp-action](https://github.com/appleboy/scp-action).

Repository: [wakralabconsulting/demo-project](https://github.com/wakralabconsulting/demo-project.git)

### Security

- Store **only** host, username, password, and paths in **GitHub Actions secrets**, never in git or chat.
- If a server password was ever shared in a message or ticket, **change it in Hostinger hPanel** (SSH details → **Change**) and update the `HOSTINGER_SSH_PASSWORD` secret.
- For production, consider switching to **SSH key** authentication (generate a deploy key, add the public key in Hostinger, store the private key in a secret) instead of a password.

### 1. SSH details in Hostinger

In **hPanel**: **Websites** → your site → **Advanced** → **SSH Access** (or **SSH details**). Note **IP**, **Port** (often `65002` on shared hosting), **Username**, and set a strong password.

Confirm the folder that should receive the website files (usually `public_html` for the main domain). In **File Manager**, open that folder and note the full path if it differs from `/home/<username>/public_html` (addon domains often live under `domains/<yourdomain>/public_html`).

### 2. GitHub Actions secrets

GitHub: **Settings** → **Secrets and variables** → **Actions** → **New repository secret**.

| Secret | Required | Description |
|--------|----------|-------------|
| `HOSTINGER_SSH_HOST` | Yes | Server IP or hostname from **SSH details** in hPanel. |
| `HOSTINGER_SSH_USERNAME` | Yes | SSH username from hPanel. |
| `HOSTINGER_SSH_PASSWORD` | Yes | SSH password (rotate if it was ever exposed). |
| `HOSTINGER_SSH_PORT` | No | SSH port. If omitted, the workflow uses **65002** (common on Hostinger). |
| `HOSTINGER_SSH_TARGET` | No | Absolute path to the web root on the server, ending with `/` recommended. If omitted, defaults to `/home/<HOSTINGER_SSH_USERNAME>/public_html/`. |

### 3. Push to `main`

```bash
git push origin main
```

Open the **Actions** tab to see deploy logs.

### Troubleshooting

- **Auth failed:** Confirm username, password, and port; reset the SSH password in hPanel and update the secret.
- **Wrong folder / 404:** Set `HOSTINGER_SSH_TARGET` to the exact directory Hostinger uses for that site’s `public_html`.
- **Prefer key-based deploy:** Use `appleboy/scp-action` with `key` instead of `password` (see the action’s README).
