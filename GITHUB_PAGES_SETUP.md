# GitHub Pages Setup Instructions

To enable GitHub Pages for this repository, follow these steps:

## 1. Enable GitHub Pages

1. Go to your repository on GitHub
2. Click on **Settings** → **Pages** (in the left sidebar)
3. Under **Source**, select:
   - Source: **GitHub Actions**
   
That's it! The GitHub Actions workflow will automatically deploy the documentation.

## 2. First Deployment

Once you merge this PR to the `main` branch:

1. The GitHub Actions workflow will automatically run
2. It will build the MkDocs site
3. Deploy it to GitHub Pages
4. Your site will be available at: `https://faxg.github.io/trusted-research-environments/`

## 3. Monitoring Deployments

- Go to the **Actions** tab in your repository
- Look for the "Deploy MkDocs to GitHub Pages" workflow
- You can see the status of deployments and troubleshoot any issues

## 4. Manual Deployment

You can also manually trigger a deployment:

1. Go to **Actions** tab
2. Select "Deploy MkDocs to GitHub Pages" workflow
3. Click "Run workflow"
4. Select the branch and click "Run workflow"

## Notes

- The workflow is configured to run automatically on every push to `main` branch
- The deployment uses the new GitHub Pages Actions deployment method
- All documentation is built from the `docs/` directory using `mkdocs.yml` configuration
