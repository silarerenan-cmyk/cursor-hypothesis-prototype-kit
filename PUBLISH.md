# Publishing this kit to GitHub

GitHub CLI (`gh`) is optional. These steps use plain Git.

## 1. Create an empty repository on GitHub

1. Open https://github.com/new  
2. Owner: your org or user (e.g. `silarerenan-cmyk`)  
3. Repository name: `cursor-hypothesis-prototype-kit` (or another name; if you change it, update the remote URL in step 2)  
4. **Do not** add a README, `.gitignore`, or license (avoids merge conflicts).  
5. Create repository.

## 2. Add remote and push

From this folder (`cursor-hypothesis-prototype-kit/`):

```powershell
git remote add origin https://github.com/<YOUR_USER_OR_ORG>/cursor-hypothesis-prototype-kit.git
git branch -M main
git push -u origin main
```

If `origin` already exists with a wrong URL:

```powershell
git remote set-url origin https://github.com/<YOUR_USER_OR_ORG>/cursor-hypothesis-prototype-kit.git
git push -u origin main
```

Use Git Credential Manager or a PAT when prompted.

## 3. Share with coworkers

Send them the HTTPS clone URL, for example:

`https://github.com/<YOUR_USER_OR_ORG>/cursor-hypothesis-prototype-kit.git`

They follow [README.md](README.md) **Install**.

## 4. Enable GitHub Pages (one-time; fixes failed `deploy` workflow)

The workflow [.github/workflows/deploy-pages.yml](.github/workflows/deploy-pages.yml) uses **GitHub Actions** as the Pages publisher. If Pages is off or still set to “Deploy from a branch”, the job **Configure GitHub Pages** fails with *Get Pages site … Not Found*.

Do this once on the GitHub repo:

1. Open **Settings** → **Pages** (under “Code and automation”).
2. Under **Build and deployment**, set **Source** to **GitHub Actions** (not “Deploy from a branch”).
3. Save if prompted.
4. Go to **Actions** → workflow **Deploy to GitHub Pages** → open the latest run → **Re-run all jobs** (or push any commit to `main`).

After that, the site URL is shown on the successful run (and under **Settings** → **Pages**). Prototypes are served under paths like `https://<user>.github.io/<repo>/prototypes/<id>/`.
