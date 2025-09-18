# Summary
1. Define environment variables/secrets in `Setting/Environment`  
2. Create deploy file at `​.github/workflows/deploy.yml`

# Define environment variables/secrets 
Path:
```
repo > Settings > Secrets and variables > Actions > New repository secret
```
Cli:
```
gh secret set VITE_API_KEY --body "your_value" --repo YOUR_USER/REPO
```

# Create deploy file

```
name: Deploy My Project

on:
  push:
    branches:
      - main # trigger branch

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: write
      pages: write
      id-token: write
    steps:
      - name: Checkout code
        uses: actions/checkout@v3
        with:
          persist-credentials: false  # use the latest

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 22 # for vite.js

      - name: Install dependencies
        run: yarn install --frozen-lockfile # I prefer yarn

      - name: Build
        run: yarn build
        env:
          VITE_API_KEY: ${{ secrets.VITE_API_KEY }} # more variables/secrets here

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist # change it if you like

```

When pushing new commit on `main`, this deploy job is been triggered. Check the workdflow on `Actions` tab in your repo.