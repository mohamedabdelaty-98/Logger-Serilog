# Deployment and Access Control

## Important: private repository does not guarantee a private website

GitHub Pages sites may be publicly accessible even when their source repository is private. Restricting a project Pages site to repository readers requires an eligible GitHub Enterprise Cloud organization and the site visibility setting **Private**. Check the plan and the Pages visibility setting before deployment. GitHub documents [Pages site visibility](https://docs.github.com/en/enterprise-cloud@latest/pages/getting-started-with-github-pages/changing-the-visibility-of-your-github-pages-site) and warns that sites are public by default unless private access control is configured.

This guide is intended as internal team documentation. The workflow therefore fails closed: the build can run, but deployment is skipped unless repository variable `PUBLISH_PRIVATE_PAGES` is exactly `true`.

## Enable private Pages deployment

After creating the private repository under an eligible organization:

1. Open repository **Settings → Pages** and choose **GitHub Actions** as the build and deployment source.
2. Confirm the GitHub Pages visibility is **Private** and that the plan supports private site access control.
3. Under **Settings → Secrets and variables → Actions → Variables**, create repository variable `PUBLISH_PRIVATE_PAGES` with value `true`.
4. Push to `main` or run the workflow manually. The workflow builds the site, uploads its artifact, and deploys it. Confirm both the Actions run and the Pages URL in repository settings.

If the organization cannot set Pages visibility to Private, do not enable this workflow for internal material. Choose an approved internal documentation host or explicitly decide that the website content is safe to make publicly accessible.

## Update and publish

Edit a chapter in `docs/logging/`, update the navigation in `mkdocs.yml` when adding a page, preview locally with `mkdocs serve`, then commit and push to `main`. GitHub Actions rebuilds the site and deploys it when the private Pages gate is enabled.

## Local build

Install Python 3.12 or a compatible supported Python version, then run:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
mkdocs build --strict
```
