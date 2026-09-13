# Deploying to domene.shop

The site is a set of static files. You build it locally, then upload the
contents of `_site/` to your web root on domene.shop over SSH/SCP.

Nothing is deleted on the server automatically — you only ever upload files.
If you rename or remove a page, delete the stale file on the server manually.

## 1. Build

```bash
npm run build
```

This regenerates the `_site/` folder. Everything you upload lives there.

## 2. What gets uploaded

After a build, `_site/` contains (roughly):

```
_site/
├── index.html                                # home (post list)
├── 404.html
├── feed.xml
├── sitemap.xml
├── robots.txt
├── favicon.svg
├── about-me/index.html
├── deploying-azure-amba-rules-with-github-actions/index.html
├── stop-maintaining-powershell-modules-software-baselines-with-azure-machine-configuration/index.html
├── tags-on-hybrid-machines-in-azure-gui-error/index.html
└── assets/
    ├── css/style.css
    └── images/…
```

List the exact files any time with:

```bash
find _site -type f | sort
```

## 3. Set your connection details

Copy the example config and fill in your values (this file is gitignored):

```bash
cp deploy.config.example deploy.config
# edit deploy.config, then:
source ./deploy.config
```

`deploy.config` defines `DEPLOY_USER`, `DEPLOY_HOST`, and `DEPLOY_PATH`
(your web root, e.g. `/home/<user>/www` — check your domene.shop account).

## 4. Upload

### Option A — SCP with an SSH key (recommended)

Uploads the whole built site:

```bash
scp -i ~/.ssh/id_ed25519 -r _site/. "$DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH/"
```

Or with literal placeholders instead of the config file:

```bash
scp -i ~/.ssh/id_ed25519 -r _site/. your-ssh-username@ssh.domene.shop:/home/your-ssh-username/www/
```

### Option B — SCP with password authentication

Omit `-i` and you'll be prompted for your password:

```bash
scp -o PubkeyAuthentication=no -r _site/. "$DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH/"
```

### Option C — rsync (faster re-uploads, only changed files)

`rsync` skips unchanged files. This example does **not** delete anything on
the server (no `--delete`):

```bash
# With SSH key:
rsync -avz -e "ssh -i ~/.ssh/id_ed25519" _site/ "$DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH/"

# With password (you'll be prompted):
rsync -avz -e "ssh -o PubkeyAuthentication=no" _site/ "$DEPLOY_USER@$DEPLOY_HOST:$DEPLOY_PATH/"
```

## Notes

- The trailing `/.` (scp) or trailing `/` on `_site/` (rsync) means "the
  contents of `_site`", so files land directly in your web root, not inside a
  `_site/` subfolder.
- Test one file first if you're unsure of the web-root path:
  `ssh "$DEPLOY_USER@$DEPLOY_HOST" 'ls -la'`.
