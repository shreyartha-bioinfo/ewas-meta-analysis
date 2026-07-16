# How to Upload This Repository to GitHub

This guide walks you through creating a GitHub repository and pushing the
`ewas-meta-analysis` project to it. Two methods are covered: the **GitHub
website** (no tools needed) and the **command line with git** (recommended).

---

## Method 1: Command line with git (recommended)

### Prerequisites

- [Git](https://git-scm.com/downloads) installed on your machine
- A [GitHub](https://github.com) account

### Step 1: Initialize the local repository

Download or copy the `github_repo` folder to your machine, then:

```bash
cd github_repo
git init
git add .
git commit -m "Initial commit: fast EWAS meta-analysis tutorial with metagen"
```

### Step 2: Create an empty repository on GitHub

1. Go to [github.com/new](https://github.com/new)
2. **Repository name**: `ewas-meta-analysis`
3. **Description**: `Inverse-variance-weighted EWAS meta-analysis with vectorized metagen — 9000x speedup`
4. Set to **Public** or **Private** (your choice)
5. **Do NOT** check "Add a README" or "Add .gitignore" or "Choose a license" — these already exist in your local repo
6. Click **Create repository**

### Step 3: Connect and push

GitHub will show you commands like these. Copy the ones for "…or push an
existing repository from the command line":

```bash
git remote add origin https://github.com/<your-username>/ewas-meta-analysis.git
git branch -M main
git push -u origin main
```

Replace `<your-username>` with your GitHub username.

### Step 4: Authenticate

When you push, Git will ask for credentials. The easiest way is a **Personal
Access Token**:

1. Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)
2. Generate new token (classic) → check the `repo` scope
3. Copy the token
4. When Git prompts for your password, paste the token

Alternatively, use the GitHub CLI:

```bash
# Install GitHub CLI: https://cli.github.com/
gh auth login
git push -u origin main
```

### Step 5: Enable GitHub Pages (optional)

The included GitHub Actions workflow (`.github/workflows/render-tutorial.yml`)
automatically renders the Rmd to HTML on every push and can deploy it to
GitHub Pages.

1. Go to your repo on GitHub → **Settings** → **Pages**
2. Under **Build and deployment**, set **Source** to **GitHub Actions**
3. On your next push to `main`, the workflow will render the tutorial and
   deploy the HTML to:
   ```
   https://<your-username>.github.io/ewas-meta-analysis/ewas_metaanalysis_tutorial.html
   ```

---

## Method 2: GitHub website (no git CLI needed)

### Step 1: Create the repository

1. Go to [github.com/new](https://github.com/new)
2. **Repository name**: `ewas-meta-analysis`
3. **Do NOT** check "Add a README", ".gitignore", or "license"
4. Click **Create repository**

### Step 2: Upload files

1. On the empty repo page, click **uploading an existing file**
2. Drag and drop ALL files from the `github_repo` folder:
   - `ewas_metaanalysis_tutorial.Rmd`
   - `README.md`
   - `LICENSE`
   - `.gitignore`
   - `data/README.md`
   - `results/README.md`
   - `results/ewas_metaanalysis_tutorial.html`
3. For the `.github/workflows/render-tutorial.yml` file:
   - GitHub's web uploader doesn't handle nested folders well
   - Click **Create new file** instead
   - Type `.github/workflows/render-tutorial.yml` as the filename
   - Paste the contents of `render-tutorial.yml`
   - Commit
4. Add a commit message: `Initial commit: fast EWAS meta-analysis tutorial`
5. Click **Commit changes**

### Step 3: Enable GitHub Pages (optional)

Same as Method 1, Step 5 above.

---

## Verifying the upload

After pushing, your repo should look like this:

```
ewas-meta-analysis/
├── .github/
│   └── workflows/
│       └── render-tutorial.yml
├── .gitignore
├── LICENSE
├── README.md
├── ewas_metaanalysis_tutorial.Rmd
├── data/
│   └── README.md
└── results/
    ├── README.md
    └── ewas_metaanalysis_tutorial.html
```

Check that:
- The README renders on the repo home page
- The `.github/workflows/` folder is present (click on `.github` to verify)
- The Actions tab shows a "Render tutorial" workflow running

---

## Updating the repository later

After making changes to the Rmd or other files:

```bash
git add .
git commit -m "Describe your changes here"
git push
```

The GitHub Actions workflow will automatically re-render the tutorial and
update the GitHub Pages deployment.
