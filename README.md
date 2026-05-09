# Resume GitHub Pages Deployment

This repository publishes the static resume site for `resume.ziqguo.com` with GitHub Pages.

## GitHub Pages settings

1. Open the repository on GitHub.
2. Go to **Settings** > **Pages**.
3. Under **Build and deployment**, set **Source** to **GitHub Actions**.
4. Under **Custom domain**, enter `resume.ziqguo.com` and save.
5. After GitHub finishes checking DNS and issuing the certificate, enable **Enforce HTTPS**.

Use **GitHub Actions** instead of **Deploy from a branch** because `.github/workflows/pages.yml` builds the exact `_site` artifact that should be published.

## Squarespace DNS settings

In Squarespace, update the custom DNS record for the `resume` subdomain:

| Type | Name | Data |
| --- | --- | --- |
| `CNAME` | `resume` | `gzquse.github.io` |

Remove or edit any old `resume` record that points to Azure Static Web Apps, such as an `azurestaticapps.net` target. Do not include `https://`, a path, or the repository name in the DNS value.

Keep unrelated records such as `_domainconnect`, TXT verification records, and other subdomains unless you know they should change.

## Deploying

The GitHub Actions workflow deploys automatically when changes are pushed to `main`. It can also be started manually from **Actions** > **Deploy GitHub Pages** > **Run workflow**.

The workflow publishes only these files:

- `index.html`
- `CV_ziqing.pdf`
- `headshot.jpeg`
- `CNAME`
- `.nojekyll`
- `404.html`

`index.html` redirects visitors to `/CV_ziqing.pdf`, and `404.html` gives GitHub Pages a fallback that redirects unknown routes to the same PDF.

## Verify after deployment

DNS may take time to update. The Squarespace TTL is commonly several hours.

Run these checks after the workflow succeeds:

```bash
dig +short resume.ziqguo.com CNAME
curl -I https://resume.ziqguo.com/
curl -I https://resume.ziqguo.com/CV_ziqing.pdf
```

Expected DNS result:

```text
gzquse.github.io.
```

Expected site behavior: `https://resume.ziqguo.com/` loads the GitHub Pages site and redirects to `https://resume.ziqguo.com/CV_ziqing.pdf`.
