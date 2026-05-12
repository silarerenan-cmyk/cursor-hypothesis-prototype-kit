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
